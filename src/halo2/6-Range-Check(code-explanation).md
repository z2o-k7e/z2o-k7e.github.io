> author: [@Demian](https://github.com/Demian101)
> 
> references: https://learn.0xparc.org/materials/halo2

[TOC]
### Overview

本部分是 Jason Morton 对于上一 Chapter 的代码超の详细解释：
https://github.com/jasonmorton/halo2-examples/blob/master/src/range_check/example1b.rs

下面一起来看看

### imports

```rust
use ff::{Field, PrimeField};
use std::marker::PhantomData; // Allows Rust to track types that do not otherwise appear in a struct's fields, here just the field element type

use halo2_proofs::{
    circuit::{
        floor_planner::V1,
        AssignedCell, // a value Value<V> together with its global location as a Cell with region_index, row_offset, and column
        Layouter,     // layout strategy and accepter struct, a bit like a Writer
        Value,        // basically an Option<V>, where Some(v) is called known and None is unknown
    },
    plonk::{
        Advice,      // empty struct to mark Advice columns
        Assigned, // enum Zero, Trivial(F) "does not require inversion to evaluate", or Rational(F, F) "stored as a fraction to enable batch inversion". This is an actual value (wrapped felt)
        Circuit,  // trait with without_witnesses, configure, and synthesize methods
        Column, // represents a pre-layout abstract Column. Fields are index: usize and column type.
        ConstraintSystem, // The container for the actual constraint system; much of the frontend code exists to make it easier to populate this container
        Constraints, // Iterable with a selector and Constraint s.  Constraints are single polynomial Expressions returned by create gate
        Error,       // Custom Error type
        Expression, // Polynomial expression enum, as binary tree, with 5 types of atomic variables v (Constant, Selector, Fixed, Advice, Instance) and combinations -v, v+v, a*v, or v*v.
        Selector, // (index: usize, simple: bool) column type, w/ index = index of this selector in the ConstraintSystem, simple = "can only be multiplied by Expressions not containing Selectors"
    },
    poly::Rotation, // i32 wrapper representing rotation in Lagrange basis
};
```

 - **AssignedCell**：AssignedCell 表示一个与其全局位置相关联的值 `Value<V>`，该位置定义为 Cell，字段： region_index, row_offset 和 column
 - **Value**：Value 本质是一个 `Option<V>` ，其中 Some(v) 被称为 known（已知），而 None 则是 unknown（未知）
 - **Assigned**：Assigned 是一个 enum 枚举类型，有 `Zero, Trivial(F) 和 Rational(F, F)` 三个成员。其中 
	 - Trivial(F) "A value that does not require inversion to evaluate."，
	 - Rational(F, F) "存储为分数以便进行批量取反 (A value stored as a fraction to enable batch inversion.)" ，这是一个实际的值（wrapped felt）
 - **Circuit**：Circuit 是一个 trait，具有 *without_witnesses, configure 和 synthesize* 方法
 - **ConstraintSystem**：ConstraintSystem 是实际约束系统的容器；大部分前端代码的存在都是为了更容易地填充这个容器
 - **Constraints** ： 提供了一个新的 API 来进行约束
 - **Expression**：
	 - Expression 是多项式表达式的枚举类型，表现为二叉树，拥有5种类型的原子变量 v（Constant, Selector, Fixed, Advice, Instance）以及combination `-v, v+v, a*v, or v*v.`  
	 - Low-degree expression representing an identity that must hold over the committed columns.

```rust
// Config 是您自定义电路的关联类型（仅要求实现 Clone trait）。它没有特定的强制结构，存储所需的任何类型信息
// A Config is an associated type of your custom circuit (required only to be Clone).  With no particular enforced structure, it stores whatever type information is needed

// to understand the constraint system (number and types of columns, their indices, some flags such as simple/complex selector, etc.).
// It is a bit like a morphism type in a Monoidal category (domain and codomain), or the row and column labels in a dataframe. Let's call it the FrameType
// It can be unstructured because it is the Circuit implementer's job to translate this information into the format needed for the Layouter.

#[derive(Clone)]
struct MyConfig<F: PrimeField, const RANGE: usize> {
    advice_column: Column<Advice>,  // a marker and index for a Selector
    q_range_check: Selector,   // similarly a marker and index for a Selector
    _marker: PhantomData<F>,
}

// 通常，Config 会有一个 configure 和 assign 方法，它们由 Circuit 的 configure 和 synthesize 方法委托。在这个版本的示例中，我们将直接在电路中放置此逻辑。
// By convention the Config gets a configure() and assign() method, which are delegated to by the configure and synthesize method of the Circuit.
// In this version of the example we will put this logic directly in the circuit.
impl<F: PrimeField, const RANGE: usize> MyConfig<F, RANGE> {}

#[derive(Default)] // Deriving Default calls Default on Value<Assigned<F>> calls impl<V> Default for Value<V> { fn default() -> Self {  Self::unknown()  }}
                   // which in turn sets value.inner: Option<V> to None
struct MyCircuit<F: PrimeField, const RANGE: usize> {
    assigned_value: Value<Assigned<F>>,
    _marker: PhantomData<F>,
}
impl<F: PrimeField, const RANGE: usize> MyCircuit<F, RANGE> {}
```


### impl Circuit for MyCircuit

```rust
// Your Circuit plays several roles and  will be passed to prover and verifier key generation, prove, and verifier.
// Implementing the Circuit trait requires three functions:
// 1) Anything special that needs to be done to set it up without witness values (which will be used in p&v key generation), usually just all witness
//    values are set to None (Remember Value is basically an alias of Option).
// 2) How to 'configure' it.  This is a bit mysterious because the required trait method configure just returns your custom associated type
//    Config, which in turn need only be Clone.  So what is happening? If we look at the MockProver code, we see that an empty ConstraintSystem is
//    created and passed to the configure function as mutable, then an immutable borrow is taken and worked with for the duration:
//         let mut cs = ConstraintSystem::default();
//         let config = ConcreteCircuit::configure(&mut cs);
//         let cs = cs;
//    Thus it the Circuit's `configure` method that is responsible for all of the contraint system definition.  It could be called compile_constraints.
```

电路会扮演多个角色，并且会被传递给 `prover` 、 `verifier` 来 进行 key generation, prove, and verify.
 - `without_witnesses`  通常在 p&v key generation 期间使用
 - How to 'configure' it?  如果我们查看 MockProver 代码，我们可以看到创建了一个空的 ConstraintSystem，传递给 `configure()` 作为可变引用

```rust
//    Why does the configure method accept an empty constraint system and return the arbitrary Self::Config, after filling the constaint system to its liking?
//    The config returned by ConcreteCircuit ::configure is used just once in the MockProver:
//         ConcreteCircuit::FloorPlanner::synthesize(&mut prover, circuit, config, constants)?;
//    So after setting up the constraints, the Config stores the information about the layout that the floor planner will need to synthesize.
//
//    The `configure` function is where we call cs.create_gate() and so on, adding polynomials to the system.  At this point, the variables in these polynomials
//    are still locally named/scoped, and will be given a global name/scope during layout.
```

为什么 configure 方法接受一个空的约束系统，并在填充约束系统后返回任意的 Self::Config？
1. ConcreteCircuit::configure 返回的 config 在 MockProver 中只使用一次：
2. `ConcreteCircuit::FloorPlanner::synthesize(&mut prover, circuit, config, constants)?;`
3. 因此，在设置约束之后，Config 存储了 floor planner 在合成时需要的布局信息。

```rust
// 3) How to synthesize, given the circuit, a provided &mut Layouter and the data passed in the Config.  The synthesize method returns only error information, and
//    changes state by writing to the Layouter.  Thus the Layouter is like a buffer or Writer to which the final circuit is written.  One will be supplied by the
//    MockProver below (and variants will be needed during keygen and proving).
//
//  Roughly, configure provides the concrete but relative layout, and synthesize combines such blocks and assigns an absolute layout.
impl<F: PrimeField, const RANGE: usize> Circuit<F> for MyCircuit<F, RANGE> {
    type Config = MyConfig<F, RANGE>;
    type FloorPlanner = V1;

    // This boilerplate could be removed by putting it in the Circuit trait and defining the Circuit trait with a bound as Circuit: Default, but that might be annoying too.
    fn without_witnesses(&self) -> Self {
        Self::default() // should fill all the Witness Values with None/Unknown.
    }
```

How to synthesize?  
 - 给定电路 Circuit、 `&mut Layouter` 和在 Config 中传递的数据。synthesize 方法只返回错误信息，并通过写入 Layouter 更改状态。因此，Layouter 就像最终电路被写入的缓冲区或 Writer。

大致来说，configure 提供了具体但相对的布局，而 synthesize 组合这些块并分配绝对布局。

#### fn configure()

```rust
// define the constraints, mutate the provided ConstraintSystem, and output the resulting FrameType
// 改变(mutate) 提供的 ConstraintSystem，并输出结果的 FrameType
fn configure(cs: &mut ConstraintSystem<F>) -> Self::Config {
	// Create the column marker types. Requests the CS to allocate a new column (giving it a unique cs-global index and incrementing its
	// 创建列标记类型。请求 CS 分配一个新列（给它一个唯一的 cs-global 索引并增加它的
	//   num_selectors, num_fixed_columns, num_advice_columns, or num_instance_columns).
	let advice_column = cs.advice_column();
	let q_range_check = cs.selector();

	// When we use cs.query_advice or cs.query_selector, we obtain an Expression which is a reference to a cell in the matrix.
	// 访问 cs.query_advice / query_selector 获得一个 Expression，它是矩阵中一个单元格的引用(reference)
	//   Expression::Advice {
	//     query_index: self.meta.query_advice_index(column, at),
	//     column_index: column.index,
	//     rotation: at,
	//   }
	// Such an a_{ij} or a_{this_row + at, column} can be treated as a symbolic variable and put into a polynomial constraint.
	// More precisely, this is a relative reference wrt rows. 
	// 例如 a_{ij} / a_{this_row + at, column} 会被视为一个符号变量，并放入一个多项式约束中。
	// (这是一个相对于行的相对引用)

	// cs.create_gate takes a function from virtual_cells to contraints, pushing the constraints to the cs's accumulator.  So this puts
	// (value.clone()) * (1 - value.clone()) * (2 - value.clone()) * ... * (R - 1 - value.clone())
	// into the constraint list.
	// 注意 [VirtualCells], 它持有对`ConstraintSystem`的可变引用，存储已查询的选择器/不同类型的列
	cs.create_gate("range check", |virtual_cells| {
		let q = virtual_cells.query_selector(q_range_check);
		let value = virtual_cells.query_advice(advice_column, Rotation::cur());

		// Given a range R and a value v, returns the expression
		// (v) * (1 - v) * (2 - v) * ... * (R - 1 - v)
		//  Range Check poly:
		let rc_polynomial = (1..RANGE).fold(value.clone(), |expr, i| {
			expr * (Expression::Constant(F::from(i as u64)) - value.clone())
		});

		Constraints::with_selector(q, [("range check", rc_polynomial)])
	});

	// The "FrameType"
	Self::Config {
		q_range_check,
		advice_column,
		_marker: PhantomData,
	}
}
```

**query_selector  &  query_advice**
 - When we use `cs.query_advice` or `cs.query_selector`, we obtain an *Expression* which is a reference to a cell in the matrix. 
 - 一个 `a_{ij}` 或 `a_{this_row + at, column}` 可以被视为一个符号变量，并放入一个多项式约束中。更准确地说，这是一个相对于行的相对引用。


#### fn synthesize()

```rust
fn synthesize(
	&self,
	config: Self::Config,
	mut layouter: impl Layouter<F>, // layouter is our 'write buffer' for the circuit
) -> Result<(), Error> {
	// From the function docs:
	// Assign a region of gates to an absolute row number. 将门的 region 分配一个绝对行号。
	// Inside the closure, the chip may freely use relative offsets; the `Layouter` will
	// treat these assignments as a single "region" within the circuit. Outside this
	// closure, the `Layouter` is allowed to optimise as it sees fit.
	// 闭包内，chip 可以自由使用相对偏移；`Layouter` 会将这些 assignments 视为电路中的单个“region”。
	// 在闭包外部，`Layouter` 可以根据需要进行优化

	layouter.assign_region(
		|| "Assign value", // the name of the region
		|mut region| {
			let offset = 0;

			// Enable q_range_check. Remember that q_range_check is a label, a Selector. Calling its enable
			// - calls region.enable_selector(_,q_range_check,offset)  which
			// - calls enable_selector on the region's RegionLayouter which
			// - calls enable_selector on its "CS" (actually an Assignment<F> (a trait), and whatever impls that
			// does the work, for example for MockProver the enable_selector function does some checks and then sets
			//   self.selectors[selector.0][row] = true;
			config.q_range_check.enable(&mut region, offset)?;

			// Similarly after indirection calls assign_advice in e.g. the MockProver, which
			// takes a Value-producing to() and does something like
			// CellValue::Assigned(to().into_field().evaluate().assign()?);
			// 类似 MockProver 间接调用 assign_advice, 它接受一个 Value-producing 的 to() (进行赋值)
			region.assign_advice(
				|| "value",
				config.advice_column,
				offset,
				|| self.assigned_value,
			)
		},
	)?;
	Ok(())
} }
```

### tests

```rust
#[cfg(test)]
mod tests {
    use halo2_proofs::{
        dev::{FailureLocation, MockProver, VerifyFailure},
        pasta::Fp,
        plonk::{Any, Circuit},
    };

    use super::*;

    #[test]
    fn test_range_check_1() {
        let k = 4; //2^k rows
        const RANGE: usize = 8; // 3-bit value
        let testvalue: u64 = 22;

        // Successful cases
        for i in 0..RANGE {
            let circuit = MyCircuit::<Fp, RANGE> {
                assigned_value: Value::known(Fp::from(i as u64).into()),
                _marker: PhantomData,
            };

            // The MockProver arguments are log_2(nrows), the circuit (with advice already assigned), and the instance variables.
            // The MockProver will need to internally supply a Layouter for the constraint system to be actually written.
            // k 对应 2^k 行, MockProver 将需要内部提供一个 Layouter，以便实际编写约束系统
            let prover = MockProver::run(k, &circuit, vec![]).unwrap();
            prover.assert_satisfied();
        }

        // Out-of-range `value = 8`
        {
            let circuit = MyCircuit::<Fp, RANGE> {
                assigned_value: Value::known(Fp::from(testvalue).into()),
                _marker: PhantomData,
            };
            let prover = MockProver::run(k, &circuit, vec![]).unwrap();
            assert_eq!(
                prover.verify(),
                Err(vec![VerifyFailure::ConstraintNotSatisfied {
                    constraint: ((0, "range check").into(), 0, "range check").into(),
                    location: FailureLocation::InRegion {
                        region: (0, "Assign value").into(),
                        offset: 0
                    },
                    cell_values: vec![(((Any::Advice, 0).into(), 0).into(), "0x16".to_string())]
                }])
            );
        }
    }
}
```


### References : 
 - https://github.com/enricobottazzi/halo2-intro/blob/master/src/range_check/example5/table.rs
 - [Jason Morton halo2 codes](https://github.com/jasonmorton/halo2-examples/blob/master/src/fibonacci/example1.rs)
 - [ZCash halo2 books](https://zcash.github.io/halo2/user/simple-example.html#define-a-chip-implementation)
 - [trapdoor-tech halo2 book](https://trapdoor-tech.github.io/halo2-book-chinese/user/simple-example.html)
 - [icemelon/HaiCheng Shen](https://github.com/icemelon/halo2-examples/blob/master/src/fibonacci/example3.rs)
 - [0xPARC halo2](https://learn.0xparc.org/)
