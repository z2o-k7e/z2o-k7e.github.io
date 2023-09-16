

**AssignedCell** : 
 - a value `Value<V>` together with its global location as a Cell with `region_index`, `row_offset`, and `column`  (具有全局坐标的 Cell , 有 `region_index`, `row_offset`, and `column`)

>  根据代码，请对比下  plonk/circuit.rs 中的 VirtualCells  和 src/circuit.rs 中的  AssignedCell  之间的不同和设计特点

**设计目标**：
- `VirtualCells`: 它似乎是为了帮助用户查询不同类型的列：selector、advice、fixed 和 instance。它跟踪查询过的选择器和单元，并提供查询功能。这有助于构建约束，其中用户可以根据所需的列和旋转查询数据
- `AssignedCell`: 它代表一个在电路中分配了特定值的单元。这包括存储值的实际内容、该单元的位置和其关联字段的标记。这为用户提供了对已分配单元的直接访问，以及根据需要从这些单元获取值的能力

**存储**：
 - `VirtualCells`: 持有对`ConstraintSystem`的可变引用，存储已查询的选择器列表，存储已查询的单元格列表
 - `AssignedCell`: 存储该单元格的值，存储该单元格的位置信息，

**功能**
 - `VirtualCells` :
	 - 提供了查询selector、advice、fixed、instance和any列的功能。
	- 对查询过的选择器和单元进行跟踪。
 - `AssignedCell`:
	- 提供访问已分配值的功能。
	- 提供访问单元格的功能。
	- 提供将值从一个单元复制到另一个单元的功能，并约束它们的值相等。
	- 提供了评估该分配单元格值的功能。

**使用场景**
- **VirtualCells**: 在你需要构建和查询约束系统时使用，它为用户提供了在构建约束时需要的灵活性。
- **AssignedCell**: 当你已经在电路中某个位置分配了一个值并希望与其互动时使用。


-----

**query_selector  &  query_advice**

When we use `cs.query_advice` or `cs.query_selector`, we obtain an *Expression* which is a reference to a cell in the matrix.

一个 `a_{ij}` 或 `a_{this_row + at, column}` 可以被视为一个符号变量，并放入一个多项式约束中。更准确地说，这是一个相对于行的相对引用。

```rust
Expression::Advice {
    query_index: self.meta.query_advice_index(column, at),
    column_index: column.index,
    rotation: at,
}
```

 - `query_selector`  不需要指定 rotation,   because the selector always get queried at the current row .
 - `query_advice`  则需要指定 relative rotation.  because advice col 是相对于选择器偏移量(Selector offset)进行查询的，所以我们需要指定 relative rotation.
```rust
let q = meta.query_selector(q_range_check);

// note that we don't need to specify the rotation when querying the `selctor`
// That's because the selector always get queried at the current row .
// While the `advice columns` get queried relatively to the selector offset, so we need to specify the relative rotation
// 然而 advice col 是相对于选择器偏移量(Selector offset)进行查询的，所以我们需要指定 relative rotation.
let value = meta.query_advice(value, Rotation::cur());
```


**Config** :
 - A Config is an <u>associated type</u> of your custom circuit (required only to be `Clone`).  With no particular enforced structure, it stores whatever type information is needed  to understand the constraint system (number and types of columns, their indices, some flags such as simple/complex selector, etc.).    (由于没有特定的强制结构，它存储理解约束系统所需的任何类型信息（列的数量和类型、它们的索引、一些标志，例如简单/复杂选择器等)

```rust
#[derive(Clone)]
struct MyConfig<F: FieldExt, const RANGE: usize> {
    advice_column: Column<Advice>, // note that this is a marker, cannot hold an element (but will be assigned a usize index). It describes the constraint system output by circuit.configure(). It tells us there is one advice column (which will need to be assigned a global column index in the matrix).
    q_range_check: Selector,       // similarly a marker and index for a Selector
    _marker: PhantomData<F>,
}
```

**Value**
 - (Remember **Value** is basically an alias of Option).
```rust
pub struct Value<V> {
    inner: Option<V>,
}
```

### Expression



#### copy_advice

The main goals of `copy_advice()` are:
1. Copy a value from `&self` to another.
2. Make sure the copied value is equal to the original value.

