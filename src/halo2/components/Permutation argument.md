## Constructing the permutation

Given that gates in halo2 circuits operate "locally" (on cells in the current row or defined relative rows), it is common to need to copy a value from some arbitrary cell into the current row for use in a gate. This is performed with an equality constraint, which enforces that the source and destination cells contain the same value.
鉴于 halo2 的电路门在 “本地”运行（on cells in the current row or defined `relative` rows），通常需要将某个任意 cell 中的值**复制**到当前行以在门中使用。这是通过等式约束来执行的，该约束强制源单元格和目标单元格包含相同的值。

We implement these equality constraints by constructing a permutation that represents the constraints, and then using a `permutation argument` within the proof to enforce them.
我们通过构造表示约束的排列来实现这些等式约束，然后在证明中使用 `permutation argument`  来强制执行它们

> [!note] 通常使用 [轮换记号](https://en.wikipedia.org/wiki/Permutation#Cycle_notation) 来表示置换 :
> 1. 令 $(a \ b \ c)$ 代表如下轮换：$a$ 映射到 $b$， $b$ 映射到 $c$ ， $c$ 再映射到 $a$ （显然，该法则可以推广到任意大小的轮换）
> 2. 把两个或更多的轮换一个接一个的写在一起就代表了一个相关置换的**组合** :  比如，$(ab) \ (cd)$ 就代表了 $a$ 映射到 $b$ ， $b$ 映射到 $a$ ; $c$ 映射到 $d$ ， $d$ 映射到 $c$ 的置换

**Goal**: 我们希望构造这样一个置换，在同一个相等约束下的这些变量自己形成一个轮换。举例来说，假设我们有一个定义了如下相等约束的电路：
- $a \equiv b$
- $a \equiv c  \quad \Rightarrow \quad b \equiv c$ 
- $d \equiv e$

由上可得相等约束集合 $\{a, b, c\}$ 和 $\{d, e\}.$  于是我们构造了如下的置换：$(a\ b\ c)\ (d\ e)$ , 
该置换就定义了一个由 $[a,b,c,d,e]$ 到 $[b,c,a,e,d]$ 的一个 mapping(映射)

**Algorithm** : 

我们需要持续记录这些轮换的集合，实际上也就是 [一些不相交集合的集合(Disjoint-set data structure)](https://en.wikipedia.org/wiki/Disjoint-set_data_structure). 为了简单起见，我们选择了一个虽然不是渐进最优，但是确是很容易实现的方法 

我们采取如下方法来表示当前的状态：
 - 数组 $\mathsf{mapping}$ 来表示置换本身；
 - 一个辅助的数组 $\mathsf{aux}$ 用来记录每个轮换(cycle) 中代表元素；
 - 还有一个数组 $\mathsf{sizes}$ 用来记录每个轮换(cycle) 的元素个数。

我们在一个给定的轮换 $C$ 中选取一个不变的值 $c$ ，对每一个 $C$ 中的元素 $x$ ，$\mathsf{aux}(x)$ 都得到相同的值，即 $c \in C.$  有了这个值，对于 2 个给定的元素 $x$ 和 $y$ ，我们可以通过检查 $\mathsf{aux}(x) = \mathsf{aux}(y)$ 是否成立，来快速判断它们是否在同一个轮换(cycle) 中

$\mathsf{sizes}(\mathsf{aux}(x))$  也代表了包含 $x$ 的轮换的大小 (只有 $\mathsf{sizes}(\mathsf{aux}(x))$  是有保证的，而不是 $\mathsf{sizes}(x).$  )

本算法是以表示一个单位置换开始： 对所有 $x$ ，我们令 $\mathsf{mapping}(x) = x,$ $\mathsf{aux}(x) = x$ , 和 $\mathsf{sizes}(x) = 1$ 

按如下步骤增加一条相等约束  $\mathit{left} \equiv \mathit{right}$  :
1. 检查 $\mathit{left}$  和 $\mathit{right}$  是否已经在同一个轮换中了，也就是检查 $\mathsf{aux}(\mathit{left}) = \mathsf{aux}(\mathit{right})$ 是否成立, 若成立，那么不做任何事情 
2. 否则,  $\mathit{left}$  和 $\mathit{right}$  就一定分属不同的轮换。令 $\mathit{left}$ 是相对大的轮换，而 $\mathit{right}$  则为相对小的那个， 如果 $\mathsf{sizes}(\mathsf{aux}(\mathit{left})) < \mathsf{sizes}(\mathsf{aux}(\mathit{right}))$ ，那我们就 Swap 它俩, 使其满足需求
3. 令 $\mathsf{sizes}(\mathsf{aux}(\mathit{left})) := \mathsf{sizes}(\mathsf{aux}(\mathit{left})) + \mathsf{sizes}(\mathsf{aux}(\mathit{right}))$
4. 下一步对 $\mathit{right}$  (较小的) 轮换中每一个元素 $x$ 令 $\mathsf{aux}(x) := \mathsf{aux}(\mathit{left})$ 
5. 通过交换 $\mathsf{mapping}(\mathit{left})$ 和 $\mathsf{mapping}(\mathit{right})$  中的元素（的轮换），将较小的轮换接入到较大的轮换中去

For example, given two disjoint cycles(不相交的 2 个 轮换)  $(A\ B\ C\ D)$ and $(E\ F\ G\ H)$:

```plaintext
A +---> B
^       +
|       |
+       v
D <---+ C       E +---> F
                ^       +
                |       |
                +       v
                H <---+ G
```

After adding constraint $B \equiv E$ the above algorithm produces the cycle:

```plaintext
A +---> B +-------------+
^                       |
|                       |
+                       v
D <---+ C <---+ E       F
                ^       +
                |       |
                +       v
                H <---+ G
```

### [Broken alternatives](https://trapdoor-tech.github.io/halo2-book-chinese/design/proving-system/permutation.html#broken-alternatives)

如果不检查 $left$ 和 $right$ 是否在同一个轮换中，那么我们可能就会丢掉某些相等约束。 举个例子，如果我们有如下的约束:
- $a \equiv b$
- $b \equiv c$
- $c \equiv d$
- $b \equiv d$

如果在处理一条新的相等约束时仅仅只执行上述算法中的第五步，那么我们得到的最终结果将是 $(a\ b)\ (c\ d),$  rather than the correct $(a\ b\ c\ d).$

