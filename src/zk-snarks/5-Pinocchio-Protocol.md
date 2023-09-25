> - 作者：Maksym Petkus
> - 翻译 & 注解：even@安比实验室（even@secbit.io）
> - 校对：valuka@安比实验室
> - [本系列文章](https://arxiv.org/pdf/1906.07221.pdf)已获作者中文翻译授权
> - [翻译原链接](https://secbit.io/blog/2019/12/25/learn-zk-snark-from-zero-part-one/)

[TOC]

### 约束和公共输入 Constraints and Public Inputs
#### 约束 Constraints

我们的分析主要集中在运算的概念上。但是，协议实际上不是去做”计算“，而是检验输出值是否是操作数正确运算得到的结果。所以我们称之为约束，即一个 $V$ 约束 $P$ 去为预定义的“程序”提供有效值，而无论这个“程序”是什么。多个约束组成的系统被称为“约束系统”（在我们的例子中这是一个一阶约束系统，或被称为 **R1CS**）

> @Maksym（作者）：这里其实隐含了寻找所有正确答案的一个方法就是对所有可能的组合值进行一次暴力破解，然后只选择一个满足的约束，或者使用可满足约束的更精密的技术 [con18](https://secbit.io/blog/2020/01/22/learn-zk-snark-from-zero-part-five/#b979)
> even@安比实验室：请注意这个约束是定义在算术电路，或者布尔电路上。因为这两类电路的可满足性问题是 NP-Complete 问题。

因而我们也可以使用约束来确保其它的关系。例如，如果我们想要确认变量 $a$ 的值只能为 0 或 1（即二进制数），我们可以用一个简单的约束去做这件事：
$$
\textcolor{green}{a} \times \textcolor{blue}{a}  =\textcolor{red}{c} 
$$
我们也可以约束 $a$ 的值只能为 2：
$$
\textcolor{green}{(a -2)} \times \textcolor{blue}{1}  =\textcolor{red}{0}
$$
一个更复杂的例子是确保数字  $a$  是一个 4-bit 的数字（也称为半字节 nibble），换句话说可以用 4 个bit 来表示出  $a$  , 我们也可以称这个为“确保取值范围” , 因为一个 4-bit 的数字可以代表 $2^4$ 的组合，因而也就是从 `0 ~ 15` 范围内的 16 个数字。如 $1011= 1 \cdot 2^3 + 0 \cdot 2^2 + 1 \cdot 2^1 + 1 \cdot 2^0 = 11$ 

Therefore if  $a$  is a 4-bit number, then $a= b_3⋅2^3 + b_2 ⋅ 2^2 + b_1⋅ 2^1 + b_0 ⋅ 2^0$ ，for some boolean  $b_3, b_2, b_1, b_0$  ,  The constraint can be following:
$$
\textcolor{green}{a} \quad \times  \quad \textcolor{blue}{1} \quad  = \quad  \textcolor{red}{8 b_3 +4 b_2 + 2 b_1 + 1 b_0} \tag{1}
$$
并且为了确保 $b_3, b_2, b_1, b_0$  都是二进制数我们需要增加约束：
$$
\begin{align}
&\textcolor{green}{b_0} \quad \times \quad \textcolor{blue}{b_0} \quad = \quad \textcolor{red}{b_0}  \tag{2} \\ 
&... \\
&\textcolor{green}{b_2} \quad \times \quad  \textcolor{blue}{b_2} \quad  = \quad  \textcolor{red}{b_2}  \tag{3}  \\
&\textcolor{green}{b_3} \quad  \times \quad  \textcolor{blue}{b_3} \quad  = \quad  \textcolor{red}{b_3}   \tag{4} \\
\end{align}
$$
可以写成 Circom 代码:
```rust
a * 1 = 8*b3 + 4*b2 + 2*b1 + 1*b0
b0 * b0 = b0
b1 * b1 = b1
b2 * b2 = b2
b3 * b3 = b3
```

更复杂的约束也可以用这种方式表示，以此来确保使用的值满足规则。需要注意的是，上述约束 $(1)$ 在当前操作的构造中是不可能的：
$$
\textcolor{green}{\sum_{i=1}^n{c_{l, \ i} \cdot v_i }} \times \textcolor{blue}{\sum_{i=1}^n{c_{r, \ i} \cdot v_i}} = \textcolor{red}{\sum_{i=1}^n{c_{o, \ i} \cdot v_i }}
$$
因为值 1 （以及前面约束中的 2）必须通过 $c \cdot v_{one}$  表达出来，其中 _c_ 可以被固定到 _proving key_ 中，但是因为 _v_ 是由 $P$ 提供的，所以可以是任何别的值。尽管我们可以通过设置 _c = 0_ 来强制 $c \cdot v_{one}$  变成 0，但是在我们前面受限的构造方法中很难找到一个约束来强制 $v_{one}$  为 1。于是，$V$ 需要有一种办法来设置 $v_{one}$ 的值

> 首先，我们需要明确 $v_{one}$ 这个特殊变量的角色。在 zk-SNARK的 约束系统中，它是被预设为 1 的。换句话说，无论其他变量如何变化， $v_{one}$ 的值始终应该是1。
> 
> 然后，该部分内容提到的 $c \cdot v_{one}$ ，实际上是约束系统中的一个公式，其中 $c$ 是证明者在证明过程中需要选择的一个值， $v_{one}$ 是我们前面说的那个始终为 1 的特殊变量。
>
> 然后，它提到，即使我们可以通过设置 $c = 0$ 来使得 $c \cdot v_{one}$ 为0，但在当前的构造方法中，我们却无法找到一个约束来强制  $v_{one}$  为 1 。这个意思是说，在我们的构造方法中，我们可以通过选择不同的 $c$ 使得 $c \cdot v_{one}$ 为任何我们希望的值，但这并不能保证  $v_{one}$  始终为1

> even@安比实验室: 我们前文中提到的表达式的约束关系就称为 R1CS

#### Public Inputs and One (公共输入和 1)

如果不能根据 $V$ 的输入对其进行检查，例如，知道证明者已将两个值相乘而不知道结果和/或值是什么，那么证明的可用性将受到限制。虽然可以在 _proving key_ 中通过“硬编码(hardwire)”来进行验证一些特定的值（如，约束某步乘法运算的结果必须为 12 ），但这就需要针对每一个所需的的 “verifier 输入”生成单独的密钥对 (this would require to generate separate pair of keys for each desired “`verifier’s input`.”)

> even@安比实验室: 这样会严重限制实用性，电路需要支持参数。

**因而如果可以由 $V$  为计算指定一些值(输入/输出)，包括 $\color{red}v_{one}$  ，而不是由 $P$  来控制,  那证明就可以变得更通用!!** (Therefore it would be universal if the verifier could specify some of the values (inputs or/and outputs) for the computation, including the $v_{one}$ , instead of the $\color{red}P$.)

首先，我们看一下要证明的值
$$\textcolor{green}{g^{L(s)}}, \ \textcolor{blue}{g^{R(s)}}, \ \textcolor{red}{g^{O(s)}}$$
Because we are using the `homomorphic encryption` it is possible to augment these values, for example, we can add another encrypted polynomial evaluation (利用同态加密，我们可以扩大这些值)
$$g^{L(s)} \cdot g^{l_v(s)} =g^{L(s) + l_v(s)}$$
which means that the verifier could add other variable polynomials to the already provided ones. Therefore if we could exclude necessary variable polynomials from the ones available to the prover, the verifier would be able to set his values on those variables, while the computation check should still match.
这意味着验证者 $V$  可以将 other *variable polynomials* 添加到已经提供的 polys 中。因此，如果我们可以从提供给 $Prover$ 的变量多项式中, 排除(exclude) necessary variable polys, 验证者 $V$ 将能够在这些变量上设置他的值，而计算检查应该仍然匹配。

也就是说, 这样如果我们能够在提供给 $P$ 的变量多项式中排除必要的一项，$V$ 就可以在这一项变量多项式上设置他自己的值，并且使得检查依然能够通过

It is easy to achieve since the $V$ is already constraining the prover in the choice of polynomials he can use empolying the _α_-shift. Therefore those variable polynomials can be moved from the proving key to the verification key while eliminating its _α_-s and _β_ checksum counterparts.
这很容易实现，因为 $V$  已经限制了 $P$ 选择他可以使用 α-shift 的多项式。因此，这些可变多项式可以从 *proving key* 转移到 *verification key* ，同时消除其 α-s 和 β 校验和对应项。

也就是说, 因为 $V$ 早已能通过加入 α-shift 来限制 $P$ 选择多项式，所以这个应该很容易实现。因而当消除了它的 $α-s$ 和  $\beta$  校验和对应的项，这些可变多项式就可以从 _proving key_ 转移到 _verification key_ 当中去了

必要的协议更新为：

**Setup**
(需自行对比 former protocol version)
 - …将 $n$ 个 variable polys 全部分为两组：
     - $V$ 的 $m+1$ 项： $\textcolor{purple}{L_v(x)} = l_0(x) + l_1(x) +…+l_m(x)$ , 对 $R_v(x)$  和  $O_v(x)$  也做同样的计算。这里对于索引 0 保留值  $v_{one}=1$  (where idx-0 is reserved for the value of $v_{one}=1$ )
     - $P$ 的 $n-m$ 项：$\textcolor{brown}{L_p(x)} = l_{m+1}(x) + \ldots +l_n(x)$ 
     - ... 对  $\textcolor{purple}{R_p(x)}$  和  $\textcolor{brown}{O_p(x)}$  也做同样的计算
- 设置 _proving key_： 
$$
\left(\{g^{s^k}\}_{k \in [d]}, \ \ \Big\{g_l^{l_i(s)},g_r^{r_i(s)},g_o^{o_i(s)},g_l^{α_ll_i(s)},g_r^{α_rr_i(s)},g_o^{α_oo_i(s)},g_l^{βl_i(s)},g_r^{βr_i(s)},g_o^{βo_i(s)}\Big\}_{i \in \{m+1,…,n\}}\right)
$$
- 添加到 _verification key_： 
$$(……，\{g_l^{l_i(s)}, g_r^{r_i(s)}, g_o^{o_i(s)}\}_{i \in \{0,…,m\}})$$

**Proving** :
 - …为 $V$ 的多项式计算  $h(x) = \frac{L(x) \cdot R(x) -O(x)}{t(x)}$ , 其中 $L(x) = \textcolor{purple}{L_v(x)} + \textcolor{brown}{L_p(x)}$  ,  and **similarly** for  $R(x), \ O(x)$ 
 - Provide the Proof ：
$$
\left(g_l^{L_p(s)},g_r^{R_p(s)},g_o^{O_p(s)},g_l^{α_lL_p(s)},g_r^{α_rR_p(s)},g_o^{α_oO_p(s)},g^{Z(s)},g^{h(s)}\right)
$$
**Verification** : 
 - 为 $V$ 的变量多项式赋值，并加 $1$  (and add to 1) :
$$
{g_l}^{L_v(s)} = {g_l}^{l_0(s)} \cdot \prod_{i=1}^m{ \left({g_l}^{l_i(s)}\right)^{v_i}}
$$
    对 $g_r^{R_v(s)}$  和   $g_o^{O_v(s)}$  做同样的计算
    关于为啥要 +1 : 需要一种机制，让 $V$ 能够控制一些变量的值，而不是由 $P$ 控制, 通过同态加密, 让`v_one` 就是一个始终为 1 的特殊变量 (先感性理解下吧 ...)

 - 变量多项式约束检查：
$$
e(g_l^{L_p},g^{α_l}) = e(g_l^{L'_p},g)
$$
对  $g_r^{R_p}$    和  $g_o^{O_v(s)}$  做同样的计算

变量值一致性检查：
$$
\begin{array}{lllll}
e\left({g_l}^{L_p} {g_r}^{R_p}{g_o}^{O_p}, \ \ \ g^{βγ}\right) = e\left(g^Z, \ g^\gamma \right) \\

e\left({g_l}^{L_p}, \ g^{βγ} \right) \cdot e\left({g_r}^{R_p}, \ g^{βγ} \right) \cdot e\left({g_o}^{O_p}, \ g^{βγ} \right) = e\left(g^Z, \ g^\gamma \right) \leftarrow   \ 双线性性 \\
e(g_l, g)^{L_p \times βγ} \cdot e(g_r, g)^{R_p \times βγ} \cdot e(g_o, g)^{O_p \times βγ} = e(g^Z, g^\gamma)  \\ 
\textcolor{pink}{e(g,g)^{βγ \cdot(L_p + R_p +O_p)} = e(g^Z, g^\gamma)   \leftarrow \ 考虑如何规约到这种形式 } \\
\because \ \ \textcolor{red}{g_l=g^{\rho_l}, \ g_r=g^{\rho_r}, \ g_o=g^{\rho_o}} \\
e(g^{\rho_l}, g)^{L_p \times βγ} \cdot e(g^{\rho_r}, g)^{R_p \times βγ} \cdot e(g^{\rho_o}, g)^{O_p \times βγ}  \\
=e(g, g)^{\rho_l \cdot L_p \cdot βγ} \cdot e(g, g)^{\rho_r \cdot  R_p \cdot  βγ} \cdot e(g, g)^{\rho_o \cdot  O_p \cdot  βγ}  \\
= e(g, g)^{βγ\cdot (\rho_l \cdot L_p +\rho_r \cdot  R_p + \rho_o \cdot  O_p )} \quad  = e\left(g^Z, \ g^\gamma \right) \leftarrow 可能是这样? 不知道对不对 \\

\because \textcolor{red}{ g^{Z(s)} = \prod_{i=1}^n{\left(g_l^{\beta \ l_i(s)} \cdot g_r^{\beta \ r_i(s)} \cdot g_o^{\beta \ o_i(s)}\right)^{v_i}}} \leftarrow  \ 可能是这样? 不知道对不对 \\
... 协议的变量值一致性得到检查
\end{array}  
$$

有效计算检查：
$$e\left(g_l^{L_v(s)}g_l^{L_p}, \ \ g_r^{R_v(s)}g_r^{R_p}\right) =e\left(g_o^t,g^h\right) \ e \left(g_o^{O_v(s)}g_o^{O_p},g\right)$$

> 注意：根据协议（**单个变量操作数多项式** 的章节）的性质，由多项式 $l_0(x), \ \ r_0(x), \ \ o_0(x)$  表示的值 $1$ 已在相应的运算中具备了合适的值，因此不需要再赋值了
> 
> 注意：$V$ 将不得不在验证步骤中做额外的工作，使得赋值的变量成比例。

**这实际上是把一些变量从 $P$ 手中拿到 $V$ 的手中，并同时保持等式相等。** 因而只有当 $P$ 和 $V$ 的输入中使用相同值的时候， _有效计算检查_ 才依然成立。

1 这个值相当重要，它能够通过与任意一个常数项相乘来生成这个值（从选择的有限域上），例如，用 123 去乘以 _a_：
$$
\textcolor{green}{1 \cdot a} \times \textcolor{blue}{123 \cdot v_{one}} = \textcolor{red}{1 \cdot r}
$$
> even@安比实验室:  这里将原本由 $P$ 赋值的一些变量改为由拿到 $V$ 赋值，使得 $P$ 不得不与 $V$ 保持相同的输入。这不仅解决了 $V$ 参数输入的问题，也间接解决了常数赋值的问题


### Zero-Knowledge Computation 

#### Zero-Knowledge Proof of Computation

(计算的零知识证明)  自从引入通用计算协议（**计算的证明**这一章节），我们一直放弃了 _零知识_ 的性质，这是为了让协议的改进变得更简单。至此，我们已经构建了可验证的计算协议。

以前我们使用随机数 δ-转换来构造多项式的“零知识” 证明，这种方法能够使得证明与随机数无法区分（**零知识**这一章节）：
$$\delta p(s ) = t(s) \cdot \delta h(s)$$
通过计算我们证明了：
$$L(s) \cdot R(s) - O(s)= p(s)h(s)$$
尽管我们可以通过用相同 δ × 多项式的方法来调整解决方案，即提供随机值 $\delta  L(s), \ \delta  R(s), \ \delta^2  O(s), \ \delta^2  h(s)$ ，这依然能够通过 _有效计算检查_ 来满足配对验证：
$$e(g,g)^{\delta^2L(s)R(s)}=e(g,g)^{\delta^2(t(s)h(s)+O(s))}$$
但是问题是使用相同的  $\delta$  会妨碍**安全性**，因为我们在证明中分别用了以下这些值：
 - 其他人可以很容易得辨认出两个不同的多项式值是否相同，以此来获取一些知识，即：$g^{\delta L(s)}=g^{\delta R(s)}$
 - $L(s)$ 和 $R(s)$ 的不同值之间潜在的微小关系可能会通过暴力破解来区分开来，例如如果 $L(s) = 5R(s)$ ，就可以对 $i \in \{1,\cdots,n\}$  取值反复校验  $g^{L(s)}=(g^{R(s)})^i$，只需要执行 5 步就可以揭示出两者 5 倍区别的关系。同样的暴力破解也可以用在破解加密值的加法运算上，如： $g^{L(s)}=g^{R(s)+5}$
 - 证明元素之间的其它关系也可能会被发现，例如，如果 $e(g^{\delta L(s)},\ g^{\delta R(s)}) = e(g^{\delta^2 O(s)}, \ g)$ ，那么也就表示 $L(x) ⋅R(x) =O(x)$  

> 注意：**一致性检查优化** 使得挖掘数据关系变得更加困难了，但是依然能够发现一些关系 ，且不说 $V$ 可以选择特定 $\rho_l, \ \rho_r$ 来为揭示知识提供便利（只要这不是一个多样化的 $Setup$ ）

最终，我们需要对每一个多项式的值使用不同的随机数  $\delta_s$ ，例如：
$$
\delta_l L(s) \cdot \delta_r R(s)-\delta_o O(s)=t(s) \cdot(\Delta \ \text { (?) } \  h(s))
$$
为了解决等式右边不相等的问题，我们不必改变协议，只要修改证明的值  $h(s)$  即可。 这里 Delta ( $\Delta$ ) 代表为了平衡方程另一侧的随机性而对 $h(s)$ 做的处理，?⃝ 代表 `乘法运算`或者 `加法运算`（这个反过来也适应了除法和减法）。
- 如果我们选择用乘法 (?⃝ = ×) 来计算 $\Delta$ ，也就意味着不太可能有较大的概率可以找到一个 $\Delta$ ，因为存在随机性：
$$\Delta = \frac{\delta_lL(s) \cdot \delta_rR(s) - \delta_oO(s)}{t(s)h(s)}$$
设置 $\delta_o = \delta_l \cdot \delta_r$  ，于是就变成了：
$$\Delta = \frac{\delta_l \delta_r(L(s) \cdot R(s) - O(s))}{t(s)h(s)} = \delta_l \delta_r$$
但是如前文所述，这个妨碍了零知识的性质，更重要的是这个结构也不再适合 verifier 的输入多项式，因为它们必须是 $\delta s$ 相应的倍数，这就需要额外的交互了

我们可以尝试把随机数加到变量上：
$$(L(s)+ \delta_l) \cdot (R(s)+ \delta_r) - (O(s)+ \delta_o) = t(s) \cdot (\Delta \times h(s))$$
$$
\Delta = \frac{\overbrace{L(s)R(s)-O(s)}^{t(s)h(x)} + \delta_rL(s) + \delta_lR(s) + \delta_l \delta_r - \delta_o}{t(s)h(s)} = 1+ \frac{\delta_rL(s) + \delta_lR(s) + \delta_l \delta_r -\delta_o}{t(s)h(s)}
$$

但是随机数是不可除尽的。尽管我们可以用 $t(s)h(s)$ 去乘以每一个 $\delta$  ，但由于我们已经用了 $\Delta$  乘以 $h(s)$ ，  $\Delta$  是组成加密结果的一部分（即 $E(L(s))$ 相等），因此在没有使用配对（它的结果在另一个数值空间内）的情况下是不能计算出 $g^{\Delta h(s)}$ 的。同样也不能使用 $s$ 的幂（from $1$  to $d$ ）的加密值对 $\Delta h(s)$  进行加密计算， $\Delta h(s)$  的阶将达到 $2^d$  并且，基于上述同样的原因也无法计算这个随机操作数多项式的值：
$$g^{L(s) + \delta_l \ t(s) \ h(s)}$$
于是我们应该用加法(?⃝ = +)来使用 $\Delta$ ，因为它可以同态地计算。
$$
\begin{aligned}
&(L(s)+ \delta_l) \cdot (R(s) +\delta_r) - (O(s) + \delta_o) = t(s) \cdot (\Delta + h(s)) \\
&\Delta = \frac{L(s)R(s)-O(s)+\delta_rL(s) + \delta_lR(s) + \delta_l \delta_r - \delta_o - t(s)h(s)}{t(s)} => \\
&\Delta = \frac{\delta_rL(s)+ \delta_lR(s) + \delta_l \delta_r -\delta_o}{t(s)}
\end{aligned}
$$

分子中的每一项都是  $\delta$  的倍数，因而我们可以将其与 $t(s)$  相乘使它可以被分母整除：
$$
\begin{aligned}
&(L(s)+ \delta_l t(s)) \cdot (R(s) + \delta_r t(s)) -(O(s) + \delta_o t(s)) = t(s) \cdot (\Delta + h(s)) \\
&\cancel{L(s)R(s)-O(s)} +t(s)(\delta_rL(s) + \delta_lR(s) + \delta_l \delta_rt(s) - \delta_o) = t(s)\Delta + \cancel{t(s)h(s)} \\
&\Delta = \delta_rL(s) + \delta_lR(s) + \delta_l \delta_r t(s) -\delta_o
\end{aligned}
$$

这样就可以在加密的空间中进行“有效计算检查”了：
$$
\begin{aligned}
&g^{L(s)+ \delta_lt(s)} = g^{L(s)} \cdot (g^{t(s)})^{\delta_l} \quad .etc \\
&g^{\Delta} = (g^{L(s)})^{\delta_r} \cdot (g^{R(s)})^{\delta_l} \cdot (g^{t(s)})^{\delta_l \delta_r} g^{-\delta_o}
\end{aligned}
$$
于是既隐藏了加密值，又使得等式可以通过 _有效计算_ 的检查
$$
L \cdot R -O + \textcolor{magenta}{t(\delta_rL + \delta_lR + \delta_l \delta_r t - \delta_o)} = t(s)h+ \textcolor{magenta}{t(s)(\delta_rL+\delta_lR+\delta_l \delta_rt-\delta_o)}
$$
这个结构就是**统计学上的零知识** 因为增加了 $\delta_l, \ \delta_r, \ \delta_o$ 的均匀随机倍数（参见 [Gen+12] 中的定理 13）

> 注意：这种方法和 $V$ 的操作数也是一致的，即：$g_l^{L_p + \delta_lt} \cdot g_l^{L_v} = g^{L_p+L_v+ \delta_l t}$
>
> 因而当且仅当 $P$ 使用了 $V$ 的值来构造证明(即， $\Delta =\delta_r(L_p+L_v) + \delta_l(R_p + R_v ) + \delta_l \delta_r t - \delta_o )$ ，这个有效计算的检查依然是成立的，更多的细节看下一部分

为了使得 “变量多项式限制” 和 “变量值一致性”检查与 _零知识_ 的修改一致，就有必要去增加以下的参数到 _proving key_ 中：
$$g_l^{t(s)}, g_r^{t(s)},g_o^{t(s)}, g_l^{α_lt(s)}, g_r^{α_rt(s)},g_o^{α_ot(s)}, g_l^{βt(s)}, g_r^{βt(s)},g_o^{βt(s)}$$
非常奇怪的是最初的 Pinocchio 协议[Par+13]主要关注可验证的计算，而较少涉及 _零知识_ 性质，这其实只需要一点点小修改，这个几乎是没有什么成本的。

> even@安比实验室: 与前文中的零知方案不同，这里通过相加而不是相乘的方式来确保 prover 知识的零知性。
>
> Pinocchio 协议是针对 GGPR 论文的改进，在3.1节中也提到了实现零知识只需要沿用 GGPR 论文的方法即可，并不是这篇论文的贡献。另外，Pinocchio 协议论文侧重工程实践，在2013年时，零知识证明还并没有得到应用。真正的应用还是自从 ZCash 起始


### zk-SNARK 协议

在这一步步的改进之后，我们得到了最终版本的 zkSNARK，又名 Pinocchio [Par + 13]，协议 （zero knowledge is Optional, 并用紫色标注出来了），就是：

**Setup**
 -  选择生成元 $g$  和加密配对  $e$
 -  将变量总数为 $n$ , 其中输入/输出变量数位 $m$ 的函数  $f(u)=y$ ，转换为阶数为 $d$ 大小为 $n+1$  的多项式形式（QAP） 
$$\{l_i(x),r_i(x),o_i(x)\}_{i \in \{0,……,n\}},t(x)$$
 -  选择随机数  $s, \rho_l, \rho_r, \alpha_l, \alpha_r, \alpha_o, \beta, \gamma$
 -  设置 $\rho_o = \rho_l \cdot \rho_r$   和操作数生成元 $g_l=g^{\rho_l}, \ \  g_r = g^{\rho_r}, \ \  g_o = g^{\rho_o}$
 -  设置 _proving key_：
$$
\begin{aligned}
&(\{g^{s^k}\}_{k \in [d]}, \{g_l^{l_i(s)},g_r^{r_i(s)},g_o^{o_i(s)}\}_{i \in {0,…,n}}),  \\ 
&\{g_l^{\alpha_ll_i(s)},g_r^{\alpha_rr_i(s)}, g_o^{\alpha_oo_i(s)},g_l^{\beta l_i(s)},g_r^{\beta r_i(s)},g_o^{\beta o_i(s)}\}_{i \in \{m+1,…,n\}}, \\
&\textcolor{magenta}{g_l^{t(s)}, g_r^{t(s)}, g_o^{t(s)}, g_l^{\alpha_l t(s)}, g_r^{\alpha_r t(s)}, g_o^{\alpha_o t(s)}, g_l^{\beta t(s)},g_r^{\beta t(s)}, g_o^{\beta t(s)}} )
\end{aligned}
$$

 - 设置 _verfication key_：
$$
\begin{aligned}
&(g^1,g_o^{t(s)}, \ \{g_l^{l_i(s)},g_r^{r_i(s)},g_o^{o_i(s)}\}_{i \in \{ 0,…,m \ \} }, \ \ \ \ g^{α_l},g^{\alpha_r},g^{\alpha_o},g^{\gamma},g^{\beta \gamma})
\end{aligned}
$$

**Proving**
 - 代入输入值  $u$ ，执行 $f(u)$ 计算获取所有的中间变量值  $\{v_i\}_{i \in \{m+1,…, n\}}$
 - 把所有未加密的变量多项式赋值给 $L(x) = l_0(x) + \sum_{i=1}^n {v_i \cdot l_i(x)}$ ，并对 $R(x)$ 和 $O(x)$ 做同样的计算
 - 选择随机数 $\textcolor{magenta}{\delta_l ,\delta_r \ 和 \  \delta_o}$
 - 计算 
$$h(x) = \frac{L(x)R(x)-O(x)}{t(x)} \textcolor{magenta}{+ \delta_r L(x) + \delta_lR(x) + \delta_l \delta_r t(x) - \delta_o}$$
 -  将 prover 的变量值赋值给加密的可变多项式 
$$\textcolor{magenta}{并进行零知识的 \  \delta-转换}\quad g_l^{L_p(s)}= \textcolor{magenta}{(g_l^{t(s)})^{\delta_l}} \cdot \prod_{i=m+1}^n{(g_l^{l_i(s)})^{v_i}}$$
 - 再用同样的方式计算 $g_r^{R_p'(s)}$  和  $g_o^{O_p'(s)}$
 - 为变量值一致性多项式赋值 : 
$$
g^{Z(s)} = \textcolor{magenta}{ 
{\left(g_l^{\beta \ t(s)}\right)}^{\delta_l} 
{\left(g_r^{\beta \ t(s)}\right)}^{\delta_r} 
{\left(g_o^{\beta \ t(s)}\right)}^{\delta_o}}
\cdot \prod_{i=m+1}^n{\left(g_l^{\beta l_i(s)}g_r^{\beta r_i(s)}g_o^{\beta o_i(s)}\right)^{v_i}}
$$
 - 计算证明 
$$
(g_l^{L_p(s)}, g_r^{R_p(s)}, g_o^{O_p(s)}, g^{h(s)}, g_l^{L_p'(s)}, g_r^{R_p'(s)}, g_o^{O_p'(s)}, g^Z)
$$

**Verification**
 - 解析提供的证明为 $(g_l^{L_p},g_r^{R_p},g_o^{O_p},g^h,g_l^{L_p'},g_r^{R_p'},g_o^{O_p'},g^Z)$
 - 将 `输入/输出` 赋值给 verifier 的加密多项式并加 1 ：
$$g_l^{L_v(s)} = g_l^{l_o(s)} \cdot \prod_{i=1}^m{(g_l^{l_i(s)})^{v_i}}$$
      并对 $g_r^{R_v(s)}$  和  $g_o^{O_p}$  做同样的计算

 -  可变多项式约束检查：
$$e(g_l^{L_p},g^{\alpha_l}) = e(g_l^{L_p'},g)$$
    并对 $g_r^{R_p}$   和  $g_o^{O_p}$  做同样的检查

 - 变量值一致性检查：
$$e(g_l^{L_p},g^{\alpha_l}) = e(g_l^{L_p'},g)$$

 - 有效的计算检查：
$$
e(g_l^{L_p}g_l^{L_v(s)},g_r^{R_p}g_r^{R_v(s)}) = e(g_o^{t(s)},g^h) \cdot e(g_o^{O_p}g_o^{O_v(s)},g)
$$

### 结论
我们最终完成了一个允许证明计算的有效协议：

- 简明 (Succinctly) —— 独立于计算量，证明是恒定的，小尺寸的
- 非交互性 (Non-interactive) —— 证明只要一经计算就可以在不直接与 prover 交互的前提下使任意数量的 verifier 确信
- 可论证的知识 (with Argument of Knowledge) —— 对于陈述是正确的这点有不可忽略的概率，即无法构造假证据；并且 prover 知道正确陈述的对应值（即：证据），例如，如果陈述是 “B 是 sha256(a) 的结果” 那么就说明 prover 知道一些值 a 能够使得 B = sha256(a) 成立，因为 B 只能够通过 a 的知识计算出来，换句话说就是无法通过 B 来反算出 a（假定 a 的熵足够）。
- 陈述有不可忽略的概率是正确的  (even@安比实验室: 这里指 Soundness 可靠性)，即，构造假证据是不可行的
- _零知识_ ( zero-knowledge) —— 很“难”从证明中提取任何知识，即，它与随机数无法区分。

> even@安比实验室: 所谓 Argument——论证，区别于 Proof —— 证明。
> Pinocchio 协议是 Argument 而非 Proof。这是因为 Pinocchio 的可靠性是 Computational Soundness，Statistical ZK，这一类的证明系统被称为 Argument。所谓的 Computational Soundness 暗含了这样的事实：如果 Prover 计算能力足够强大的话，可以破坏可靠性。

基于多项式的特殊性质，模运算，同态加密，椭圆曲线密码学，加密配对和发明者的聪明才智才使得这个协议得以实现。

这个协议证明了一个特殊有限执行机制的计算，即在一次运算中可以将几乎任意数量的变量加在一起但是只能执行一次乘法，因而就有机会优化程序以有效地利用这种特性的同时也使用这个结构最大限度地减少计算次数。

为了验证一个证明， **verifier 并不需要知道所有的秘密数据，这一点很关键**，这就使得任何人都可以以非交互式方式发布和使用正确构造的 _verification key_。这一点与只能让一个参与者确信证明的“指定 verifier”方案相反，因而它的信任是不可转移的。在 _zkSNARK_ 中，如果不可信或由单方生成密钥对，则可以实现这个属性。

零知识证明构造领域正在不断发展，包括引入了优化（[BCTV13, Gro16, GM17]），改进例如可更新的 _proving key_ 和 _verification key_（[Gro+18]），以及新的构造方法（Bulletproofs [Bün+17], ZK-STARK [Ben+18], Sonic [Mal+19]）