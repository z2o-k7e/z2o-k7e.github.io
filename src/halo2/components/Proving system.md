Halo2 的证明系统可以分解成五个部分：

1 . 对电路中的主要模块多项式承诺：
 - Cell assignment (单元格的赋值)
 - Permuted values and products for each lookup argument. (查找表证明中的置换和乘积)
 - Equality constraint permutations.
2 . Construct the vanishing argument to constrain all circuit relations to zero:
 - Standard and custom gates.
 - Lookup argument rules.
 - Equality constraint permutation rules.
3 . Evaluate the above polynomials at all necessary points :
 - All relative rotations used by custom gates across all columns. (相对偏移)
 - Vanishing argument pieces. 
4 . 构建多点打开证明，检查所有的多项式取值和相应的承诺是否一致
5 . 采用内积证明(inner product argument) 为多点打开证明生成证明

为了辅助我们的解释，我们时不时地用如下的约束系统：
 - 4 个 advice 列 : $a,b,c,d$
 - 1 个 fixed 列 $f$
 - 3 个 custom gates : 
	- $a \cdot b \cdot c_{-1} -d =0$
	- $f_{-1} \cdot c =0$
	- $f  \cdot  d  \cdot  a=0$

### TL;DR

The table below provides a (probably too) **succinct description** of the Halo 2 protocol. 

This description will likely be replaced by the Halo 2 paper and security proof, but for now serves as a summary of the following sub-sections.

| Prover                                                                      |         | Verifier                           |
| --------------------------------------------------------------------------- | ------- | ---------------------------------- |
|                                                                             | $\leftarrow$ | $t(X) = (X^n - 1)$                 |
|                                                                             | $\leftarrow$ | $F = [F_0, F_1, \dots, F_{m - 1}]$ |
| $\mathbf{A} = [A_0, A_1, \dots, A_{m - 1}]$                                 | $\rightarrow$ |                                    |
|                                                                             | $\leftarrow$ | $\theta$                           |
| $\mathbf{L} = [(A'_0, S'_0), \dots, (A'_{m - 1}, S'_{m - 1})]$              | $\rightarrow$ |                                    |
|                                                                             | $\leftarrow$ | $\beta, \gamma$                    |
| $\mathbf{Z_P} = [Z_{P,0}, Z_{P,1}, \ldots]$                                 | $\rightarrow$ |                                    |
| $\mathbf{Z_L} = [Z_{L,0}, Z_{L,1}, \ldots]$                                 | $\rightarrow$ |                                    |
|                                                                             | $\leftarrow$ | $y$                                |
| $h(X) = \frac{\text{gate}_0(X) + \dots + y^i \cdot \text{gate}_i(X)}{t(X)}$ |         |                                    |
| $h(X) = h_0(X) + \dots + X^{n(d-1)} h_{d-1}(X)$                             |         |                                    |
| $\mathbf{H} = [H_0, H_1, \dots, H_{d-1}]$                                   | $\rightarrow$ |                                    |
|                                                                             | $\leftarrow$ | $x$                                |
| $evals = [A_0(x), \dots, H_{d - 1}(x)]$                                     | $\rightarrow$ |                                    |
|                                                                             |         | Checks $h(x)$                      |
|                                                                             | $\leftarrow$ | $x_1, x_2$                         |
| Constructs $h'(X)$ multipoint opening poly                                  |         |                                    |
| $U = \text{Commit}(h'(X))$                                                  | $\rightarrow$ |                                    |
|                                                                             | $\leftarrow$ | $x_3$                              |
| $\mathbf{q}_\text{evals} = [Q_0(x_3), Q_1(x_3), \dots]$                     | $\rightarrow$ |                                    |
| $u_\text{eval} = U(x_3)$                                                    | $\rightarrow$ |                                    |
|                                                                             | $\leftarrow$ | $x_4$                              |

Then the prover and verifier:
- Construct   $\text{finalPoly}(X)$  as a linear combination of $\mathbf{Q}$ and $U$ using powers of $x_4$;
- Construct $\text{finalPolyEval}$ as the equivalent linear combination of $\mathbf{q}_\text{evals}$ and  $u_\text{eval}$; and
- Perform $\text{InnerProduct}(\text{finalPoly}(X), x_3, \text{finalPolyEval}).$

> TODO: Write up protocol components that provide zero-knowledge.



### Lookup

![[Lookup#subset argument]]


![[Lookup#`Zero-knowledge` adjustment]]