```rust
/// Copies the value to a given advice cell and constrains them to be equal.
/// Returns an error if either this cell or the given cell are in columns where equality has not been enabled.
pub fn copy_advice<A, AR>(
	&self,
	annotation: A,
	region: &mut Region<'_, F>,
	column: Column<Advice>,
	offset: usize,
) -> Result<Self, Error>
where
	A: Fn() -> AR,
	AR: Into<String>,
{
	let assigned_cell =
		region.assign_advice(annotation, column, offset, || self.value.clone())?;
		region.constrain_equal(assigned_cell.cell(), self.cell())?;

	Ok(assigned_cell)
}
```

Params :
 - `&self` 即调用 `.copy_advice()` 的那个 Cell
 - `column、offset` 即即将被赋值过去的那个目的地 column、offset

Process ：
1. 将本 Cell 的取值 `self.value` *assign_adcice* 到目标位置
2. 约束被赋值的 Cell:  `assigned_cell` 和 本 Cell `self.cell()` 相等。

问题：`region.constrain_equal` 这一步是不是多余的？因为本来 assigned_cell 不就是根据self.value 生成的吗？
解答：非也，这个约束非常必要！
1. **区分计算与证明**：在零知识证明系统中，有两个关键的阶段：**构建**和**证明**。在构建阶段，我们设置了所有的约束并确定我们想要证明的属性。然后，在证明阶段，我们确保所有的约束都得到满足，而不暴露任何私有信息。
2. **约束的作用**：在构建阶段，`constrain_equal`函数的作用是声明：“我想要确保这两个位置的值是相等的。”即使这两个值在我们的知识中是相等的，但零知识证明系统并不“知道”它们是相等的，除非我们明确告诉它。如果不设置这个约束，那么在证明阶段，证明系统就不会检查这两个值是否相等。
3. **防止欺诈**：考虑到一个恶意的证明者可能会尝试提交一个无效的证明。如果没有`constrain_equal`这样的约束，他们就可以在不同的位置设置不同的值，而不会被检测到。
4. **在R1CS中的等价性**：在很多零知识证明系统中，最终所有的约束都会被转化为一个叫做R1CS（Rank-1 Constraint Systems）的形式。在这种形式中，`constrain_equal`实际上就是确保某两个变量的值相乘的结果（也就是它们的差的平方）是 0

总结：虽然在外部逻辑中，这个步骤看起来是多余的，但在零知识证明的上下文中，确保等价性是至关重要的。这确保了证明的完整性和正确性，不论证明者的意图如何。

看一个例子：

```rust
impl<F: PrimeField> FiboChip<F> {
  pub fn construct(...)
  pub fn configure(...)
  pub fn assign_first_row(...)
  pub fn assign_row(
    prev_b: &ACell<F>,
    prev_c: &ACell<F>,
  ) -> Result<ACell<F>, Error> {
    layouter.assign_region(|| "next row",
      |mut region| {
        self.config.selector.enable(&mut region, 0)?;
        prev_b.0.copy_advice(|| "a", &mut region, self.config.advice[0], 0)?;
        prev_c.0.copy_advice(|| "b", &mut region, self.config.advice[1], 0)?;
        
        let c_val = prev_b.0.value().copied() + prev_c.0.value();
        let c_cell = region.assign_advice(
            || "c", 
            self.config.advice[2], 
            0, 
            || c_val
         ).map(ACell)?;
      }
```

<img src="http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-09-01-IMG_0854.jpg" width="50%" />
代码需结合图一起看：
 - 现在操作的行是 Row 1（Current row，黄框那行）
 - `prev_b` 和 `prev_c`  是传入的 2 个 Cell
 - 观察红色和蓝色的连接线，这就是被做 Copy Constraints 的 2 个 Cell

```rust
prev_b.0.copy_advice(|| "a", &mut region, self.config.advice[0], 0)?;
```
 - prev_b 即 Row 0 、 $a_1$  的这个元素
 - 在 prev_b 上调用 `copy_advice`  即将其上的值赋值+约束到 `advice[0]` 列， offset 为 0 的行（即 Current Row 即 Row 1）

#### assign_advice

`halo2_proofs/src/circuit.rs`

 - `assign_advice()` 是为了在指定的 advice column 和偏移（行）中分配一个值，并返回该已分配值的 wrapping，i.e. `AssignedCell`

参数 : 
 - `&'v mut self`: 代表函数需要一个 `self` 的可变引用，并且这个引用的生命周期至少为 `'v` 
 - `annotation: A`: 是一个返回注释字符串的函数。可用于为调试或错误消息提供上下文 
 - `column: Column<Advice>`: 指定应该在哪一列分配该值。	 
 - `offset: usize`: 指定应该在哪一行分配该值。
 - `mut to: V`: 是一个函数，它返回将要被分配的值。这是一个闭包或函数对象

