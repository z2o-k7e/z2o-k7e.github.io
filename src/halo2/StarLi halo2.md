第一代 zkp 算法:  groth16 , groth16 算法的每一个新的业务, 每次迭代都需要重新做 Setup, 模式太重, 需要强信任

后面的 universal setup 优化: 只需要做一次 Setup, 所有的业务都可以使用. 最初代的 universal setup 算法是 Sonic, poly-com 用了 KZG, 但是性能不够

19 年 ZCash 团队设计出了 Halo 算法, 使用了 Inner Product argument 替换 KZG, 把实现 universal Setup 的条件更进一步削弱了, 改成了不需要 Setup . Halo2 不光想支持 SNARK 算法, 他更想支持递归特性, 而因为 Sonic 的验证是线性的, 所以其提出了一套 `嵌套均摊` (nested amortization) 技术

History : 
- Sonic : 支持 universal Setup 的 SNARK (性能不够)
- Halo: Sonic +  Inner Product argument  + `嵌套均摊` (nested amortization) 技术
- PLONK : 对 Sonic 的改进
- Plookup : PLONK + lookup table
- UltraPLONK : Plookup + custom gate  (需要 Setup)
- Halo2 : UltraPLONK + `嵌套均摊` (nested amortization) 技术  (无需 Setup)
	- 通过 nested amortization 将递归证明的 overhead 降低

总结下 : 
 - Groth16 : 第一代 Pairing-based zk-SNARKs 
 - PLONK : 第二代 Pairing-based zk-SNARKs (universal $Setup$ )
 - Halo2 : 第三代 zkp 技术, YYDS ! 
	 - 不需要 Setup 
	 - 不需要配对曲线, 只需要一个支持离散对数的曲线, 需要的安全性假设较弱
	 - 支持递归

### Concepts

