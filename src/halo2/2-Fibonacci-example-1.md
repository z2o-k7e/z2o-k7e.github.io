> author: [@Demian](https://github.com/Demian101)
> 
> references: https://learn.0xparc.org/materials/halo2

[TOC]
### Goal

Given $f(0) = x, \ \  f(1) = y$ , we will prove $f(9) = z$

```bash
    1, 1, 2, 3, 5, 8, 13, ...

    | elem_1 | elem_2 | sum | q_fib
    --------------------------------
    |    1   |    1   |  2  |   1
    |    1   |    2   |  3  |   1
    |    2   |    3   |  5  |   1
    |        |        |     |   0

    q_fib * (elem_1 + elem_2 - elem_3) = 0
```


![](./imgs/2-Fib/2023-08-25-160959.png)

### Overview

`struct ACell` : 是一个 *tuple struct* ，是对 `AssignedCell` 封装和抽象

`FiboChip` ：斐波那契数列”芯片“（本 Application 无需自定义指令集）

`FiboConfig` ： 定义了需要挑选哪些”芯片元件“（advice、selector、instance...）

`impl FiboChip`：芯片元件的连接排布方式
 - `fn construct()`
 - `fn configure()` ： 门(create_gate)

#### ACell 

```rust
#[derive(Debug, Clone)]
struct ACell<F: FieldExt>(AssignedCell<F, F>);
```

**Why ACell ?**
1. **封装和抽象**：通过使用 `ACell`，我们为用户提供了一个简化和更直观的接口，使他们可以更容易地与已分配的单元格进行交互，而不必每次都直接处理 `AssignedCell`
2. **灵活性**：将来，如果我们想在 `ACell` 中添加更多的功能或属性，我们可以这样做而不影响现有的代码
3. 故 : `ACell` 主要是一个辅助结构体，用于简化与电路中单元格的交互

**元素访问 :** 
```rust
// 因为 `ACell` 是对 `AssignedCell` 的简单包装，
// 所以可以直接使用 `.0` 语法来访问其内部的 `AssignedCell` :  `prev_b.0`
let c_val = prev_b.0.value().copied() + prev_c.0.value();
```

`.map(ACell)?` 访问 :
 - 具体来说，`assign_advice` 返回的是 `Result<AssignedCell<F, F>, Error>`， `.map(ACell)` 会将其转换为 `Result<ACell<F>, Error>` 
 - 元组结构体本身可以作为函数来调用, 相当于调用一个带有一个参数的构造函数。

```rust
// when call .map() , 我们提供一个函数，将其应用于 Result 内的 Ok 的值（if so）
// 本例中传递的函数是 ACell 的构造函数，所以我们是将 AssignedCell 转换成 ACell
// 对于 tuple struct, 如 `let black = Color(0, 0, 0);`
// therefore  `AssignedCell<F, F>` 本身是一个函数
let a_cell = region
    .assign_advice(|| "a", self.config.advice[0], 0, || a)
    .map(ACell)?;
```

#### impl FiboChip { ...

 - `fn construct`
 - `fn configure`
 - `fn assign_first_row`  
 - `fn assign_row`
 - `fn expose_public`

##### fn configure()

参数：
 - `meta`: 是对约束系统的可变引用，允许我们在其中配置列和约束。
 - Selector : 用于激活或禁用某些特定约束
 - `meta.query_selector` : Query a selector at the current position.
 - `Query an advice column at a relative position` : Query an advice column at a relative position

```rust
impl<F: FieldExt> FiboChip<F> {
    // pub fn construct(config: FiboConfig) -> Self 
    pub fn configure(
        meta: &mut ConstraintSystem<F>,
        advice: [Column<Advice>; 3],
        instance: Column<Instance>,
    ) -> FiboConfig {
        let col_a = advice[0]; // 对每个 advice 列进行命名
        let col_b = advice[1];
        let col_c = advice[2];
        let selector = meta.selector();

        meta.enable_equality(col_a);
        meta.enable_equality(col_b);
        meta.enable_equality(col_c);
        meta.enable_equality(instance);

        meta.create_gate("add", |meta| {
            //
            // col_a | col_b | col_c | selector
            //   a      b        c       s
            //
            // Query a selector at the current position.
            let s = meta.query_selector(selector);
            let a = meta.query_advice(col_a, Rotation::cur());
            let b = meta.query_advice(col_b, Rotation::cur());
            let c = meta.query_advice(col_c, Rotation::cur());
            vec![s * (a + b - c)]
        });

        FiboConfig {
            advice: [col_a, col_b, col_c],
            selector,
            instance,
        }
    }
```

##### fn assign_first_row()

函数作用：为 Fibonacci list 的第一行的前 2 个元素分配值 `1 `  , 返回前 3 个元素 `a_cell, b_cell, c_cell`

 - `region.assign_advice_from_instance` ：从 Instance column (public input) 中取值并赋值给 Advice
```rust
// 在 MockProver::run 中，instance 就是 vec![public_input.clone()]
MockProver::run(k, &circuit, vec![public_input.clone()]).unwrap();
```

- `region.assign_advice`：给 Advice column 赋值，这里特指给斐波那契数列的第 3 个数赋值 `f(2) = f(0) + f(1)`


```rust
pub fn assign_first_row(
	&self,
	mut layouter: impl Layouter<F>,
) -> Result<(ACell<F>, ACell<F>, ACell<F>), Error> {
	layouter.assign_region(
		|| "first row",
		|mut region| {
			self.config.selector.enable(&mut region, 0)?;

			let a_cell = region.assign_advice_from_instance(
				|| "f(0)",
				self.config.instance,
				0,  // instance column's row 0
				self.config.advice[0],
				0  // offset, advice column's row.
			).map(ACell)?;

			let b_cell = region.assign_advice_from_instance(
				|| "f(1)",
				self.config.instance,
				1, // instance column's row 1
				self.config.advice[1],
				0  // offset, advice column's row.
			).map(ACell)?;

			let c_cell = region.assign_advice(
				|| "f(0)+f(1) i.e. a + b",
				self.config.advice[2],
				0,
				|| a_cell.0.value().copied() + b_cell.0.value()
			).map(ACell)?;

			Ok((a_cell, b_cell, c_cell))
		},
	)
}
```

##### fn assign_row()

`fn assign_row()` 函数的作用：不断将上一行的 $a_1$ 和 $a_2$ Advice 列复制到当前行的 $a_0$ 和 $a_1$ Advice 列

PS：这是通过 `impl Circuit for MyCircuit..` 中的  `synthesis()` 重复调用实现的： 
```rust
fn synthesis() {
    // ...
	for _i in 3..10 {
		let c_cell = chip.assign_row(layouter.namespace(|| "next row"), &prev_b, &prev_c)?;
		prev_b = prev_c;
		prev_c = c_cell;
	}
```

传入参数：
 - `prev_b: &ACell<F>, prev_c: &ACell<F>,` ： 这正是*上一行*中的 $a_1$ 和 $a_2$ Advice 列（第 2/3 Advice 列），这意味着前一个`b`值被复制到新行的第一列（标记为`a`），前一个`c`值被复制到新行的第二列（标记为`b`）

将 `prev_b、prev_c` 2 个 Cells  `copy_advice` 到新的 Advice 列后：
 - 计算新的斐波那契数`c_val`，它是`prev_b`和`prev_c`的和。
 - 使用`assign_advice`分配`c_val`到新行的第三列，并返回此值的 `ACell`

```rust
pub fn assign_row(
	&self,  // 当前`FiboChip`实例的引用
	mut layouter: impl Layouter<F>,
	prev_b: &ACell<F>,   // Fibonacci 数列中的上一行的第 2/3 个 Advice Cell
	prev_c: &ACell<F>,
) -> Result<ACell<F>, Error> {
	layouter.assign_region(
		|| "next row",
		|mut region| {
			self.config.selector.enable(&mut region, 0)?;

			prev_b.0.copy_advice(
				|| "a", 
				&mut region, 
				self.config.advice[0], 
				0
			)?;
			prev_c.0.copy_advice(|| "b", &mut region, self.config.advice[1], 0)?;

			let c_val = prev_b.0.value().copied() + prev_c.0.value();

			let c_cell = region
				.assign_advice(|| "c", self.config.advice[2], 0, || c_val)
				.map(ACell)?;

			Ok(c_cell)
		},
	)
}
```

**copy_advice  vs  assign_advice**   i.e.   **复制 vs. 赋值**: 
 - 当我们说“复制”，我们实际上是说我们要确保一个 Region-Cell 的值与另一个 Region-Cell 中的值是相同的。与其为每个地方重新计算/分配(assign)一个值，不如简单地“复制”该值到新位置，以确保它们是一样的（考虑 PLONK 中的 permutation argument）
 - **Permutations and Copy Constraints**:  Halo2 使用一种称为"permutation argument"的技术来确保两个或多个单元格中的值是相同的。`copy_advice` 实际上是在背后使用这个技术，通过引入一个额外的约束来确保值的一致性

##### fn expose_public()

`expose_public` 函数作用：将指定的 `ACell` 公开为 Public Input

```rust
fn synthesis() {
    // ...
    // 将最后一个值公开为 public input
    chip.expose_public(layouter.namespace(|| "out"), &prev_c, 2)?;
}
```


```rust
pub fn expose_public(
	&self,
	mut layouter: impl Layouter<F>,
	cell: &ACell<F>,
	row: usize,
) -> Result<(), Error> {
	layouter.constrain_instance(cell.0.cell(), self.config.instance, row)
}
```

#### MyCircuit

1. `let chip = FiboChip::construct(config);` : 传入 `config` 创建一个新的 `FiboChip` 实例
2. `chip.assign_first_row(layouter.namespace(|| "first row"), self.a, self.b)?;` : **初始化斐波那契数列**:  调用 `assign_first_row` 函数以在第一行中设置斐波那契数列的前两个值 self.a 和 self.b。返回的结果是三个值：prev_a, prev_b 和 prev_c。其中，prev_c 是前两个数的和 
3. `chip.expose_public(layouter.namespace(|| "private a"), &prev_a, 0)?;` : **公开前两个数**:  将前两个数 expose 为 public, 这意味着这些值可以被 $Verifier$ 访问和验证
4. **计算后续的斐波那契数**: ` for` 循环中，`assign_row` 函数被调用以计算后续的斐波那契数。每次迭代都会生成新的斐波那契数并为下一次迭代更新 `prev_b` 和 `prev_c`
5. `chip.expose_public(layouter.namespace(|| "out"), &prev_c, 2)?;` : **公开最终的斐波那契数**:  将循环结束后的最后一个斐波那契数值设为 Public

```rust
#[derive(Default)]
struct MyCircuit<F> (PhantomData<F>);

impl<F: PrimeField> Circuit<F> for MyCircuit<F> {
    type Config = FiboConfig;
    type FloorPlanner = SimpleFloorPlanner;

    fn without_witnesses(&self) -> Self {
        Self::default()
    }

    fn configure(meta: &mut ConstraintSystem<F>) -> Self::Config {
        let col_a = meta.advice_column();
        let col_b = meta.advice_column();
        let col_c = meta.advice_column();
        let instance = meta.instance_column();
        FiboChip::configure(meta, [col_a, col_b, col_c], instance)
    }
}
```

##### fn synthesis()

这个函数定义了在 `MyCircuit` 电路中如何放置约束和分配单元格 Cell，它构建了一个斐波那契序列，并确保输出正确。

```rust
fn synthesize(
	&self,
	config: Self::Config,
	mut layouter: impl Layouter<F>,
) -> Result<(), Error> {
	let chip = FiboChip::construct(config);

	let (_, mut prev_b, mut prev_c) =
		chip.assign_first_row(layouter.namespace(|| "first row"))?;
	
	// 这是干啥??
	// chip.expose_public(layouter.namespace(|| "private a"), &prev_a, 0)?;
	// chip.expose_public(layouter.namespace(|| "private b"), &prev_b, 1)?;

	for _i in 3..10 {
		let c_cell = chip.assign_row(layouter.namespace(|| "next row"), &prev_b, &prev_c)?;
		prev_b = prev_c;
		prev_c = c_cell;
	}

	chip.expose_public(layouter.namespace(|| "out"), &prev_c, 2)?;

	Ok(())
}
```

#### Test

```rust
#[cfg(test)]
mod tests {
    use super::MyCircuit;
    use halo2_proofs::{circuit::Value, dev::MockProver, pasta::Fp};
    use std::marker::PhantomData;

    #[test]
    fn test_example1() {
        let k = 4;

        let a = Fp::from(1); // F[0]
        let b = Fp::from(1); // F[1]
        let out = Fp::from(55); // F[9]

        let circuit = MyCircuit(PhantomData);

        let mut public_input = vec![a, b, out];

        let prover = MockProver::run(k, &circuit, vec![public_input.clone()]).unwrap();
        prover.assert_satisfied();

        public_input[2] += Fp::one(); // out += 2  =>  unsatisfied
        let _prover = MockProver::run(k, &circuit, vec![public_input]).unwrap();
        // uncomment the following line and the assert will fail
        // _prover.assert_satisfied();
    }
```

### Usage

```bash
cargo test -- --nocapture fibonacci::example1

# Draw
cargo test --release --all-features plot_fibo1
```

 - the white column is the instance column, 
 - the pink one is the advice and 
 - the purple one is the selector.
 - the green part shows the cells that have been assigned
	 - light green : selector not used.

### Reference :
 - [Jason Morton halo2 codes](https://github.com/jasonmorton/halo2-examples/blob/master/src/fibonacci/example1.rs)
 - [ZCash halo2 books](https://zcash.github.io/halo2/user/simple-example.html#define-a-chip-implementation)
 - [trapdoor-tech halo2 book](https://trapdoor-tech.github.io/halo2-book-chinese/user/simple-example.html)
 - [icemelon/HaiCheng Shen](https://github.com/icemelon/halo2-examples/blob/master/src/fibonacci/example3.rs)
 - [0xPARC halo2](https://learn.0xparc.org/)


### question

... No Question?