> example 3.rs

### Goal 

we can look up smaller ranges, for example our lookup table right now is eight bits, but using a `single lookup table` we can constrain a one bit value,  or two bit value,  3-bit value and we don't always have to be constraining the maximum range of 8-bits.

our range is 8 bits, but we want to perform a range check on 4 bits.

### Pre-requisites

`value in (1 << (num_bits - 1))..(1 << num_bits)` 这个范围的意思是？

这个范围是用来确定在给定的二进制位数`num_bits`下，一个数字可以表示的最小值和最大值

1. **(1 << (num_bits - 1))**: 这是取 2 的`num_bits - 1`次幂。在计算机编程中，`<<` 是左移操作，它等同于将 1 乘以 2 的给定次幂。结果是该位数的最小值。
2. **(1 << num_bits)**: 这是取 2 的`num_bits`次幂，代表了在给定的`num_bits`位数下的最大值 +1 （因为范围的上限是不包含的）

- 如果`num_bits = 3`:
    - 最小值是 `1 << (3-1) = 4
	    - `1 << 2 ` 即 1 左移 2 位 : 
		    - 当我们左移一位（`1 << 1`）时， `0001` 变成了 `0010`  即 `2 `
		    - 当我们左移二位（`1 << 2`）时， `0001` 变成了 `0100` 即 `4`
		    - 当我们左移三位（`1 << 3`）时， `0001` 变成了 `1000` 即 `8`
    - 上限值是 `1 << 3` = 8 (不含 8) 因此，该范围表示的数字集合为 `{4, 5, 6, 7}`
    - eg.`100` 是 4 , `111` 是 `4+2+1 = 7 `

- 如果`num_bits = 4`:
    - 最小值是 `1 << (4-1)`s = 8
    - 上限值是 `1 << 4` = 16(不含)   因此，该范围表示的数字集合为 `{8, 9, 10, ..., 15}`
    - eg. `1000` 是 8 , `1111` 是  `8+4+2+1 = 15 `

这个范围用于确保在给定的 `num_bits` 下，数字的值在预期的最小和最大之间

### table

`src/range_check/example2/table.rs`
#### struct RangeTableConfig

```rust
// example 2
pub(super) struct RangeTableConfig<F: FieldExt, const RANGE: usize> {
    pub(super) value: TableColumn, 
    _marker: PhantomData<F>,
}

// example 3
pub(super) struct RangeTableConfig<F: FieldExt, const NUM_BITS: usize, const RANGE: usize> {
    pub(super) num_bits: TableColumn, // tag for our table.
    pub(super) value: TableColumn,
    _marker: PhantomData<F>,
}
```

加一 Advice col： `num_bits` 
```rust
struct RangeCheckConfig<F: FieldExt, const NUM_BITS: usize, const RANGE: usize> {
    q_lookup: Selector,
    num_bits: Column<Advice>, /////// 
    value: Column<Advice>,
    table: RangeTableConfig<F, NUM_BITS, RANGE>,
}
```

#### impl RangeCheckConfig 

```rust
impl<F: FieldExt, const NUM_BITS: usize, const RANGE: usize> RangeCheckConfig<F, NUM_BITS, RANGE> {
  pub fn configure(
    meta: &mut ConstraintSystem<F>,
    num_bits: Column<Advice>,
    value: Column<Advice>,
  ) -> Self {
    let q_lookup = meta.complex_selector();
    let table = RangeTableConfig::configure(meta);

    meta.lookup(|meta| {
      let q_lookup = meta.query_selector(q_lookup);
      let num_bits = meta.query_advice(num_bits, Rotation::cur());
      let value = meta.query_advice(value, Rotation::cur());

      let not_q_lookup = Expression::Constant(F::one()) - q_lookup.clone();
      let default_num_bits = Expression::Constant(F::one()); // 1-bit
      let default_value = Expression::Constant(F::zero());  // 0 is a 1-bit value

      let num_bits_expr =
        q_lookup.clone() * num_bits + not_q_lookup.clone() * default_num_bits;
      let value_expr = q_lookup * value + not_q_lookup * default_value;

      vec![(num_bits_expr, table.num_bits), (value_expr, table.value)]

      // This is `num_bits` when q_lookup = 1
      // and `default_num_bits` when not_q_lookup=1 

      // THIS IS BROKEN!!!!!!
      // Hint: consider the case where q_lookup = 0. What are our input expressions to the lookup argument then?
      // vec![
      //   (q_lookup.clone() * num_bits, table.num_bits),
      //   (q_lookup * value, table.value),
      // ]

      // when q_lookup = 0, we lookup(0,0), but we WANT (1,0)
      // Solution: looks up (num_bits, value) = (1, 0) when q_lookup = 0.
    }); 

    Self {
      q_lookup,
      num_bits,
      value,
      table,
    }
  }
```


Tips: right now halo2 only allows fixed columns to be used as lookup tables and the reason is that behind the scenes um halo 2 will pad your lookup table for
