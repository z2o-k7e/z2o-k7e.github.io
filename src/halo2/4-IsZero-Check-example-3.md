> author: [@Demian](https://github.com/Demian101)
> 
> references: https://learn.0xparc.org/materials/halo2

[TOC]
### Goal

We want Prove that : `f(a, b, c) = if a == b {c} else {a - b}` 

> 证明某人知道三个数字  a、b 和  c，使得当 `a == b` 时，输出为 c，否则输出为 `a - b`，而无需揭示a、b 和 c 的实际值。

how to describe it ?   Firstly, let's dive into the `Iszero` Chip

这一部分主要展示了如何复用另外一个电路来辅助电路设计

### Overview

```bash
$ tree
├── fibonacci
│   └── example3.rs
├── is_zero.rs
```

### Iszero Chip
#### structs

```rust
#[derive(Clone, Debug)]
pub struct IsZeroConfig<F> {
    pub value_inv: Column<Advice>,
    pub is_zero_expr: Expression<F>,
}

impl<F: FieldExt> IsZeroConfig<F> {
    pub fn expr(&self) -> Expression<F> {
        self.is_zero_expr.clone()
    }
}

pub struct IsZeroChip<F: FieldExt> {
    config: IsZeroConfig<F>,
}
```

#### impl IsZeroChip { ..

##### configure 

传入参数：
- `q_enable, value` 都接收一个闭包，将执行的**时机**和执行的**具体内容**进行解耦。也就是说，`configure` 方法可以决定何时执行 `value`，而调用者可以决定执行 `value` 时应该做什么
- 闭包可以捕获其环境，这意味着每次传入不同的闭包，`configure` 函数的行为都可能不同。这为函数调用者提供了更大的灵活性。

注意 `AssignedCell` 和 `VirtualCells` 的区别（读下 Source code）

```rust
impl<F: FieldExt> IsZeroChip<F> {
    pub fn construct(config: IsZeroConfig<F>) -> Self {
        IsZeroChip { config }
    }

    pub fn configure(
        meta: &mut ConstraintSystem<F>,
        q_enable: impl FnOnce(&mut VirtualCells<'_, F>) -> Expression<F>,
        value: impl FnOnce(&mut VirtualCells<'_, F>) -> Expression<F>,
        value_inv: Column<Advice>,
    ) -> IsZeroConfig<F> {
        let mut is_zero_expr = Expression::Constant(F::zero());

        meta.create_gate("is_zero", |meta| {
            //
            // valid | val |  val_inv |  1 - val * val_inv | val * (1 - val * val_inv)
            // ------+-----+----------+--------------------+-------------------
            //  yes  |  x  |    1/x   |        0           |   0
            //  no   |  x  |    0     |        1           |   x
            //  yes  |  0  |    0     |        1           |   0
            //  yes  |  0  |    y     |        1           |   0

            //
            let value = value(meta);
            let q_enable = q_enable(meta);
            let value_inv = meta.query_advice(value_inv, Rotation::cur());

            is_zero_expr = Expression::Constant(F::one()) - value.clone() * value_inv;
            vec![q_enable * value * is_zero_expr.clone()]  // gate's constraints
        });

        IsZeroConfig {
            value_inv,
            is_zero_expr,
        }
    }
```

configure defines the logic for the "is-zero" gate. It uses the following table to guide the logic: 

```bash
valid | val |  val_inv |  1 - val * val_inv | val * (1 - val * val_inv)
------+-----+----------+--------------------+-------------------       
 yes  |  x  |    1/x   |        0           |   0                      
 no   |  x  |    0     |        1           |   x                      
 yes  |  0  |    0     |        1           |   0                      
 yes  |  0  |    y     |        1           |   0                      
```

第 `1 / 3 / 4` 行涉及到的约束不需要通过 `q_enable` 即可完成, 但是考虑第二行所涉及到的情况 : 
 - 如果 $P$  是个 malicious Prover, 他提供了 `val == x` 和 `val_inv == 0` , 此时仅靠 `is_zero_expr`  是无法分辨的 (这个 case 里 assign 函数会直接分配 `self.config.value_inv`  i.e.  $\textcolor{red}{0}$  即认为这个值是 $\textcolor{red}{0}$ 
 - 但是添加了 `vec![q_enable * value * is_zero_expr.clone()]`  约束就不一样了 , 约束强制要求 `val * is_zero_expr` i.e.  `val * ( 1 - val * val_inv)` 必须为 0 , 从而解决了这种 malicious situation
 - 如果 malicious $P$  提供了这种 Witness, 将不会通过约束校验, 也就不会生成该 proof
 - 只有 $P$  提供了符合约束的 Witness, `val_inv`  才会被赋值给 `val_inv` column


The gate ensures that for valid rows:
- If the $value \neq 0$ , its inverse is computed such that their multiplication (`val * val_inv`) 's  results in 1.
- If the $value ==0$ , its inverse **can be any value**, but the result of their multiplication should be 0.

The gate equation is `q_enable * value * (1 - value * value_inv)`, which should be satisfied for the valid conditions.
- `assign()`: This method is used to assign the inverse of a value (if it exists) or zero to the specified advice column in the circuit.

```rust
is_zero_expr = Expression::Constant(F::one()) - value.clone() * value_inv;
```
 - i.e. `1 - val * val_inv` , like the table above : 
	 - if `val != 0` :  is_zero_expr = 0
	 - if `val == 0`  : is_zero_expr = 1

`vec![q_enable * value * is_zero_expr.clone()]`  is the gate's constraint. it should be $0$

##### assign

```rust
pub fn assign(
	&self,
	region: &mut Region<'_, F>,
	offset: usize,
	value: Value<F>,
) -> Result<(), Error> {
	// value.invert()  OR  F::zero()
	let value_inv = value.map(|value| value.invert().unwrap_or(F::zero()));
	region.assign_advice(|| "value inv", self.config.value_inv, offset, || value_inv)?;
	Ok(())
}
```

在 `IsZero` 的验证过程中，将要验证的值（或输入值）分配到电路区域中，以便在电路中进行计算和约束的验证 : 
1. 如果要验证的值为零，`assign` 方法将为逆元分配一个特定的值（例如 `F::zero()`）
2. 如果要验证的值不为零，`value_inv` columns 将被分配为 `value.invert().unwrap_or(F::zero())` i.e.  `value.invert()`

这些 IsZero 的 check 将被赋值到 `value_inv` column 并在其上得到体现


### Example 3 

welcome back, now we have the gadget `IsZero` , so we can constrain  malicious $P$ 's  input

```rust
#[derive(Debug, Clone)]
struct FunctionConfig<F: FieldExt> {
    selector: Selector,
    a: Column<Advice>,
    b: Column<Advice>,
    c: Column<Advice>,
    a_equals_b: IsZeroConfig<F>,
    output: Column<Advice>,
}
#[derive(Debug, Clone)]
struct FunctionChip<F: FieldExt> {
    config: FunctionConfig<F>,
}
```

#### configure

> Recap : `f(a, b, c) = if a == b {c} else {a - b}` 

1. column : 除了常规的 `a/b/c` advice column,  还申请了 `is_zero_advice_column`
2. `IsZeroChip` : `use crate::is_zero::{IsZeroChip, IsZeroConfig};`  使用了上面定义的 `IsZero` chip 来校验 $a == b$ 这个事情 (因为 a/b 都是 $P$  提供的, 一个 malicious $P$  有动机去提供 `a=3 , b=4` 然后 `return c` , 必须通过生成 proof 前的约束来限制 $P$ 的行为)
3. `IsZeroChip::configure` 返回 `IsZeroConfig<F>` 

```rust
impl<F: FieldExt> FunctionChip<F> {
    pub fn construct(config: FunctionConfig<F>) -> Self { Self { config } }

    pub fn configure(meta: &mut ConstraintSystem<F>) -> FunctionConfig<F> {
        let selector = meta.selector();
        let a = meta.advice_column();
        let b = meta.advice_column();
        let c = meta.advice_column();
        let output = meta.advice_column();

        let is_zero_advice_column = meta.advice_column();
        
        let a_equals_b = IsZeroChip::configure(
            meta,
            |meta| meta.query_selector(selector),
            |meta| meta.query_advice(a, Rotation::cur()) - meta.query_advice(b, Rotation::cur()),
            is_zero_advice_column,
        );

        meta.create_gate("f(a, b, c) = if a == b {c} else {a - b}", |meta| {
            let s = meta.query_selector(selector);
            let a = meta.query_advice(a, Rotation::cur());
            let b = meta.query_advice(b, Rotation::cur());
            let c = meta.query_advice(c, Rotation::cur());
            let output = meta.query_advice(output, Rotation::cur());
            vec![
                s.clone() * (a_equals_b.expr() * (output.clone() - c)),
                s * (Expression::Constant(F::one()) - a_equals_b.expr()) * (output - (a - b)),
            ]
        });

        FunctionConfig {
            selector,
            a,
            b,
            c,
            a_equals_b,
            output,
        }
    }
```

#### assign

1. `IsZeroChip::construct`  : 创建一个`IsZeroChip`实例
2. `layouter.assign_region(` : 
	1. `is_zero_chip.assign(&mut region, 0, Value::known(a - b))?;`
	2. `region.assign_advice(|| "output", self.config.output, 0, || Value::known(output))`
```rust
pub fn assign(
  &self,
  mut layouter: impl Layouter<F>,
  a: F,  b: F,  c: F,
) -> Result<AssignedCell<F, F>, Error> {
  let is_zero_chip = IsZeroChip::construct(self.config.a_equals_b.clone());

  layouter.assign_region(
    || "f(a, b, c) = if a == b {c} else {a - b}",
    |mut region| {
      self.config.selector.enable(&mut region, 0)?;
      region.assign_advice(|| "a", self.config.a, 0, || Value::known(a))?;
      region.assign_advice(|| "b", self.config.b, 0, || Value::known(b))?;
      region.assign_advice(|| "c", self.config.c, 0, || Value::known(c))?;

      // 正式使用 IsZeroChip 子电路来检查 a - b 是否为零
      is_zero_chip.assign(&mut region, 0, Value::known(a - b))?;

      // Rust expr to calculate val.
      let output = if a == b { c } else { a - b };
      // assign to cell.
      region.assign_advice(|| "output", self.config.output, 0, || Value::known(output))
    },
  ) }
```

#### test

```rust
#[cfg(test)]
mod tests {
    use super::*;
    use halo2_proofs::{dev::MockProver, pasta::Fp};

    #[test]
    fn test_example3() {
        let circuit = FunctionCircuit {
            a: Fp::from(10),
            b: Fp::from(12),
            c: Fp::from(15),
        };

        let prover = MockProver::run(4, &circuit, vec![]).unwrap();
        prover.assert_satisfied();
    }
}
```

### usage


```bash
cargo test -- --nocapture fibonacci::example3

# Draw
cargo test --release --all-features plot_fibo3
```

 - the white column is the instance column, 
 - the pink one is the advice and 
 - the purple one is the selector.
 - the green part shows the cells that have been assigned
      - light green : selector not used.


### References:
 - [Jason Morton halo2 codes](https://github.com/jasonmorton/halo2-examples/blob/master/src/fibonacci/example1.rs)
 - [ZCash halo2 books](https://zcash.github.io/halo2/user/simple-example.html#define-a-chip-implementation)
 - [trapdoor-tech halo2 book](https://trapdoor-tech.github.io/halo2-book-chinese/user/simple-example.html)
 - [icemelon/HaiCheng Shen](https://github.com/icemelon/halo2-examples/blob/master/src/fibonacci/example3.rs)
 - [0xPARC halo2](https://learn.0xparc.org/)
