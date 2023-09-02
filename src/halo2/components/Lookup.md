> [!note] : We call the $Z(X)$ polynomial (the grand product argument polynomial for the permutation argument) the "permutation product" column.

设计思想：we **precompute a lookup table** of the **legitimate (input, output)** combinations; and the **prover** argues **the witness values exist in this table**.

1. 提前计算一个由有效的 **(input, output)** 组成的 **lookup table** ; 
2. Prover 证明 witness 存在这个表里 ;

> $P$  只需要证明合法的 witness 的存在一个预先计算的 **lookup table** 中，而不是通过电路计算来证明合法的 witness 存在，理论上就可以减少电路的规模

对比 PLONKup :
1. Halo 2 uses the lookup technique, which allows for lookups in arbitrary sets, and is arguably **simpler** than Plookup. (通过使用 lookup argument ，允许在任意的集合上进行 lookup 比Plookup 更简单)
2. 比 Plookup 技术更具普适性，Plookup 要求元素种类相同，数量可不相同；
3. 和 Plonk 里的 permutation argument 相比，Halo2 Lookup 的置换不需要指定索引；

> [!note]  :  仔细阅读 [[7. Lookup Gate]]

#### subset argument

> `subset argument` 的目标就是证明 : “A 中的每个元素都出现在了 S 上，即 A 是 S 的一个子集”，这意味着 S 中**必有** A 的所有元素，而 A 中**未必**要一定有 S 中的所有元素”。

- S 可能是 `fixed`，也可能是 `variable` ; 比如 : 
	- 当做 range proof 时，S 是 fixed，例如是 $S := {0,…2^k-1}$，
	- 当证明一个 circuit 的 input 是属于另外一个 circuit 的 output 时，S 是 variable 。
	- S 可能同时包括多个 columns，一些是 fixed，一些是 variable (advice columns)
- A 和 S 中可以包含**重复数据**。如果 A 或者 S 中的数据大小没有达到 $2^k$ ，我们可以填充重复的数据或者虚假的数据至 $2^k$ 大小
	- Alternatively, 我们可以增加一个“查找选择子(lookup selector)”，控制在 $A$ 列中的哪些元素参与查找。采用这种方法可以替换下面公式中的 $A(X)$ , 如果一个元素不需要查找，用 $S_0$ ​替换 $A$
- 令 ${\ell}_i$ 是 Lagrange baisi polynomial，即，在  $row \ \ i$  处值为 1 ，其他行为 0。

> - S might be `fixed`, but it doesn't need to be. That is, we can support looking up values in either fixed or variable tables (where the latter includes advice columns).
> - $A$ and $S$ can contain `duplicates`. If the sets represented by $A$ and/or $S$ are not naturally of size  we extend  with duplicates and  with dummy values known to be in $S.$
> - Alternatively we could add a "lookup selector" that controls which elements of the $A$
    column participate in lookups. This would modify the occurrence of $A(X)$ in the
    permutation rule below to replace $A$ with, say, $S_0$ if a lookup is not selected.

LFG!  让我们从 $A$ 和 $S$ 列的置换开始。假设它们的置换分别为 $A'$ 和 $S'$ 列。我们可以通过置换论据 $Z$ 约束它们之间的置换关系：
$$
\begin{align*}
&Z(\omega X) \cdot (A'(X) + \beta) \cdot (S'(X) + \gamma) - Z(X) \cdot (A(X) + \beta) \cdot (S(X) + \gamma) = 0 \\
&\ell_0(X) \cdot (1 - Z(X)) = 0
\end{align*}
$$
> i.e. provided that division by zero does not occur(除以 0 不发生的情况下), we have for all $i \in [0, 2^k)$:

$$
\begin{align*}
&Z_{i+1} = Z_i \cdot \frac{(A_i + \beta) \cdot (S_i + \gamma)}{(A'_i + \beta) \cdot (S'_i + \gamma)} \\
&Z_{2^k} = Z_0 = 1.
\end{align*}
$$
> 如上简单版本的 permutation argument 能证明 $A'$  和 $S'$  是 $A$ 和 $S$ 列的置换，但是并没有指明具体的置换关系。$\beta$ 和 $\gamma$ 是 separate challenges, 采用这 2 个独立因子，we can combine these two `permutation arguments` into one,  without worrying that they might interfere(干扰) with each other.

下面我们想让  $A'$ 和 $S'$ 列满足一些更具体的条件 : 
1. $A'$ 中相同的元素位置靠在一起。
2. $A'$ 列中挨在一起的那些相同元素, 其**第一个元素**存在于 $S'$ 列中
$$
A'_i = S'_i \quad or \quad  A'_i = A'_{i-1}
$$
i.e. 
$$
\underbrace{(A'(X) - S'(X))}_{A'_i = S'_i } \cdot \underbrace{(A'(X) - A'(\omega^{-1} X))}_{A'_i = A'_{i-1}} = 0
$$

为了防止出现 $A'$ 在 $H$ 上 `循环回卷` 导致的漏洞：要求 $A'$ 和 $S'$ 的**第一个元素**必须相同，therefore we enforce $A'_0 = S'_0$ using the rule : 
$$  
\ell_0(X) \cdot (A'(X) - S'(X)) = 0
$$
这些约束规则一起有效的限制了 $A'$ （也就是 $A$ 列）中的每个元素都存在于 $S'$ 列中（也就是 $S$ 列）

