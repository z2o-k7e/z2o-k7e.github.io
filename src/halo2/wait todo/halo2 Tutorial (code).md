https://erroldrummond.gitbook.io/halo2-tutorial/

### Column type

|Column type|Purpose|
|---|---|
|Advice|Witness values, i.e. wire values that are dependent on the circuit's input|
|Instance|Public input values|
|Fixed|Circuit hardcoded constants, such as selector values or constants|
|Selector|Okay this isn't actually a column type. This is essentially a 'spiced up' fixed column|

```rust
#[allow(non_snake_case, dead_code)]
#[derive(Debug, Clone)]
struct TutorialConfig{
    l:  Column<Advice>,
    r:  Column<Advice>,
    o:  Column<Advice>,

    sl: Column<Fixed>,
    sr: Column<Fixed>,
    so: Column<Fixed>,
    sm: Column<Fixed>,
    sc: Column<Fixed>,

    PI: Column<Instance>,        
}
```

Note: it would be good to point out here that these columns will all be the same length, and since we can put them side by side we can think of them as columns of some table.

Next we want to wrap this struct in another struct with `marker: PhantomData`
```rust
struct TutorialChip<F: FieldExt> {
    config: TutorialConfig,
    marker: PhantomData<F>,
}
```

We will need to define a trait (let's refer to it as the composer trait), within which we will have the functions for the types of gates we want (and perhaps some other functionality like `expose_public` or `copy` depending on how we design things). 

When creating this trait we should be doing so with the knowledge of the config and the equation we are trying to emulate.
> 我们需要定义一个 trait（让我们将其称为作曲家 trait），在其中我们将拥有我们想要的门类型的函数（也许还有一些其他功能，如 `expose_public` 或 `copy`，具体取决于我们的设计方式）。创建此 trait 时，我们应该了解配置和我们试图模拟的方程

```rust
trait TutorialComposer<F: FieldExt> {
    fn raw_multiply<FM> (
        &self,
        layouter: &mut impl Layouter<F>,
        f: FM,
    ) -> Result<(Cell, Cell, Cell), Error>
    where 
        FM: FnMut() -> Value<(Assigned<F>, Assigned<F>, Assigned<F>)>;
    
    fn raw_add<FM> (
        &self,
        layouter: &mut impl Layouter<F>,
        f: FM,
    ) -> Result<(Cell, Cell, Cell), Error>
    where 
        FM: FnMut() -> Value<(Assigned<F>, Assigned<F>, Assigned<F>)>;
    
    // Ensure 2 wire values are the same, in effect connecting the waires to each other.
    fn copy(&self, layouter: &mut impl Layouter<F>, a: Cell, b: Cell) -> Result<(), Error>;

    // Exposes a number as a Public input to the Circuit:
    fn expose_public(
        &self,
        layouter: &mut impl Layouter<F>,
        cell: Cell,
        row: usize,
    ) -> Result<(), Error>;
}
```



code reference: 
 - https://github.com/EDGDrummond/EF-grant/tree/main/halo2