`where` 子句指定了该函数的泛型参数必须满足的约束：
 - `V` 是一个可变函数对象，返回一个 `Value<VR>`。 
 - `Assigned<F>` 必须实现 `From` trait，从一个对 `VR` 的引用转换。
 - `A` 是一个函数对象，返回一个 `AR`。
 - `AR` 必须能转换为字符串，这是为了 `annotation` 能够提供描述性的字符串。

**Process :** 
1. 函数首先声明了一个未知的值：`let mut value = Value::unknown();`
2. 然后，它使用 `self.region.assign_advice` 方法来真正分配值，传递注释、列、偏移和一个获取值的闭包。此闭包先调用 `to()` 获取值，然后转换该值为字段，并更新之前声明的未知值。
3. 最后，一个新的 `AssignedCell` 被构造并返回，其中包含已分配的值和对应的单元格。
4. `assign_advice` 函数为一个给定的建议列和行分配一个值，并返回一个表示已分配值的 `AssignedCell`。这个函数设计得非常泛化，允许客户端提供任意的值提供函数和注释函数。

```rust
pub fn assign_advice<'v, V, VR, A, AR>(
	&'v mut self,
	annotation: A,
	column: Column<Advice>,
	offset: usize,
	mut to: V,
) -> Result<AssignedCell<VR, F>, Error>
where
	V: FnMut() -> Value<VR> + 'v,
	for<'vr> Assigned<F>: From<&'vr VR>,
	A: Fn() -> AR,
	AR: Into<String>,
{
	let mut value = Value::unknown();
	let cell =
		self.region
			.assign_advice(&|| annotation().into(), column, offset, &mut || {
				let v = to();
				let value_f = v.to_field();
				value = v;
				value_f
			})?;

	Ok(AssignedCell {
		value,
		cell,
		_marker: PhantomData,
	})
}
```


#### assign_advice_from_instance

- 将绝对位置“row”处的 instance col cell 的值分配给该区域内“offset”处的列“advice”。

```rust
/// Assign the value of the instance column's cell at absolute location
/// `row` to the column `advice` at `offset` within this region.
///
/// Returns the advice cell, and its value if known.
pub fn assign_advice_from_instance<A, AR>(
	&mut self,
	annotation: A,
	instance: Column<Instance>,
	row: usize,
	advice: Column<Advice>,
	offset: usize,
) -> Result<AssignedCell<F, F>, Error>
where
	A: Fn() -> AR,
	AR: Into<String>,
{
	let (cell, value) = self.region.assign_advice_from_instance(
		&|| annotation().into(),
		instance,
		row,
		advice,
		offset,
	)?;

	Ok(AssignedCell {
		value,
		cell,
		_marker: PhantomData,
	})
}
```


#### MockProver::run

If we look at the MockProver code, we see that an empty ConstraintSystem is created and passed to the configure function as mutable, then an immutable borrow is taken and worked with for the duration : 

```rust
let mut cs = ConstraintSystem::default();
let config = ConcreteCircuit::configure(&mut cs);
let cs = cs;
```

Thus it the Circuit's `configure` method that is responsible for all of the contraint system definition.  It could be called compile_constraints.  (负责所有约束系统定义。 它可以被称为`compile_constraints` )

```rust
impl<F: FieldExt> MockProver<F> {
    /// Runs a synthetic keygen-and-prove operation on the given circuit,  
    /// collecting data about the constraints and their assignments.

```

Why does the configure method accept an empty constraint system and return the arbitrary Self::Config, after filling the constaint system to its liking?   (为什么 configure 方法接受一个空的约束系统并在根据自己的喜好填充约束系统后返回任意的 `Self::Config` ? )

The config returned by `ConcreteCircuit::configure` is used just once in the MockProver: `ConcreteCircuit::FloorPlanner::synthesize(&mut prover, circuit, config, constants)`?;

So after setting up the constraints, the Config stores the information about the layout that the floor planner will need to synthesize.

The `configure` function is where we call cs.create_gate() and so on, adding polynomials to the system.  At this point, the variables in these polynomials are still locally named/scoped, and will be given a global name/scope during layout.

How to synthesize, given the circuit, a provided &mut Layouter and the data passed in the Config.  The synthesize method returns only error information, and changes state by writing to the Layouter.  Thus the Layouter is like a buffer or Writer to which the final circuit is written.  One will be supplied by the MockProver below (and variants will be needed during keygen and proving).