> 鉴于规则 2 ，规则 1 中的 $A'(X) - A'(\omega^{-1} X)$ 这一项在 0 行不起效果，尽管 $\omega^{-1} X$  “能反转” (even though $\omega^{-1} X$ "wraps")

#### `Zero-knowledge` adjustment

In order to achieve zero knowledge for the PLONK-based proof system, we will need the last
$t$ rows of each column to be filled with random values. (在每列的最后加入 $t$ 个随机元素)

This requires an adjustment to the `lookup argument`, because these random values would not satisfy the constraints described above. (加入的随机元素并不满足上面 👆🏻 的约束)

We limit the number of usable rows to $u = 2^k - t - 1.$ (我们限制/认为前  $u = 2^k - t - 1.$ 是有效行) , We add two selectors:
 -  $q_\mathit{blind}$  is set to $1$ on the last $t$ rows, and $0$ elsewhere; (在最后 $t$ 行设置为 1，其他行为 0 )
 - $q_\mathit{last}$  is set to $1$ only on row $u,$  and $0$ elsewhere (i.e. it is set on the row in between the usable rows and the blinding rows). ( $q_\mathit{last}$ 仅设置在 row $u$  , 即有效行和盲化行的交界处)

我们将之前的规则限制在有效行上：
$$
\begin{align*}
&\big(1 - (q_\mathit{last}(X) + q_\mathit{blind}(X))\big) \cdot \big(Z(\omega X) \cdot (A'(X) + \beta) \cdot (S'(X) + \gamma) - Z(X) \cdot (A(X) + \beta) \cdot (S(X) + \gamma)\big) = 0 \\
&\big(1 - (q_\mathit{last}(X) + q_\mathit{blind}(X))\big) \cdot (A'(X) - S'(X)) \cdot (A'(X) - A'(\omega^{-1} X)) = 0
\end{align*}
$$

在 $0$ 行的限制规则保持不变：
$$
\begin{align*}
&\ell_0(X) \cdot (A'(X) - S'(X)) = 0\\
&\ell_0(X) \cdot (1 - Z(X)) = 0
\end{align*}
$$
> [!info]
> 因为我们不能再依赖在 $\omega^{2^k},$ 点的环绕保证Z为1，相反我们要约束 $Z(\omega^u)$ 为 1 。这里有个难点：如果在任意 $i \in [0, u),$， $A_i + \beta$ or $S_i + \gamma$ 为 0的话，置换论据可能不成立。虽然这种情况在 $β$ 和 $γ$ 的取值下概率可以忽略，但是，对于完美零知识和完备性来说是个障碍（攻击者可以构造这样的情况）
> 
> To ensure both perfect `completeness` and `perfect zero knowledge`, we allow $Z(\omega^u)$ to be either zero or one:
> $$
>  q_\mathit{last}(X) \cdot (Z(X)^2 - Z(X)) = 0
> $$
> Now if $A_i + \beta$ or $S_i + \gamma$ are zero for some $i,$ we can set $Z_j = 0$ for $i < j \leq u,$ satisfying the constraint system. 
> Note that the challenges  and  are chosen after committing to  and  (and to $A'$ and $S'$), so the prover cannot force the case where some $A_i + \beta$ or $S_i + \gamma$ is zero to occu

**Cost:**
* There is the original column $A$ and the fixed column $S.$
* There is a permutation product column $Z.$
* There are the two permutations $A'$ and $S'.$
* The gates are all of low degree. (约束方程（门）的阶都不高)


#### Lookup table (最强特性)

Lookup Background : 
 - 在电路中构建 SHA-256/AES-128 是非常困难费劲的, Lookup 帮我们节省电路的规模

![[Pasted image 20230822144002.png]]

可以表达 : 列 a6 中的 Cell 的值一定包含在 $f_0$ 列中

举个例子, 如 Range Check (范围检查):
 - 假设 $f_0$  列中包含的是 $[0 \ldots 2^{16} -1 ]$  通过 Lookup 证明 $a_6 \subset f_0$  , 就可以约束 $a_6$ 列中 cell 的值都 $< 2^{16}$ , i.e. 都属于 u16 类型
 - 假设  $f_1$  列中包含的是 $[0 \ldots 2^{8} -1 ]$  通过 Lookup 证明 $i_0 \subset f_1$  , 就可以约束 $i_0$ 列中 cell 的值都 $< 2^{8}$ , i.e. 都属于 u8 类型
 - PLONK 不擅长处理 Bool 操作, 此时也可以使用 Lookup

以上都是静态的 Examples, 都是 fixed table ,但她强大的点在于 `Expression` : 
 - 约束 2 倍的 $a_2$ 包含在 $a_3$ 里 : $2 \times a_2 \subset a_3$ 
 - Lookup table 可以去申请 Selector, 可以这样: 
	 - 第一行满足 $a_1 + a_2 \subset a_3$ 
	 - 第二行不用满足 👆🏻 上面约束
	 - 第三行继续满足 $a_1 + a_2 \subset a_3$ ...

