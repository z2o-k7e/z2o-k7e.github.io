zk : 重在证明，不在计算

halo2(ultra plonk) :
IPjA https://github.com/zcash/halo2
kzg https://github.com/privacy-scaling-explorations/halo2
DSL https://github.com/privacy-scaling-explorations/chiquito


zkevm
common gadget
https://github.com/scroll-tech/zkevm-circuits/tree/develop/gadgets

circuit pattern 

zkEVM 电路: 大大小小 10 几个电路

```rust
impl<F: Field> IsZeroConfig<F> {
    /// Returns the is_zero expression
    pub fn expr(&self) -> Expression<F> {
        self.is_zero_expression.clone()
    }

    /// Returns the is_zero expression from inputs, at any rotation where q_enable=1.
    pub fn expr_at(
        &self,
        meta: &mut VirtualCells<'_, F>,
        at: Rotation,
        value: Expression<F>,
    ) -> Expression<F> {
        1.expr() - value * meta.query_advice(self.value_inv, at)
    }

    /// Annotates columns of this gadget embedded within a circuit region.
    pub fn annotate_columns_in_region(&self, region: &mut Region<F>, prefix: &str) {
        [(self.value_inv, "GADGETS_IS_ZERO_inverse_witness")]
            .iter()
            .for_each(|(col, ann)| region.name_column(|| format!("{prefix}_{ann}"), *col));
    }
}
```