Roughly, configure provides the concrete but relative layout, and synthesize combines such blocks and assigns an absolute layout.

#### cs.create_gate

`cs.create_gate` takes a function from `virtual_cells` to contraints, pushing the constraints to the cs's accumulator. So this puts
```rust
(value.clone()) * (1 - value.clone()) * (2 - value.clone()) * ... * (R - 1 - value.clone())
```
into the constraint list.  ( `cs.create_gate` 从 `virtual_cells` 获取一个函数 take 到约束，将约束推送到 cs's accumulator , 所以这使得上式进入约束列表(constraint list)

```rust
pub fn create_gate<C: Into<Constraint<F>>, Iter: IntoIterator<Item = C>>(
	&mut self,
	name: &'static str,
	constraints: impl FnOnce(&mut VirtualCells<'_, F>) -> Iter,
) {
	let mut cells = VirtualCells::new(self);
	let constraints = constraints(&mut cells);
	let queried_selectors = cells.queried_selectors;
	let queried_cells = cells.queried_cells;

	let (constraint_names, polys): (_, Vec<_>) = constraints
		.into_iter()
		.map(|c| c.into())
		.map(|c| (c.name, c.poly))
		.unzip();

	assert!(
		!polys.is_empty(),
		"Gates must contain at least one constraint."
	);

	self.gates.push(Gate {
		name,
		constraint_names,
		polys,
		queried_selectors,
		queried_cells,
	});
}
```


### value

#### Value::known vs unknown

- it's basically a wrapper around an optional value where previously we were passing around values as option and they would be **Some** during `proving time` and **None** during `key gen` 


在 `halo2` 库中，`Value::known` 和 `Value::unknown` 是用于表示电路中的不同类型的值的方法。`halo2` 是一个用于编写零知识证明的 Rust 库，用于构建零知识证明系统中的电路和约束系统。

- `Value::known`: 这表示一个已知的值，也就是在电路运算过程中，该值是已知的，不需要进行计算。这种情况通常在电路的输入中使用，也就是你已经知道这些值。例如，证明一个加法运算的结果是已知的，可以使用 `Value::known` 来表示这个结果。
- `Value::unknown`: 这表示一个未知的值，也就是在电路运算中，该值是需要计算的，而不是直接提供。这种情况通常在电路内部使用，例如，在证明过程中计算的中间变量。`Value::unknown` 可以表示这些中间计算的结果，这些结果需要根据约束系统的规则进行计算。

这些方法用于在电路约束系统中表示和处理不同类型的值。在零知识证明系统中，约束系统会规定输入、中间变量和输出之间的关系，以确保计算的正确性和安全性。通过使用 `Value::known` 和 `Value::unknown`，你可以在电路中明确地表示这些不同类型的值，并将它们与约束系统的规则相匹配，以完成正确的计算和证明过程。

总之，`Value::known` 和 `Value::unknown` 是 `halo2` 中用于表示电路中不同类型值的方法，用于帮助构建零知识证明系统中的电路和约束系统。


### impl Circuit for xCircuit

#### without_witnesses
 - called only during keygen
 - the circuit without witnesses

#### synthesize

with takes the output of the 'configure' and the floorplanner to make the actual circuit
 - called at keygen and proving time
 - has copy constraints
 - this is the witness generation code, so you don't need to do much halo2-specific stuff except call assign once per region

##### assign_region

Assign a region of gates to an absolute row number.
 - Inside the closure, the chip may freely use relative offsets; the `Layouter` will treat these assignments as a single "region" within the circuit. 
 - Outside this closure, the Layouter is allowed to optimise as it sees fit.

`q_range_check` : 
```rust
struct MyConfig<F: FieldExt, const RANGE: usize> {
    advice_column: Column<Advice>,
    q_range_check: Selector,  // Attention 
    _marker: PhantomData<F>,
}

//..
layouter.assign_region(
	|| "Assign value", // the name of the region
	|mut region| {
		let offset = 0;
        config.q_range_check.enable(&mut region, offset)?;
```
Enable q_range_check. Remember that q_range_check is a label, a Selector.  Calling its enable
- calls `region.enable_selector(_,q_range_check,offset)`  which
- calls enable_selector on the region's RegionLayouter which
- calls enable_selector on its "CS" (actually an `Assignment<F>` (a trait), and whatever impls that does the work, for example for MockProver the enable_selector function does some checks and then sets  `self.selectors[selector.0][row] = true;`





Reference : 
 - https://github.com/jasonmorton/halo2-examples/blob/master/src/range_check/example1b.rs
 - 