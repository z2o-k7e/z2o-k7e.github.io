## Recursive zk-snakes & Accumulation Scheme

Halo2采用Pasta curves: Pallas 和 Vesta实现recursive zk-snarks证明系统，Mina也采用了这个cycle curves，关于recursive zk-snarks的设计原理，可以参考[ZKSwap团队浅析L2扩容关键技术：递归零知识证明剖析](https://link.zhihu.com/?target=https%3A//zks.org/zh/blog/64)。

由于Halo2采用了Inner Product Argument论证，因此Verify的复杂度是O(n)的，这破坏了Snaks的简洁性。因此，Halo2提出一种新的技术Nested Amortization，是一种[Accumulation Scheme](https://link.zhihu.com/?target=https%3A//zcash.github.io/halo2/background/recursion.html%23Polynomial-commitment-using-inner-product-argument)聚合技术。具体如下：

1. 在verification circuit中，不实现G的运算，这个运算是O(n)的，而是由Prover提供G以及相关计算参数，作为circuit的witness，即对于verification circuit来讲，已经默认了G的正确性，而实际上G的正确性仍然需要verifier去做，只不过这一步可以聚合，即采用Accumulation Scheme方案；

2. 第1点提到，关于G的正确性验证，仍然需要Verifier去做，但是我们可以Accumulate多个proof instance的G，以及相关参数，迭代到最后一步，由verifier一次完成验证，这实际上完成了验证成本的摊销，即Nested Amortization的核心。