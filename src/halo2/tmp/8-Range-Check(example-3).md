> author: [@Demian](https://github.com/Demian101)
> 
> references: https://learn.0xparc.org/materials/halo2/learning-group-1/exercise-3

[TOC]
### Goal 

we can look up smaller ranges, for example our lookup table right now is eight bits, but using a `single lookup table` we can constrain a one bit value,  or two bit value,  3-bit value and we don't always have to be constraining the maximum range of 8-bits.

our range is 8 bits, but we want to perform a range check on 4 bits.

### Overview

文件架构：
```rust
├── range_check
│   ├── example3
│   │   └── table.rs  // lookup table
│   ├── example3.rs   // main config
```

本部分代码的更新：
- Table：
	- `num_bits: TableColumn`  RangeTableConfig 中新增一列
### Pre-requisites

`value in (1 << (num_bits - 1))..(1 << num_bits)` 这个范围的意思是？

这个范围是用来确定在给定的二进制位数`num_bits`下，一个数字可以表示的最小值和最大值
- **(1 << (num_bits - 1))**: 这是取 2 的`num_bits - 1`次幂。在计算机编程中，`<<` 是左移操作，它等同于将 1 乘以 2 的给定次幂。结果是该位数的最小值。
- **(1 << num_bits)**: 这是取 2 的`num_bits`次幂，代表了在给定的`num_bits`位数下的最大值 +1 （因为范围的上限是不包含的）

如果`num_bits = 3`:
- 最小值是 `1 << (3-1) = 4
	- `1 << 2 ` 即 1 左移 2 位 : 
		- 当我们左移一位（`1 << 1`）时， `0001` 变成了 `0010`  即 `2 `
		- 当我们左移二位（`1 << 2`）时， `0001` 变成了 `0100` 即 `4`
		- 当我们左移三位（`1 << 3`）时， `0001` 变成了 `1000` 即 `8`
- 上限值是 `1 << 3` = 8 (不含 8) 因此，该范围表示的数字集合为 `{4, 5, 6, 7}`
- eg.`100` 是 4 , `111` 是 `4+2+1 = 7 `

如果`num_bits = 4`:
- 最小值是 `1 << (4-1)`s = 8
- 上限值是 `1 << 4` = 16(不含)   因此，该范围表示的数字集合为 `{8, 9, 10, ..., 15}`
- eg. `1000` 是 8 , `1111` 是  `8+4+2+1 = 15 `

这个范围用于确保在给定的 `num_bits` 下，他所能表示的数字的值在预期的最小和最大之间

### lookup table - table.rs

`src/range_check/example2/table.rs`
#### struct RangeTableConfig

对比 `example-2` 和 `example-3` ： 

```rust
// example 2
pub(super) struct RangeTableConfig<F: FieldExt, const RANGE: usize> {
    pub(super) value: TableColumn, 
    _marker: PhantomData<F>,
}

// example 3
/// A lookup table of values up to RANGE
/// e.g. RANGE = 256, values = [0..255]
/// This table is tagged by an index `k`, where `k` is the number of bits of the element in the `value` column.
pub(super) struct RangeTableConfig<F: FieldExt, const NUM_BITS: usize, const RANGE: usize> {
    pub(super) num_bits: TableColumn, // tag for our table.
    pub(super) value: TableColumn,
    _marker: PhantomData<F>,
}
```

#### impl RangeTableConfig {..

##### fn configure()

```rust
impl<F: PrimeField, const NUM_BITS: usize, const RANGE: usize> RangeTableConfig<F, NUM_BITS, RANGE> {
    pub(super) fn configure(meta: &mut ConstraintSystem<F>) -> Self {
        assert_eq!(1 << NUM_BITS, RANGE);  // "1" 左移一位 NUM_BITS 位, 即变大 2^NUM_BITS 倍

        let num_bits = meta.lookup_table_column();
        let value = meta.lookup_table_column();

        Self {
            num_bits,
            value,
            _marker: PhantomData,
        }
    }
```

##### fn load()

1. 赋值 2 列 lookup column 的首行为：`(num_bits = 1, value = 0)`
	1. `0` 的二进制编码为 `0`，占 1 位
2. 后面就是在给定 NUM_BITS 下能表示的上下界中赋值：
	1. 如 `value=2` ，那么 `num_bits=2` （∵ 2 的二进制表示是 `10` ，有 2 位）；
	2. 如 `value=8`，那么`num_bits=4`（∵ 8 的二进制表示是 `1000`，有 4 位）。
	3. 简而言之，`num_bits` 描述了表示 `value` 所需的最小位数。

前面也提到，这个范围用于确保在给定的 `num_bits` 下，数字的值在预期的最小和最大之间：

如果`num_bits = 4`:
- 最小值是 `1 << (4-1)`s = 8
- 上限值是 `1 << 4` = 16(不含)   因此，该范围表示的数字集合为 `{8, 9, 10, ..., 15}`
- eg. `1000` 是 8 , `1111` 是  `8+4+2+1 = 15 `

```rust
pub(super) fn load(&self, layouter: &mut impl Layouter<F>) -> Result<(), Error> {
	layouter.assign_table(
		|| "load range-check table",
		|mut table| {
			let mut offset = 0;

			// Assign (num_bits = 1, value = 0), 2 列都是 lookup columns.
			// 这部分是赋值首行, 为 num_bits 和 value 分配了其首个值，即 1 和 0, 方便下面累加
			{
				table.assign_cell(
					|| "assign num_bits",
					self.num_bits,
					offset,
					|| Value::known(F::ONE),
				)?;
				table.assign_cell(
					|| "assign value",
					self.value,
					offset,
					|| Value::known(F::ZERO),
				)?;

				offset += 1;
			}

			// (1 << (num_bits_ - 1))..(1 << num_bits_) : 在给定的 NUM_BITS 下的 min & max value.
			//   num_bits_ 标识了 value 所占的位数,比如 213
			//   value_ 则是实际赋值(约束)到电路里的实际 Private value
			for num_bits_ in 1..=NUM_BITS {
				for value_ in (1 << (num_bits_ - 1))..(1 << num_bits_) {
					table.assign_cell(
						|| "assign num_bits",
						self.num_bits,
						offset,
						|| Value::known(F::from(num_bits_ as u64)),
					)?;
					table.assign_cell(
						|| "assign value",
						self.value,
						offset,
						|| Value::known(F::from(value_ as u64)),
					)?;
					offset += 1;
				}
			}
			Ok(())
		},
	)
} }
```


### main - example3.rs

#### Overview 

This helper uses a lookup table to check that the value witnessed in a given cell is
within a given range.

The lookup table is tagged by `num_bits` to give a strict range check.

```
       value     |   q_lookup  |  table_num_bits  | lookup table_value  |
      -------------------------------------------------------------
         v_0     |      0      |        1         |       0       |
         v_1     |      1      |        1         |       1       |
         ...     |     ...     |        2         |       2       |
         ...     |     ...     |        2         |       3       |
         ...     |     ...     |        3         |       4       |
```

We use a K-bit lookup table, that is tagged `1..=K`, where the tag `i` marks an `i`-bit value.
使用 K 位查找表，标记为 `1 ..= K`，其中标记 `i` 标记 `i` 位值


#### structs

- `RangeConstrained` 加一字段：`num_bits: AssignedCell<Assigned<F>, F>,`
- `RangeCheckConfig` 加一 Advice col：`num_bits`

```rust
#[derive(Debug, Clone)]
/// A range-constrained value in the circuit produced by the RangeCheckConfig.
struct RangeConstrained<F: PrimeField> {
    num_bits: AssignedCell<Assigned<F>, F>,
    assigned_cell: AssignedCell<Assigned<F>, F>,
}

struct RangeCheckConfig<F: FieldExt, const NUM_BITS: usize, const RANGE: usize> {
    q_lookup: Selector,
    num_bits: Column<Advice>, /////// 
    value: Column<Advice>,
    table: RangeTableConfig<F, NUM_BITS, RANGE>,
}
```

#### impl RangeCheckConfig

当 `q_lookup`（complex_selector）被激活或禁用时，应该使用哪些值进行查找
 - `q_lookup`和`not_q_lookup`分别表示查找被激活或禁用的情况
 - 默认： Selector 不被激活时，直接使用位数为 1 的默认值和值为 0 的默认值，确保约束成立
 - 当激活 Selector，它将使用提供的 Advice col 中的实际值
 - 这确保了当不需要范围检查时，查找仍然是有效的，并且查找表中有相应的条目。

`vec![(num_bits_expr, table.num_bits), (value_expr, table.value)]` :
 - 2 个约束 `(num_bits_expr, table.num_bits), (value_expr, table.value)` 都需要成立
 - 在 `meta.lookup` -> `query_fixed` -> `query_fixed_index` 源码中，可以看到：
	 - `value_expr` 会被循环遍历是否在 `table.value` 这个 `fixed` 列中存在

##### fn configure()

```rust
// Write the gate for our range check Config
// It's good practive to pass advice columns to the config (rather than creating it within the config)
// because these are very likely to be shared across multiple config
impl<F: PrimeField, const NUM_BITS: usize, const RANGE: usize> RangeCheckConfig<F, NUM_BITS, RANGE> {
   // REMEMBER THAT THE CONFIGURATION HAPPEN AT KEYGEN TIME
   pub fn configure(
      meta: &mut ConstraintSystem<F>,
      num_bits: Column<Advice>,
      value: Column<Advice>,
   ) -> Self {
      let q_lookup = meta.complex_selector();  // complex_selector
      // 配置查找表 configure lookup table.
      let table = RangeTableConfig::configure(meta);

      meta.lookup(|meta| {
         let q_lookup = meta.query_selector(q_lookup);
         let num_bits = meta.query_advice(num_bits, Rotation::cur());
         let value = meta.query_advice(value, Rotation::cur());

         // q_lookup = 1, not_q_lookup = 0 ; q_lookup = 0, not_q_lookup = 1
         let not_q_lookup = Expression::Constant(F::ONE ) - q_lookup.clone();
         let default_num_bits = Expression::Constant(F::ONE);// 1-bit
         let default_value = Expression::Constant(F::ZERO);  // 0 is a 1-bit value
 
         // default_num_bits / default_value only used when `q_lookup` is not active.
         let num_bits_expr =
            q_lookup.clone() * num_bits + not_q_lookup.clone() * default_num_bits;
         let value_expr = q_lookup * value + not_q_lookup * default_value;

         // When q_lookup is active, the circuit will use the actual advice values, 
         //   but when it's not, the circuit will use the default values.
         // 根据 meta.lookup 源码(query_fixed_index), 我们需要确保:
         //  - num_bits_expr ∈  table.num_bits 和
         //  - value_expr ∈ table.value  都成立
         vec![(num_bits_expr, table.num_bits), (value_expr, table.value)]
        }); 

      Self {
          q_lookup,
          num_bits,
          value,
          table,
     }
   }
```


Tips: right now halo2 only allows fixed columns to be used as lookup tables and the reason is that behind the scenes um halo 2 will pad your lookup table for...

##### fn assign()

将某些值 (如 Private value) 分配到特定的电路区域内

```rust
pub fn assign(
   &self,
   mut layouter: impl Layouter<F>,
   num_bits: Value<u8>,
   value: Value<Assigned<F>>,
) -> Result<RangeConstrained<F>, Error> {
   layouter.assign_region(
      || "Assign value",
      |mut region| {
         let offset = 0;

         // Enable q_lookup
         self.q_lookup.enable(&mut region, offset)?;

         // Assign num_bits
         let num_bits = num_bits.map(|v| F::from(v as u64));
         let num_bits = region.assign_advice(
            || "num_bits",
            self.num_bits,
            offset,
            || num_bits.into(),
         )?;

         // Assign value
         let assigned_cell =
            region.assign_advice(|| "value", self.value, offset, || value)?;

         Ok(RangeConstrained {
            num_bits,
            assigned_cell,
         })
      },
   )
} }
```

#### Test

和上一节类似

```rust
#[test]
fn test_range_check_3() {
	let k = 9;
	const NUM_BITS: usize = 8;
	const RANGE: usize = 256; // 8-bit value

	// Successful cases
	for num_bits in 1u8..=NUM_BITS.try_into().unwrap() {
		for value in (1 << (num_bits - 1))..(1 << num_bits) {
			let circuit = MyCircuit::<Fp, NUM_BITS, RANGE> {
				num_bits: Value::known(num_bits),
				value: Value::known(Fp::from(value as u64).into()),
			};

			let prover = MockProver::run(k, &circuit, vec![]).unwrap();
			prover.assert_satisfied();
		}
	}
}
```


### Usage

```bash
cargo test -- --nocapture test_range_check_3

# Draw
cargo test --release --all-features print_range_check_3
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
