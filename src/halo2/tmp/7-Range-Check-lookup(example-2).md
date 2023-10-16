> author: [@Demian](https://github.com/Demian101)
> 
> references: https://learn.0xparc.org/materials/halo2

[TOC]
### Overview

上一章节使用的 `(v) * (1 - v) * (2 - v) * ... * (R - 1 - v)`，本章使用了 **Lookup Table** 来执行范围检查。

连乘表达式的问题：如果数字 “R” 很大，那么多项式的次数将非常高，这会增加电路的成本，所以现在我们必须稍微改变我们的布局：使用查找表进行范围检查：当您尝试检查更大的范围并且希望将约束的度数限制保持在较低水平时非常有用。

（if you have a vary large `R`, then polynomial is going to be very high degree and that will increase the cost of your circuit so now we have to change our layout a little bit. 
range check with Lookup table: useful when you're trying to check a larger range and you want to keep the degree bound of your constraints low.）

文件架构：
```rust
├── range_check
│   ├── example2
│   │   └── table.rs  // lookup table
│   ├── example2.rs   // main config
```

调用链：
![](imgs/RangeCheck/Pasted%20image%2020230917215114.png)

impl relationship：
![](imgs/RangeCheck/Pasted%20image%2020230917220003.png)

Draw the circuit：
![](imgs/RangeCheck/Pasted%20image%2020230917171123.png)


![[Range-Check-impl-relationship.excalidraw]]
### lookup table - table.rs

`src/range_check/example2/table.rs`
##### struct RangeTableConfig

```rust
use std::marker::PhantomData;
use halo2_proofs::{
    arithmetic::FieldExt,
    circuit::{Layouter, Value},
    plonk::{ConstraintSystem, Error, TableColumn},
};

// pub(super) 仅当前模块的父模块中可见，但不对外公开
/// A lookup table of values from 0..RANGE.
#[derive(Debug, Clone)]
pub(super) struct RangeTableConfig<F: FieldExt, const RANGE: usize> {
    pub(super) value: TableColumn, 
    // 这个 struct 中存在一个与类型 F 相关的关联，即使 struct 自身并没有实际使用这个类型
    _marker: PhantomData<F>,
}
```

##### fn configure()
 - 泛型常量参数 `const N: usize`，来处理不同大小和类型的数组
 - `meta.lookup_table_column();`  实际会返回一个 `fixed_column`

```rust
impl<F: FieldExt, const RANGE: usize> RangeTableConfig<F, RANGE> {
    pub(super) fn configure(meta: &mut ConstraintSystem<F>) -> Self {
        let value = meta.lookup_table_column(); // Define a  *Lookup column*
        Self {
            value,
            _marker: PhantomData,
        }
    }
    // fn load ..
```


`halo2_proofs/src/plonk/circuit.rs` :
```rust
    /// Allocates a new fixed column that can be used in a lookup table.
    pub fn lookup_table_column(&mut self) -> TableColumn {
        TableColumn {
            inner: self.fixed_column(),
        }
    }
```
##### fn load()

 - `load()` assign the values to our fixed table
 - `fn load()` 是一个在 `RangeTableConfig` 结构体上定义的方法，它用于 load (赋值) 一个范围检查表。在这段代码中，范围检查表是一个  `0 ~ RANGE-1` 的 table 

```rust
// load function assign the values to our fixed table
// This action is performed at key gen time
pub(super) fn load(&self, layouter: &mut impl Layouter<F>) -> Result<(), Error> {
  // firstly, for some RANGE we want to load all the values and assign it to the lookup table
  // assign_table is a special api that only works for lookup tables
  layouter.assign_table (
    || "load range-check table",
    |mut table| {
      // from row_0 to row_{RANGE-1}
      let mut offset = 0;
      for value in 0..RANGE {
        table.assign_cell(
          || "num_bits",
          self.value,
          offset,  // row num
          || Value::known(F::from(value as u64)), // assigned value
        )?;
        offset += 1;  // 循环向下赋值, 直到填满 RANGE 所需的所有列
      }

      Ok(()) // return empty tuple (∵ Result<(), Error>)
    },
  )
}
```

### main - example2.rs

#### Overview

This helper checks that the value witnessed in a given cell is within a given range.

Depending on the range, this helper uses either a range-check expression (for small ranges), or a lookup (for large ranges).
- above a certain `RANGE` we use a **lookup** argument , like $v_1$ , enabled `q_range_check` & disabled `q_looup` Selector
- below that `RANGE` we stick to the **simple** expression, like $v_1$ , enabled `q_looup` & disabled `q_range_check` Selector 

```bash
  value   |  q_range_check  |  q_lookup  |  table_value  |
------------------------------------------------------------
   v_0    |       1         |     0      |       0       |
   v_1    |       0         |     1      |       1       |
```

 - 在一个比较小的特定范围里，使用 range-check 连乘 expression
 - 对于比较大的查找范围，使用 Lookup Table 查找表

#### structs

 - `RangeConstrained` : 由 `RangeCheckConfig` 生成的电路中的范围约束值 (range-constrained value)，即用来表示一个范围受限的值。
 - `RangeCheckConfig` :  main 电路的 Chip Config，用于配置和执行范围检查
	 - `q_range_check` : Selector used for *small* RANGE number.
	 - `q_lookup` : Selector used for *large* RANGE number.
	 - value：an Advice column 用于存储 Private value without revealing it.
	 - `table: RangeTableConfig<F, LOOKUP_RANGE>` : Lookup table

```rust
#[derive(Debug, Clone)]
/// A range-constrained value in the circuit produced by the RangeCheckConfig.
struct RangeConstrained<F: FieldExt, const RANGE: usize>(AssignedCell<Assigned<F>, F>);

#[derive(Debug, Clone)]
struct RangeCheckConfig<F: FieldExt, const RANGE: usize, const LOOKUP_RANGE: usize> {
    q_range_check: Selector,
    q_lookup: Selector,
    value: Column<Advice>,
    table: RangeTableConfig<F, LOOKUP_RANGE>, // Lookup table
}
```
#### impl RangeCheckConfig

##### fn configure()

1. 在 `query_selector` 即查询 Selector 时，无需指定显式 rotation，因为 selector always get queried at the current row .
2. 在 `query_advice` 即查询 Advice 时，因为 advice col 是相对于 Selector 偏移量(Selector offset)进行查询的，所以我们需要指定 relative rotation.
3. 不像之前我们在 `configure()` 函数内部声明 Advice column：

```rust
pub fn configure(){
  let col_a = meta.advice_column();
  meta.enable_equality(col_a); // 在 `configure()` 内部声明 Advice column：
  /// ...
}
```

在本 `configure()` 中，我们传入 `value: Column<Advice>)` ，这样可以更方便地 shared across multiple config ：
```rust
// 在 impl Circuit for MyCircuit 中调用：
fn configure(meta: &mut ConstraintSystem<F>) -> Self::Config {
	let value = meta.advice_column(); // 声明 value 这个 Advice column
	RangeCheckConfig::configure(meta, value) // 传入 value 这个 Advice column
}
```

在 `meta.create_gate` 过程中，创建了 1 个约束门，赋值了 1 个 `special fixed colum` ：
1. `meta.create_gate("range check"..` 门：使用 range-check 连乘 expression 限制小范围数字
2. `meta.lookup(|meta| { ..` 门：Fix 查找表，在一个更大的范围内进行约束 

```rust
// Write the gate for our range check Config
// It's good practive to pass advice columns to the config (rather than creating it within the config)
// because these are very likely to be shared across multiple config
impl<F: FieldExt, const RANGE: usize, const LOOKUP_RANGE: usize>
  RangeCheckConfig<F, RANGE, LOOKUP_RANGE>
{
  // Remember that the configuration happen at keygen time.
  pub fn configure(meta: &mut ConstraintSystem<F>, value: Column<Advice>) -> Self {
    // Toggles the range_check constraint
    let q_range_check = meta.selector();
    // Toggles the lookup argument
    let q_lookup = meta.complex_selector(); // for lookup table
    // configure a lookup table. and **pass it to config**
    let table = RangeTableConfig::configure(meta);

    // later we will return this config.
    let config = Self {
      q_range_check,
      q_lookup,
      value,
      table: table.clone()
    }; 

    // 1. range-check gate
    meta.create_gate("range check", |meta| {
      let q = meta.query_selector(q_range_check);

      // note that we don't need to specify the rotation when querying the `selctor`
      // That's because the selector always get queried at the current row .
      // While the `advice columns` get queried relatively to the selector offset, so we need to specify the relative rotation
      // 然而 advice col 是相对于选择器偏移量(Selector offset)进行查询的，所以我们需要指定 relative rotation.
      let value = meta.query_advice(value, Rotation::cur());

      // Given a range R and a value v, returns the multiplication expression
      //  (v) * (1 - v) * (2 - v) * ... * (R - 1 - v)
      let range_check = |range: usize, value: Expression<F>| {
        assert!(range > 0);
        (1..range).fold(value.clone(), |expr, i| {
          expr * (Expression::Constant(F::from(i as u64)) - value.clone())
        })
      };
      // like the previously using "vec![s * (a + b - c)]",
      // multiplies the specified constraint by the selector, api 将指定的约束 × Selector
      Constraints::with_selector(q, [("range check", range_check(RANGE, value))])
    });
    
    // 2. Lookup Gate  - range-check using lookup argument
    // 这个查找表将会在后面的范围检查中使用，以便在某些情况下使用查找表, 而不是表达式来执行范围检查。
    meta.lookup(|meta| {
      let q_lookup = meta.query_selector(q_lookup);
      let value = meta.query_advice(value, Rotation::cur());

      vec![(q_lookup * value, table.value)]
    });

    config
  }
```

如何协同？

```rust
// 1. 定义 Circuit
let circuit = MyCircuit::<Fp, RANGE, LOOKUP_RANGE> {
	simple_value: Value::known(Fp::from(i as u64).into()),
	lookup_value: Value::known(Fp::from(j as u64).into()),
};

// 2. 将 circuit 传入 MockProver::run()
// 3. run() 中  (halo2_proofs/src/dev.rs ) ：
// 3.1  ConcreteCircuit::configure
// 3.2  ConcreteCircuit::FloorPlanner::synthesize
impl<F: Field + Ord> MockProver<F> {
    /// Runs a synthetic keygen-and-prove operation on the given circuit,  
    /// collecting data about the constraints and their assignments.
    pub fn run<>(){
        let mut cs = ConstraintSystem::default();
        let config = ConcreteCircuit::configure(&mut cs);
        let cs = cs;
        // ...
        ConcreteCircuit::FloorPlanner::synthesize(&mut prover, circuit, config, constants)?;
    }
```

在 `synthesize` 中：

##### fn assign_simple()

used for *small* value. We pass `value` and assign it on the offset.

```rust
// pass `value` and assign it on the offset.
pub fn assign_simple(
  &self,
  mut layouter: impl Layouter<F>,
  value: Value<Assigned<F>>,
) -> Result<RangeConstrained<F, RANGE>, Error> {
  layouter.assign_region(
    || "Assign value for simple range check",
    |mut region| {
      let offset = 0;

      // Enable q_range_check Selector.
      self.q_range_check.enable(&mut region, offset)?;

      // Assign `value` 
      region
        .assign_advice(
          || "value", 
          self.value,  // current col ?
          offset, 
          || value
        ).map(RangeConstrained) // 将结果转化为 RangeConstrained 类型
    },
  )
}
```

##### fn assign_lookup()

```rust
pub fn assign_lookup(
  &self,
  mut layouter: impl Layouter<F>,
  value: Value<Assigned<F>>,
) -> Result<RangeConstrained<F, LOOKUP_RANGE>, Error> {
  layouter.assign_region(
    || "Assign value for lookup range check",
    |mut region| {
      let offset = 0;

      // Enable q_lookup, 告诉约束系统在该区域应用这个选择器
      self.q_lookup.enable(&mut region, offset)?;

      // Assign value
      region
        .assign_advice(|| "value", self.value, offset, || value)
        .map(RangeConstrained)
      // assign_advice() 将 advice col 与值 value 关联，
      // 并将结果封装在 RangeConstrained struct 中
    },
  )}
```

`assign_simple` & `assign_lookup` 这 2 个函数的区别：

```rust
1. 泛型常量
 - RANGE
 - LOOKUP_RANGE
2. Selector enabled:
 - q_range_check // for *small* RANGE number.
 - q_lookup      // for *large* RANGE number.

region.assign_advic 部分是一样的
```

### Test Lookup table

```rust
// [cfg(test)]是一个条件编译属性，意思是只有在执行 test 时，此模块代码才会被编译和执行
// 好处是，当你在普通的编译或生产环境下构建你的程序时，测试代码不会被包括进去，
// 从而减少了编译时间和生成的可执行文件的大小。
#[cfg(test)]
mod tests {
    use halo2_proofs::{
        circuit::floor_planner::V1,
        dev::{FailureLocation, MockProver, VerifyFailure},
        pasta::Fp,
        plonk::{Any, Circuit},
    };

    use super::*;
    //// .....
```
#### struct MyCircuit

`MyCircuit`  可以处理 2 种类型的值 : 
 - `value` :  这里的 value 的约束和赋值由 `assign_simple()` 完成
 - `lookup_value` : 它的约束和赋值由 `assign_lookup()` 完成

```rust
#[derive(Default)]
struct MyCircuit<F: FieldExt, const RANGE: usize, const LOOKUP_RANGE: usize> {
	value: Value<Assigned<F>>,
	lookup_value: Value<Assigned<F>>,
}

impl<F: FieldExt, const RANGE: usize, const LOOKUP_RANGE: usize> Circuit<F>
	for MyCircuit<F, RANGE, LOOKUP_RANGE> {
	type Config = RangeCheckConfig<F, RANGE, LOOKUP_RANGE>;
	type FloorPlanner = V1;

	fn without_witnesses(&self) -> Self { Self::default() }

	fn configure(meta: &mut ConstraintSystem<F>) -> Self::Config {
		let value = meta.advice_column();
		RangeCheckConfig::configure(meta, value)
	}
    // fn synthesize
```

#### fn synthesis()

```rust
fn synthesize(
    &self,
    config: Self::Config,
    mut layouter: impl Layouter<F>,
) -> Result<(), Error> {
    // load lookup table.
    config.table.load(&mut layouter)?;

    config.assign_simple(layouter.namespace(
        || "Assign simple(smaller) value"), 
        self.value
    )?;
    config.assign_lookup(
        layouter.namespace(|| "Assign lookup(larger) value"),
        self.lookup_value,
    )?;
    Ok(())
}  }
```


#### test_range_check_2

 - 在 `i, j` 的双重循环里: 
	 - `MyCircuit{ 1,10 }`
	 - `MyCircuit{ 7,16 }`
	 - `MyCircuit{ 5,100 }`
	 - `MyCircuit{ 7,255 }` ...

like : 
![](imgs/RangeCheck/Pasted%20image%2020230917215114.png)

```rust
#[test]
fn test_range_check_2() {
  // in every circuit, we opt to reserve the last few rows of each advice cols 
  // for random values which are blinding factors(for zk), so `k` is always larger.
  let k = 9;
  const RANGE: usize = 8; // 3-bit value
  const LOOKUP_RANGE: usize = 256; // 2^8, 8-bit value

  // Successful cases
  for i in 0..RANGE {
    for j in 0..LOOKUP_RANGE {
      // According to the <i, j> to construct different Circuit.
      //MyCircuit::<Fp,.. ,..> : 指定 Constant 泛型的值.
      let circuit = MyCircuit::<Fp, RANGE, LOOKUP_RANGE> {
        simple_value: Value::known(Fp::from(i as u64).into()),
        lookup_value: Value::known(Fp::from(j as u64).into()),
      };

      let prover = MockProver::run(k, &circuit, vec![]).unwrap();
      prover.assert_satisfied();
    }
  }
```
### illustration

![](imgs/RangeCheck/Pasted%20image%2020230917171123.png)

### usage

```bash
cargo test -- --nocapture test_range_check_2

# Draw
cargo test --release --all-features xxx
```

 - the white column is the instance column, 
 - the pink one is the advice and 
 - the purple one is the selector.
 - the green part shows the cells that have been assigned
      - light green : selector not used.


### References : 
 - https://github.com/enricobottazzi/halo2-intro/blob/master/src/range_check/example5/table.rs
 - [Jason Morton halo2 codes](https://github.com/jasonmorton/halo2-examples/blob/master/src/fibonacci/example1.rs)
 - [ZCash halo2 books](https://zcash.github.io/halo2/user/simple-example.html#define-a-chip-implementation)
 - [trapdoor-tech halo2 book](https://trapdoor-tech.github.io/halo2-book-chinese/user/simple-example.html)
 - [icemelon/HaiCheng Shen](https://github.com/icemelon/halo2-examples/blob/master/src/fibonacci/example3.rs)
 - [0xPARC halo2](https://learn.0xparc.org/)
