<!-- toc -->
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



#### without_witnesses
 - called only during keygen
 - the circuit without witnesses

#### synthesize

with takes the output of the 'configure' and the floorplanner to make the actual circuit
 - called at keygen and proving time
 - has copy constraints
 - this is the witness generation code, so you don't need to do much halo2-specific stuff except call assign once per region