The arithmetization used by Halo 2 comes from [PLONK](https://eprint.iacr.org/2019/953), or more precisely its extension UltraPLONK that supports custom gates and lookup arguments. We'll call it [_**PLONKish**_](https://twitter.com/feministPLT/status/1413815927704014850).

#### Inner Product Argument

星想法对 [Inner Product Argument] 的介绍

- No Setup
- 

#### Custom Gate

Custom Gate 可以引用同一列的多行 Cell, 类似于 d 、 d_next 的作用
 - 代价 : 证明中需要多增加一个 $f(\omega x)$ 即多增加一个 Fr 
Custom Gate 的表达式是一个多元高次多项式, 即, 最高次数可以 >2 
 - Standard PLONK : Degree = 2 
每一个 Custom Gate 都有一个对应的 selector column 与其对应

Custom Gate 只能够引用相对偏移的 cells, 偏移值的类型是 $\textcolor{red}{Rotation}$

![[Pasted image 20230822142938.png]]
一个 Custom Gate 可以涉及到若干个 advice/Instance/fixed/selector 
它可以引用相对位置(上 1 行, 上 2 行, 下 1 行, 下 2 行) 

如上图有 2 个 gates, 对于 $Gate_0$ , 其 $S_0$  设置为 1 , 则需要满足 $Custom \ \ Gate_0$
对于下面那个, $S_0,  \ \ S_1$ 都是 1 , 则 Custom Gate 0 / 1 都需要满足





### Halo2 Arithmetization

- Halo 里用 R1CS 来表述 Circuit，模式如: $A * B = C$
- Halo2 里用 Plonk Arithmetization 来表示 Circuit，模式如： $qL * Xa + qR * Xb + qO * Xc + qM * XaXb + qC = 0$

其中，$q$  为 selector polynomial, 不同的取值，代表不同的 Gate，如下表所示：



### Halo2 编程 

![[Pasted image 20230822150314.png]]


### 电路进阶 (sha256 优化实现)

- Groth16 + R1CS 实现 SHA256 :  
	- 约需要 2.5W 行 R1CS 约束 ...
- Halo2 + Custom Gate + lookup table  : 
	-  2099 行  (影响的是多项式的次数 : Groth16 中多项式的次数至少是 25000, 这里只需要 2099 次数)
	- 11 advice columns
	- 3 fixed constant columns
	- 20 Custom Gates.
	- 20 Selector columns  (控制 Custom Gates.)
	- Spread Table 使用了 $2^{16}$  行  ( 使用的是 u16, 所以是  $2^{16}$  行)
		- 为什么  $2^{16}$ 远大于 2099 ?

### Halo2电路构建源代码导读

 - PLONK/Groth16 约束系统由一条条相对独立的约束组成（约束只能约束该行上的变量），并且一条约束指定了支持的计算（乘法或者加法组合）
 - Halo2 的约束系统中的约束并不限定在一行上的变量，并且采用“custom gate”，任意指定约束需要的计算

Cell : Cell指定在一个Region中的某行某列 , 其定义在 `src/circuit.rs` :

```rust
pub struct Cell {  
    region_index: RegionIndex,  
    row_offset: usize,  
    column: Column<Any>,  
}
```

Region : 
 - 给 Cell 进行配置的时候, 是需要按照一个 Region 来的, Region 可以认为是小的功能模块, Region 在论文里是没有的, 是为了方便开发实现加进去的逻辑.
 - 在一个 Region 里面的逻辑可以 copy, 在写电路的时候看到的是一个一个的 Region, 在生成另外一个 Region 的时候, 可以把相同的逻辑做平移, 因为都是使用的 `相对坐标` 
	 - (为了更好的电路模块化，电路构建往往是相对于一个 Region, 同一个电路模块，可以在不同的 Region 中“复制”)
 - 开发人员写代码关注的事 Region 里的实现 —— 在 Region 里干什么事, 然后 Region 会自己找合适的地方放置这些 Chip

#### Chip/cfig/instruction/Layouter

 - **Chip(芯片)**  :  电路由一个个 Chip 逻辑堆砌而成。每个 Chip 的创建从 “Config”开始。
 - **Config**  :  所谓的 Config，就是申请Chip需要的Column以及配置Fixed列的逻辑含义。这些配置可能是 Custom Gate，可能是 lookup。
 - **Instructions**  : 每一个 Chip 都有指令（Instruction）。通过指令的调用，将 Chip 的部分或者全部功能加入电路中
 - **Layouter**
	 - Layouter 做的事就是根据需求, 找到能放下 Chip 的空地, 把他放置上去, 类似在一个矩形上画东西: 把 Chip 放到画布上, 就像华容道, 组出来的矩阵可大可小, 有好有坏... 
	 - 将 Chip 添加到一个电路上需要布局。Layouter 就是用来实现布局。Layouter 接口定义在src/circuit.rs中：

Layouter 本身存在层级关系，所以 Layouter Interface 定义了`get_root / push_namespace / pop_namespace / namespace` 等相关的函数。核心逻辑在其他三个函数：

- `assign_region` - 获取一个 region，在这个 region 上可以进行各种“assignment”(赋值)，定义在 `RegionLayouter` 接口中
- `assign_table` - 获取一个 table，并设置 table，接口定义在 `TableLayouter` 接口中。
- `constrain_instance` - 限制一个 Cell 和 Instance 列中的某个 Cell 一致

```rust
pub trait Layouter<F: Field> {  
    type Root: Layouter<F>;  
      
    fn assign_region<A, AR, N, NR>(&mut self, name: N, assignment: A) -> Result<AR, Error>  
    where  
        A: FnMut(Region<'_, F>) -> Result<AR, Error>,  
        N: Fn() -> NR,  
        NR: Into<String>;  
  
    fn assign_table<A, N, NR>(&mut self, name: N, assignment: A) -> Result<(), Error>  
    where  
        A: FnMut(Table<'_, F>) -> Result<(), Error>,  
        N: Fn() -> NR,  
        NR: Into<String>;  
  
    fn constrain_instance(  
        &mut self,  
        cell: Cell,  
        column: Column<Instance>,  
        row: usize,  
    ) -> Result<(), Error>;  
  
    fn get_root(&mut self) -> &mut Self::Root;  
    fn push_namespace<NR, N>(&mut self, name_fn: N)  
    where  
        NR: Into<String>,  
        N: FnOnce() -> NR;  
    fn pop_namespace(&mut self, gadget_name: Option<String>);  
    fn namespace<NR, N>(&mut self, name_fn: N) -> NamespacedLayouter<'_, F, Self::Root>  
    where  
        NR: Into<String>,  
        N: FnOnce() -> NR,  
    {  
        self.get_root().push_namespace(name_fn);  
  
        NamespacedLayouter(self.get_root(), PhantomData)  
    }  
}
```

#### **Assignment**

 - 前面提到, 布局的过程中用的 Region, 但是其被放置的画布是一个全局的矩阵, 在 synthesize 完成后, 是要对所有的 Cells 进行赋值的
 - 电路“赋值”的接口 就是 **Assignment** ，定义在 `src/plonk/circuit.rs` ：
 - **Assignment** 处理的东西比较 Tricky: 
	 - 它既要知道 Region 信息(Region 用的是一些 relative 相对信息)
	 - 又要把 Region 信息翻译 Translate 成全局信息, 因为最后结果是全局的, 中间的 Assign 过程是曲折的, 是通过 Region 进行转换的 ...


### 电路内部框架

为了方便 Devs 开发电路，Halo2 的内部抽象了布局的接口 , 目前有 2 套 Layouter 的实现：
1. `SimpleFloorPlanner`
2. `V1/V1Plan`  为了理解 Halo2 的内部逻辑，这里只详细讲解 `SimpleFloorPlanner` : 

![[Pasted image 20230824151908.png]]


如上图, `SimpleFloorPlanner` 处在最核心高层, 是一个更高阶的管理器 , `Layouter` 是水平布局器, 基本上只负责一层的构建 

整个框架由四个接口组成：`FloorPlanner`，`Layouter`，`RegionLayout/TableLayout `以及Assignment

简单的说，一个 FloorPlanner 拥有一个 Layouter，一个 Layouter 可以分配多个 RegionLayout 或者TableLayout。电路对 Cell 的 assignment 通过 Assignment 实现

先从三者的整体调用关系讲起：
- *SimpleFloorPlanner* 是对 *FloorPlanner* 接口的实现，定义在 `src/circuit/floor_planner/single_pass.rs` 中：

```rust
pub struct SimpleFloorPlanner;

// SimpleFloorPlanner实现了FloorPlanner trait 的synthesize函数。
// 容易看出，该函数创建出SingleChipLayouter对象，并直接调用相应的synthesize函数开始电路的synthesize
impl FloorPlanner for SimpleFloorPlanner {
    fn synthesize<F: Field, CS: Assignment<F>, C: Circuit<F>>(
        cs: &mut CS,
        circuit: &C,
        config: C::Config,
        constants: Vec<Column<Fixed>>,
    ) -> Result<(), Error> {
        let layouter = SingleChipLayouter::new(cs, constants)?;
        circuit.synthesize(config, layouter)
    }
}
```

`SingleChipLayouter` 定义在 `src/circuit/floor_planner/single_pass.rs` 中，包括了电路的所有的信息。

```rust
pub struct SingleChipLayouter<'a, F: Field, CS: Assignment<F> + 'a> {
    cs: &'a mut CS,
    constants: Vec<Column<Fixed>>,
    /// Stores the starting row for each region.
    regions: Vec<RegionStart>,
    /// Stores the first empty row for each column.
    columns: HashMap<RegionColumn, usize>,
    /// Stores the table fixed columns.
    table_columns: Vec<TableColumn>,
    _marker: PhantomData<F>,
}
```

`SingleChipLayouter` 的 Region 管理比较简单，某行整体属于某个 Region 。
 - regions 记录每个 Region 的行的开始偏移(starting row)。
 - cs 是 Assignment 接口的实现，存储所有电路的赋值信息。
 - columns 记录当前操作 RegionColumn 对应的空的 row 的偏移。table_columns 记录 Table 需要的 Fixed 的 Column。
 - 简单的说，`SingleChipLayouter` 记录了电路（包括布局）需要的所有信息。

SingleChipLayouter's **assign_region** 函数实现一个 Region 的 synthesize 过程。简单的说，SingleChipLayouter 的 assign_region 函数的主要逻辑就是创建一个 region ，并将 region 内的布局转化为全局布局。SingleChipLayouter 的 `assign_region` 逻辑可以分成两部分：

1. 通过 RegionShape 获取 Region 的“形状”。所谓的“形状”，包括主要是采用的 Column 的信息
2. 根据上一步获取的 Column 信息，找出和其他 Region 不冲突的起始问题。

```rust
// 
fn assign_region<A, AR, N, NR>(&mut self, name: N, mut assignment: A) -> Result<AR, Error>
where
    A: FnMut(Region<'_, F>) -> Result<AR, Error>,
    N: Fn() -> NR,
    NR: Into<String>,
{
    let region_index = self.regions.len(); // 获取当前Region的编号

    // Get shape of the region. 这个 Region 有几行几列?
    // 调用 RegionShape 的 Region 接口，收集该 Region 的电路涉及到的 Column 信息
    let mut shape = RegionShape::new(region_index.into());
    {
        let region: &mut dyn RegionLayouter<F> = &mut shape;
        assignment(region.into())?;
    }

    // Lay out this region. We implement the simplest approach here: position the
    // region starting at the earliest row for which none of the columns are in use.
    let mut region_start = 0; //根据收集到的Column信息，获取Region开始的行号
    for column in &shape.columns {
        region_start = cmp::max(region_start, self.columns.get(column).cloned().unwrap_or(0));
    }
    self.regions.push(region_start.into());

    // Update column usage information. //在Region中记录下所有使用的Column的信息
    for column in shape.columns {
        self.columns.insert(column, region_start + shape.row_count);
    }

    // Assign region cells.
    self.cs.enter_region(name); //创建Region
    let mut region = SingleChipLayouterRegion::new(self, region_index.into());
    let result = {
        let region: &mut dyn RegionLayouter<F> = &mut region;
        assignment(region.into()) //采用SingleChipLayouterRegion对电路赋值
    }?;
    let constants_to_assign = region.constants;
    self.cs.exit_region(); //退出Region

    // Assign constants. For the simple floor planner, we assign constants in order in
    // the first `constants` column.
    if self.constants.is_empty() {//如果制定了constants，需要增加置换限制
        if !constants_to_assign.is_empty() {
            return Err(Error::NotEnoughColumnsForConstants);
        }
    } else {
        let constants_column = self.constants[0];
        let next_constant_row = self
            .columns
            .entry(Column::<Any>::from(constants_column).into())
            .or_default();
        for (constant, advice) in constants_to_assign {
            self.cs.assign_fixed(
                || format!("Constant({:?})", constant.evaluate()),
                constants_column,
                *next_constant_row,
                || Ok(constant),
            )?;
            self.cs.copy(
                constants_column.into(),
                *next_constant_row,
                advice.column,
                *self.regions[*advice.region_index] + advice.row_offset,
            )?;
            *next_constant_row += 1;
        }
    }

    Ok(result)
}
```

类似 0XPARC 里提到的, 即使第二行可以放置一个 Region, 但是 `SimpleFloorPlanner` 管不了那么多, 我就是往后一行放 ... (这个电路是一直往下的, 不会向左找找, 向右找找, 是比较初级的)


SingleChipLayouter's `assign_table`  : 当前的电路中增加查找表逻辑：

```rust
fn assign_table<A, N, NR>(&mut self, name: N, mut assignment: A) -> Result<(), Error>  
where  
    A: FnMut(Table<'_, F>) -> Result<(), Error>,  
    N: Fn() -> NR,  
    NR: Into<String>,  
{  
    // Maintenance hazard: there is near-duplicate code in `v1::AssignmentPass::assign_table`.  
    // Assign table cells.  
    self.cs.enter_region(name); //创建一个Region  
    let mut table = SimpleTableLayouter::new(self.cs, &self.table_columns);  
    {  
        let table: &mut dyn TableLayouter<F> = &mut table;  
        assignment(table.into()) //查找表赋值  
    }?;  
    let default_and_assigned = table.default_and_assigned;  
    self.cs.exit_region(); //退出当前Region  
  
    // Check that all table columns have the same length `first_unused`,  
    // and all cells up to that length are assigned.  
    let first_unused = { //获取出所有查找表相关的Column对应的最大使用行数  
        match default_and_assigned  
            .values()  
            .map(|(_, assigned)| {  
                if assigned.iter().all(|b| *b) {  
                    Some(assigned.len())  
                } else {  
                    None  
                }  
            })  
            .reduce(|acc, item| match (acc, item) {  
                (Some(a), Some(b)) if a == b => Some(a),  
                _ => None,  
            }) {  
            Some(Some(len)) => len,  
            _ => return Err(Error::SynthesisError), // TODO better error  
        }  
    };  
  
    // Record these columns so that we can prevent them from being used again.  
    for column in default_and_assigned.keys() {  
        self.table_columns.push(*column);  
    }  
    //根据default_and_assigned信息，采用default值扩展所有的column  
    for (col, (default_val, _)) in default_and_assigned {  
        // default_val must be Some because we must have assigned  
        // at least one cell in each column, and in that case we checked  
        // that all cells up to first_unused were assigned.  
        self.cs  
            .fill_from_row(col.inner(), first_unused, default_val.unwrap())?;  
    }  
  
    Ok(())  
}
```

`default_and_assigned` 记录了在一个 Fixed Column 上的 default 值以及某个 cell 是否已经设置

`SingleChipLayouterRegion` 实现了 Region 接口。如果在 Region 中需要给一个 Advice 列中 Cell 赋值，可以采用 assign_advice 函数：

```RUST
fn assign_advice<'v>(  
    &'v mut self,  
    annotation: &'v (dyn Fn() -> String + 'v),  
    column: Column<Advice>,  
    offset: usize,  
    to: &'v mut (dyn FnMut() -> Result<Assigned<F>, Error> + 'v),  
) -> Result<Cell, Error> {  
    self.layouter.cs.assign_advice( //调用Assignment接口设置相应的Cell信息，特别注意的是在设置的时候需要Cell的全局偏移  
        annotation,  
        column,  
        *self.layouter.regions[*self.region_index] + offset,  
        to,  
    )?;  
  
    Ok(Cell {  
        region_index: self.region_index,  
        row_offset: offset,  
        column: column.into(),  
    })  
}
```

`cs.assign_fixed` 函数，对 fixed Column 进行赋值。可以参考 MockProver 的实现(src/dev.rs)
 - 这个 cs 不是 constrain System , 只是 Assignment 的接口 : MockProver

```rust
fn assign_fixed<V, VR, A, AR>(
    &mut self,
    _: A,
    column: Column<Fixed>,
    row: usize,
    to: V,
) -> Result<(), Error>
where
    V: FnOnce() -> Result<VR, Error>,
    VR: Into<Assigned<F>>,
    A: FnOnce() -> AR,
    AR: Into<String>,
{
    ... //在一些检查后，设置Fixed列中的某个Cell（column，row指定）
    *self
        .fixed
        .get_mut(column.index())
        .and_then(|v| v.get_mut(row))
        .ok_or(Error::BoundsFailure)? = CellValue::Assigned(to()?.into().evaluate());

    Ok(())
}
```

cs.copy 函数，增加置换信息 : 

```rust
fn copy(
    &mut self,
    left_column: Column<Any>,
    left_row: usize,
    right_column: Column<Any>,
    right_row: usize,
) -> Result<(), crate::plonk::Error> {
    if !self.usable_rows.contains(&left_row) || !self.usable_rows.contains(&right_row) {
        return Err(Error::BoundsFailure);
    }

    self.permutation //增加Permutation信息
        .copy(left_column, left_row, right_column, right_row)
}
```

接着我们再详细看看 RegionLayouter 和 TableLayouter 。RegionLayouter 定义在src/circuit/layouter.rs：

```rust
pub trait RegionLayouter<F: Field>: fmt::Debug {
    //enable选择子
    fn enable_selector<'v>(
        &'v mut self,
        annotation: &'v (dyn Fn() -> String + 'v),
        selector: &Selector,
        offset: usize,
    ) -> Result<(), Error>;

    //advice或者fixed赋值
    fn assign_advice<'v>(
        &'v mut self,
        annotation: &'v (dyn Fn() -> String + 'v),
        column: Column<Advice>,
        offset: usize,
        to: &'v mut (dyn FnMut() -> Result<Assigned<F>, Error> + 'v),
    ) -> Result<Cell, Error>;

    fn assign_advice_from_constant<'v>(
        &'v mut self,
        annotation: &'v (dyn Fn() -> String + 'v),
        column: Column<Advice>,
        offset: usize,
        constant: Assigned<F>,
    ) -> Result<Cell, Error>;

    fn assign_advice_from_instance<'v>(
        &mut self,
        annotation: &'v (dyn Fn() -> String + 'v),
        instance: Column<Instance>,
        row: usize,
        advice: Column<Advice>,
        offset: usize,
    ) -> Result<(Cell, Option<F>), Error>;

    fn assign_fixed<'v>(
        &'v mut self,
        annotation: &'v (dyn Fn() -> String + 'v),
        column: Column<Fixed>,
        offset: usize,
        to: &'v mut (dyn FnMut() -> Result<Assigned<F>, Error> + 'v),
    ) -> Result<Cell, Error>;

    //cell相等约束
    fn constrain_constant(&mut self, cell: Cell, constant: Assigned<F>) -> Result<(), Error>;

    fn constrain_equal(&mut self, left: Cell, right: Cell) -> Result<(), Error>;
}
```

`RegionLayouter` 的接口很容易理解，包括设置选择子，给cell赋值，约束cell相等。

`TableLayouter` 的接口定义如下：
```rust
pub trait TableLayouter<F: Field>: fmt::Debug {
    fn assign_cell<'v>(
        &'v mut self,
        annotation: &'v (dyn Fn() -> String + 'v),
        column: TableColumn,
        offset: usize,
        to: &'v mut (dyn FnMut() -> Result<Assigned<F>, Error> + 'v),
    ) -> Result<(), Error>;
}
```

TableLayouter 只有一个接口：assign_cell.  assign_cell 是对表中的某个TableColumn的Cell进行赋值。

至此，大体的电路构造的逻辑的框架相对清楚：Halo2 中的 Chip 电路由一个个 Region 组成，在Halo2 的框架中，Region 通过 Layouter 进行分配。电路的所有的信息都存储在 Assignment 的接口中。MockProver 是一个可以参考的 Assignment 的实现

### ConstraintSystem

```rust
pub struct ConstraintSystem<F: Field> {
    pub(crate) gates: Vec<Gate<F>>,
    // ...
```

gates 描述了 “Custom Gate” 的限制表达式：
```rust
#[derive(Clone, Debug)]  
pub(crate) struct Gate<F: Field> {  
    name: &'static str,  
    constraint_names: Vec<&'static str>,  
    polys: Vec<Expression<F>>,   // Attention !!
    /// We track queried selectors separately from other cells, so that we can use them to  
    /// trigger debug checks on gates.  
    queried_selectors: Vec<Selector>,  
    queried_cells: Vec<VirtualCell>,  
}
```

如上代码, 这个 polys 多项式表达的逻辑是你自己书写的, 而不是像 R1CS 中, 就那几个门都固化了, 必须按照这几个门的约束来写

Halo2的电路构建分为两部分：
1. Configure （电路配置）
2. Synthesize（电路(实例)综合）


### a^2 +b^2 示例

#### 1 **Configure** 过程

Configure 调用 ConstraintSystem 申请各种列以及 Gate 的信息。调用某个 Circuit 的 Configure 函数会顺序调用电路涉及到的 Chip 的 Configure 信息，这些信息都记录在 ConstraintSystem 中

查看实例中的 Chip 的 Configure 函数 : 

```rust
impl<F: FieldExt> Circuit<F> for MyCircuit<F> {
    ...
    fn configure(meta: &mut ConstraintSystem<F>) -> Self::Config {
        let advice = [meta.advice_column(), meta.advice_column()];
        let instance = meta.instance_column();
        let constant = meta.fixed_column();

        FieldChip::configure(meta, advice, instance, constant)
    }
    ...
}

impl<F: FieldExt> FieldChip<F> {
    fn construct(config: <Self as Chip<F>>::Config) -> Self {
        Self {
            config,
            _marker: PhantomData,
        }
    }

    fn configure(
        meta: &mut ConstraintSystem<F>,
        advice: [Column<Advice>; 2],
        instance: Column<Instance>,
        constant: Column<Fixed>,
    ) -> <Self as Chip<F>>::Config {
        meta.enable_equality(instance.into());
        meta.enable_constant(constant);
        for column in &advice {
            meta.enable_equality((*column).into());
        }
        let s_mul = meta.selector();

        meta.create_gate("mul", |meta| {
            // | a0  | a1  | s_mul |
            // |-----|-----|-------|
            // | lhs | rhs | s_mul |
            // | out |     |       |
            let lhs = meta.query_advice(advice[0], Rotation::cur());
            let rhs = meta.query_advice(advice[1], Rotation::cur());
            let out = meta.query_advice(advice[0], Rotation::next());
            let s_mul = meta.query_selector(s_mul);

            vec![s_mul * (lhs * rhs - out)]
        });

        FieldConfig {
            advice,
            instance,
            s_mul,
            constant,
        }
    }
}
```

示例电路申请了两个 advice 列，一个 instance 和 fixed 列, 同时电路构造了一个乘法 Gate：
```rust
vec![s_mul * (lhs * rhs - out)]
```

该乘法 Gate 就是相应的限制表达式。注意和其他零知识证明的约束系统不一样的是，一个约束可以采用多个行上的 Cell 。整个调用关系如下：
![](https://mmbiz.qpic.cn/mmbiz_png/IXicdJl0t7b7ibicTPcMibia8flkibbicgl0ZXYMpUVphNgOz01Tc32lIpge03nubDSZxqbF187XTFWCbb7kT8vUK6Gow/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)


#### synthesize

Dream : synthesize 过程就不要看代码了, 看着一张图就够了
在 Configure 完电路后，可以调用 `synthesize` 综合某个电路实例。整个调用关系如下：

![[Pasted image 20230824174216.png]]

看图, 某个 Chip 调用 Layouter 分配 Region，并在 Region 中指定约束。可以查看 FieldChip 的 mul 函数：
```rust
impl<F: FieldExt> NumericInstructions<F> for FieldChip<F> {
fn mul(
	&self,
	mut layouter: impl Layouter<F>,
	a: Self::Num,
	b: Self::Num,
) -> Result<Self::Num, Error> {
	let config = self.config();

	let mut out = None;
	layouter.assign_region(
		|| "mul",
		|mut region: Region<'_, F>| {
		    // 获取到一个Region后，enable该row对应的乘法selector。
			config.s_mul.enable(&mut region, 0)?; 

			let lhs = region.assign_advice( //对两个advice进行赋值
				|| "lhs",
				config.advice[0],
				0,
				|| a.value.ok_or(Error::SynthesisError),
			)?;
			let rhs = region.assign_advice(
				|| "rhs",
				config.advice[1],
				0,
				|| b.value.ok_or(Error::SynthesisError),
			)?;
			//限制两个 advice 和之前的 load 的 Cell 一致
			region.constrain_equal(a.cell, lhs)?; 
			region.constrain_equal(b.cell, rhs)?;

			// Now we can assign the multiplication result into the output position.
			let value = a.value.and_then(|a| b.value.map(|b| a * b));
			let cell = region.assign_advice( //对乘法的输出进行赋值
				|| "lhs * rhs",
				config.advice[0],
				1,
				|| value.ok_or(Error::SynthesisError),
			)?;

			// Finally, we return a variable representing the output,
			// to be used in another part of the circuit.
			out = Some(Number { cell, value });
			Ok(())
		},
	)?;

	Ok(out.unwrap())
} // ...
}
```


### Summary

理解 Halo2，可以从两部分着手：1/ 电路构建 2/ 证明系统。从开发者的角度看，电路构建是接口。如何通过 Halo2 创建电路，这些电路在Halo2的内部如何表示是理解电路构建的关键。Halo2中的Chip电路由一个个 Region 组成，在 Halo2 的框架中，Region 通过 Layouter 进行分配。电路的所有的信息都存储在 Assignment 的接口中。Halo2的电路构建分为两部分：
1/Configure （电路配置）
2/ Synthesize（电路综合）。简单的说，Configure就是进行电路本身的配置。Synthesize进行某个电路实例的综合。