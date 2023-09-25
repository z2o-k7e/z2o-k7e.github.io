> - 作者：Maksym Petkus
> - 翻译 & 注解：even@安比实验室（even@secbit.io）
> - 校对：valuka@安比实验室
> - [本系列文章](https://arxiv.org/pdf/1906.07221.pdf)已获作者中文翻译授权
> - [翻译原链接](https://secbit.io/blog/2019/12/25/learn-zk-snark-from-zero-part-one/)

[TOC]
### Restricting a Polynomial (限制多项式)

上文说到 : 
1. 多项式的知识其实就是它的系数 $c_0, c_1, …, c_i$ 的知识
2. 上文的协议无法验证 `prover` 是否是真的使用了 verifier 提供的值 $\color{red}g^{s^0},g^{s^1},\dotsc,g^{s^d}$ 来构造证明

协议中, 我们通过对秘密值 _s_ 的幂的加密值再进行求幂来对系数进行“赋值”。我们已经限制了 prover 对 _s_ 幂的加密值的选择, 但是**这个限制并不是强制的** ，也就是说，prover 可以使用任何可能的方法找到满足下面等式的值 $z_p$ 和 $z_h$ 

$$
\begin{aligned}
Z_p &= {(Z_h)}^{t(s)} \quad means \\
g^{Z_p} &=  {g^{Z_h}}^{t(s)} \quad equation \ \ is \ \  satisfied \\
V  \ \  verifies \ \ \ g^{p} &\overset{?}{=} g^{t(s)·h}
\end{aligned}
$$
> [https://arxiv.org/pdf/1906.07221.pdf](https://arxiv.org/pdf/1906.07221.pdf)

再用**寻找到的  $z_p$ 和 $z_h$  来代替 $g^p , \ g^h$  交给 verifier**。
verifier 还是验证  $g^{p} \overset{?}{=} g^{t(s)·h} \ \ 即 \ \ g^{Z_p} \overset{?}{=}  {g^{Z_h}}^{t(s)}$   是否成立 , 自然成立, 此时 $P$ cheat 成功

所以 verifier 需要能够知道 prover 给出的  $\color{red}g^p , \ g^h$  就是用 _s_ 幂的加密值 $\color{red}g^{s^0},g^{s^1},\dotsc,g^{s^d}$  计算的, 而不是其它值算的

来看一个简单例子:  由 1 个变量和及其系数组成的一阶多项式 :  $s$ 对应的加密值为 $E(s) = g^s$ 。这里我们要做的就是确保 prover 是拿 $s$  的加密值 $g^s$  ，**而不是其他值**与其系数 _c_ 做同态相乘的。所以结果一定是这个形式（_c_ 为任意值）：
$$
{(g^{s})}^{c}
$$
解决这个问题的一种方法就是用另一个“变换”的加密值做同样的操作，充当类似算术中“校验和”_（Checksum）_ 的作用，以此确保结果是原始值的求幂值。

这个是通过 **Knowledge-of-Exponent Assumption (简称 KEA)** 方法来实现的，在 [Dam91](https://secbit.io/blog/2020/01/01/learn-zk-snark-from-zero-part-two/#bd26)  中有介绍，更精准一点（注意 $a$ 和 $\textcolor{orange}\alpha$  2 个字符的不同）说：

Alice 有一个值 $a$ ，她想要 Bob 对其进行任意指数的求幂（ $a$  is a generator of a finite field group used），唯一的要求是 Bob 只能对 $a$ 进行求幂，为保证这一点，Alice 要：
 - 选择一个随机数  $\textcolor{orange}\alpha$  
 - 计算  $a' = a^{\textcolor{orange}\alpha}  \pmod{n})$   
 - 提供一个元组  $(a, \ a')$  给 Bob, 然后让他对**这 2 个值执行任意的求幂运算**，返回结果元组  $(b, b')$     ( The α-shift remains the same. i.e.  $b' = b^{\textcolor{orange}\alpha}  \pmod{n}$   )

因为 Bob 无法从元组  $(a, \ a')$ 中提取 $\textcolor{orange}\alpha$  的值 (暴力破解也难以实现)，那么 Bob 只能老老实实地生成有效元组  $(b, b')$  ：
1. Bob 选择一个值 $c$    ( $c$ 可以类比上例的  $p , \ h$ )
2. 计算 $b ={(a)}^{c} \pmod{n}$  和   $b' = {(a')}^c = {(a^{\textcolor{orange}\alpha})}^{c} \pmod{n}$ 
3. 返回 $\left(b, b'\right) = \left({a}^{c}, a^{\textcolor{orange}\alpha c}\right)$ 

有了 Bob 回复的 $(b, b')$  和自己的  $\textcolor{orange}\alpha$  ，Alice 就可以验证等式：
 - $(b)^{\textcolor{orange}\alpha} = b'$
 - ${(a^c)}^{\textcolor{orange}\alpha} = {(a')}^c = (a^{\textcolor{orange}\alpha})^{c}$
 - $a^{c \cdot \textcolor{orange}\alpha} = (a^{\textcolor{orange}\alpha})^{c}$

结论是：
-   Bob 在元组的两个值的计算上都用了同一个指数（即 $c$ ）
-   Bob 只能用 Alice 原本的元组  $(a, \ a')$  来保持  `α-shift`
-   构造验证值  $(b, b')$  的唯一方式是用同一个指数 $c$
-   Alice 并不知道 $c$ ，这和 Bob 不知道  $\alpha$  的原因一样
-   虽然 _c_ 是被加密的，但它的可能取值范围并不足够大到保持其零知识的性质，这个问题我们将在后面“零知识”那一节解决。

最后这个协议提供了一个证明给 Alice ，Bob 确实是用他知道的某个值对 $a$  进行求幂的，而且他也不能做别的任何操作，例如：乘法，加法，因为这样就会破坏  `α-shift` (α-变换关系)

在同态加密中，求幂是对被加密值进行乘法运算。我们可以应用这个结构到一个简单的系数多项式  $f(x) = c \cdot x$  的例子中：

-   verifier 选择随机数 $s ,  \ \textcolor{orange}\alpha$  ，然后令 $x==s$ , 提供一阶及其 "shift" 的计算值： $g^s , g^{\alpha \cdot s}$
-   prover 代入其私有的系数 $c$ 计算:  $\left(\left(g^s\right)^c,\left(g^{\alpha s}\right)^c\right)=\left(g^{c s}, g^{\alpha c s}\right)$   
-   verifier 验证： ${(g^{cs})}^{\alpha} = g^{\alpha c s }$

这个结构“限制” prover 只能用 verifier 提供的加密的  $s$  进行计算，因而 prover 只能将系数 $c$ 赋给 verifier 提供的多项式。

现在我们可以扩展这种单项式(monomial) 上的方法到多项式上，因为计算是先将每项的分配分开计算然后再 “同态地” 相加在一起的（这个方法是 Jens Groth 在 [Gro10](https://secbit.io/blog/2020/01/01/learn-zk-snark-from-zero-part-two/#3068) 中介绍的）。

所以如果 $V$ 给 $P$ 一个 $s$ 的幂及其加密 `shifted` ，$P$ 就可以计算原始的和 `shift` 后的多项式，, where the same check must hold.   对于阶数为  $d$  的多项式：

**verifier** :  提供加密值  $g^{{s}^{0}} , g^{{s}^{1}}, ..., g^{{s}^{d}}$ 和他们的 `α-shift`   $g^{{\alpha s}^{0}} , g^{{\alpha s}^{1}}, ..., g^{{\alpha s}^{d}}$ 
**prover** :  
1. 计算给定的带有 $s$ 的幂的 encrypted polynomial : 
$$\textcolor{red}{g^{p(s)}}=\left(g^{s^0}\right)^{c_0} \cdot\left(g^{s^1}\right)^{c_1} \ldots \ldots\left(g^{s^d}\right)^{c_d}=g^{\left(c_0 s^0+c_1 s^1+\ldots+c_d s^d\right)}$$

2. evaluates encrypted "**shifted**" polynomial  with the corresponding  `α-shift`  of the powers of $s$  : $$
\begin{aligned}
\textcolor{red}{g^{p'} = g^{\alpha p(s)}} &= \left(g^{\alpha s^0}\right)^{c_0} \cdot\left(g^{\alpha s^1}\right)^{c_1} \cdot \dotsc \cdot\left(g^{\alpha s^d}\right)^{c_d} \\
&=g^{c_0 \alpha s^0+c_1 \alpha s^1+\ldots+c_d \alpha s^d}=g^{\alpha \cdot \left(c_0 s^0+c_1 s^1+\ldots+c_d s^d\right)}
\end{aligned}
$$
3. 将计算结果  $\textcolor{red}{g^p, \ \ g^{p'}}$  发给 verfier

**verfier** 校验 : 
$$(g^p)^{\alpha} \ \  \overset{?}{=} \ \ g^{p'}$$


前面的多项式例子  $x^3 -3x^2 +2x$   就变成了：

<img src="http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-02-09-075241.png" width="90%" />

现在我们就可以确保 prover 是**用了 verifier 提供的多项式**而不是其它值做计算的了，因为别的方法不能够保持 `α-shift` 变换。 当然如果 verifier 想要确保在 prover 的多项式中排除了 $s$ 的某些次幂，如 $j$ ， 他就不提供对应的密文及其变换：$g^{s^j} , g^{s^{\alpha j}}$ 

与前面的协议相比，我们现在已经有了一个比较健壮的协议。但是尽管已经做了加密，在 _零知识_ 性质上也还依然存在一个很明显的缺陷：

即即使理论上多项式参数 $c_i$  是一个很广的取值范围内的值，在实际中, 这个范围可能很有限（比如前例中的 6），这就意味着 verifier 可以在有限范围的系数组合中进行暴力破解，获取 $P$ 的知识 , 最终计算出一个与 $P$ 的答案相等的结果 : 

比如 $V$ 将每个系数的取值范围定为 100，多项式阶数为 2，那么大概只会有 100 万种不同的组合，可以认为 $V$ 暴力破解 $P$ 的密钥只需要少于 100 万次的迭代

更重要的是，对于一个安全的协议, 即使在只有 1 个系数，值为 1 的例子中，安全协议也必须能够保证其安全 !!!

> even@安比实验室: 有了 KEA，就可以约束 prover 只能通过用 verifier 提供的加密值去构造证明了。严格点讲，这里是用的是 KEA的扩展版本，叫做 The q-power Knowledge of Exponent Assumption.


### Zero-Knowledge

上文说到  $V$  能从 $P$  发送的数据中暴力破解 $p(x)$  ，来看一下 those provided values (the proof) : 
$$
g^{p} , g^{p'}, g^{h}
$$

双方都参与到了下面的 checks ：
1.  $g^p={(g^h)}^{t(s)}$      (poly $p(x)$ has roots of $t(x)$ )
2. ${(g^p)}^{\alpha}=g^{p'}$         (poly of a correct form is used)

问题是我们如何更换(一种新的)证明 (alter the proof) 使得这些 checks 依然有效，同时又保证没有知识能被提取？

Chap-1 给了一个提示： 我们可以使用随机值 $\delta$  (delta)来 `“shift”` 这些值,  如  ${(g^{p})}^{\delta}$   
现在，为了提取知识，就必须首先要知道一个不可知的值 _δ_。并且，这种随机化在统计学上与随机值没有什么区别。  (原文:  in order to extract the knowledge, one first needs to find _δ_ which is considered infeasible(不可行的). Moreover, such randomization is statistically indistinguishable from random.)

为了保持这种关系，我们在 $V$ 的 checks 中验证一下。等式的每一边都有 prover 提供的值  $\delta$   , 如果我们用 $\delta$   来“变换” **每一个**值，那么等式应该可以保持相等

Concretely (具体来讲)，就是 prover 选择一个随机值 $\delta$  ，并用它对证明中的值进行求幂 (and exponentiates his proof values with $\delta$  )
$$
\left(g^{p(s)}\right)^\delta,\left(g^{h(s)}\right)^\delta,\left(g^{\alpha p(s)}\right)^\delta
$$
> 不要怕,  $g^{p(s)},g^{h(s)},g^{\alpha p(s)}$  我们在前面都已经见过了 

and provides to the $V$  for verification:
$$
\begin{aligned}
& \left(g^p\right)^\delta=\left(\left(g^h\right)^\delta\right)^{t(s)} \quad \quad \textcolor{grey}{like \ \ g^p = {(g^h)}^{t(x)}} \\
& \left(\left(g^p\right)^\delta\right)^\alpha=\left(g^{p^{\prime}}\right)^\delta \quad \quad \textcolor{grey}{like \ \ {(g^p)}^{\textcolor{orange}{\alpha}} = g^{p'}}
\end{aligned}
$$
合并一下(consolidation),  可以看到校验的等式依然成立 (the check still holds) :
$$
\begin{aligned}
& g^{\delta \cdot p}=g^{\delta \cdot t(s) h} \\
& g^{\delta \cdot \alpha p}=g^{\delta \cdot p^{\prime}}
\end{aligned}
$$

注意零知识是如何轻而易举地融入到这个结构中去的，这通常也被称为“无成本的”零知识

> even@安比实验室: 借助这个”无成本的”技巧，就可以轻松实现 zero-knowledge 了。但是这里实现零知识的方法和实际中的 Pinocchio 协议，还有 Groth16 方案略有不同。实际方案中是用乘法乘以  $g^{\delta \cdot t(s)}$ 

### Non-interactivity & Distributed Setup

到现在为止，我们已经讲完了一个交互式的零知识方案。但为什么我们还需要有非交互式呢？因为交互式证明只对 original $V$ 有效，其他任何 $V$ 都不能信任这个 proof，因为：
-  $V$ 可以和 $P$ 串通，告诉 $P$  secret params  $\color{red}s , \alpha$，有了这些参数 $P$ 就肆意伪造 proof 来四处行骗
-   $V$  也可以使用同样的方法自己伪造 proof 
-   $V$ 必须保存 $\alpha$  和  $t(s)$  直到所有相关证明被验证完毕，这就带来了一个可能造成秘密参数泄漏的额外攻击面 (which allows an extra attack surface with possible leakage of secret parameters)

因而 $P$ 就需要分别和每次每个 $V$ 都做交互来证明一个 statement（该多项式的知识）

尽管 `交互式证明` 有它的用处，例如一个 $P$ 只想让一个特定的 $V$ （称为目标 verifier，更多的信息参见 [JSI96](https://medium.com/@imolfar/why-and-how-zk-snark-works-3-non-interactivity-distributed-setup-c0310c0e5d1c#4b56) ）确信，就不能再重复利用同一个证明去向别人证明这个声明了，但是当一个 prover 想让众多的参与者同时或者永久地确信的话，这种方法就很低效了。 **prover 需要保持一直在线并且对每一个 verifier 执行相同的计算**。

因而，我们就需要一个可以被**重复使用，公开，可信，又不会被滥用的秘密参数**

### Pairing: Multiplication of Encrypted Values

Cryptographic pairings (bilinear map) is a mathematical construction, denoted as a function  $e(g^{*}, \ g^{*})$ 

它被给予一个数据集中的 2 encrypted inputs  (e.g.   $g^a, \ \ g^b$ ) , 可以将他们确定性地映射到另一组不同的输出数据集上的它们的乘积，即 
$$
e(g^a, g^b) = e(g, g)^{ab}
$$

<img src="http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-02-09-082715.jpg" width="60%" />

因为源数据集和输出数据集（通常被称为一个 `group` ）是不同的，所以一个配对的结果不能用做其他配对计算的输入。我们可以将输出集（也称为“目标集”）视为“不同的宇宙”。因而我们不能用另一个加密值乘以结果，而且配对这个名称本身也表明了，我们一次只能将两个加密值相乘

> even@安比实验室: 换句话说，配对只支持 `x * y` 这种两个值的乘法，但不支持三个或以上的值相乘，比如不支持  `x * y * z` 
> 
> Pairing 类似于一个 $hash$ ，将所有可能的输入值映射到可能的输出值的集合中的一个元素上，通常情况下这个过程是不可逆的
>
> 注意：乍一眼看过去，这个限制可能会阻碍相关功能的实现，但在 zk-SNARK 中这反而是保证安全模式的最重要性质，参见前文 **remark 3.3**

配对函数  $e(g^{*}, \ g^{*})$  可以初步（and technically incorrect）类比(mathematical analogy) 成:   “交换(swap)” 每一个输出的基数(`base`) 和 指数(`exponent`) 的操作，使得基数 $g$  在交换过程中被修改成了指数的方式，即  $g^a \rightarrow a^g$   , “被转换”的两个输入一起被修改了，这样原始值 $\color{red}a$ 和 $\color{red}b$  就在同一个指数下相乘了，即：
$$
e(g^\textcolor{red}{a},g^\textcolor{red}{b}) =a^g \cdot b^g =(\textcolor{red}{ab})^g
$$

因而因为基数(base) 在“转换”中被修改了，所以在另一个配对中不能再使用这个结果  ${(ab)}^g$ （ 即：$e({(ab)}^g, \ g^d)$ ）构造出想要的加密乘积 $abd$ 了。配对的核心性质可以表示成下面的等式：
$$
e(g^a,g^b) =e(g^b,g^a)=e(g^{ab},g^1)=e(g^1,g^{ab}) =e(g^1,g^a)^b =e(g^1,g^1)^{ab} = e(g,g)^{ab}
$$

> Note：配对操作是通过改变椭圆曲线来实现这些性质的，现在我们用的符号 $g^n$ 就代表曲线上一个由生成元 $g$  自相加了 n 次的点，而不是我们前面用到的乘法群生成元。
> The survey [DBS04](https://medium.com/@imolfar/why-and-how-zk-snark-works-3-non-interactivity-distributed-setup-c0310c0e5d1c#0ea5) provides a starting point for exploration of the cryptographic pairings.

Technically, 配对的结果是目标集(target set) 的不同 generator $g$  下原始值(raw value) 的加密产物(encrypted product)，即 $e(g^a, \ \ g^b)=g^{ab}$ 。 因此它具有同态加密的性质，例如，我们可以将多个配对的加密乘积加在一起：
$$
e(g^a,\ g^b) \cdot e(g^c,\  g^d) = \textcolor{red}{g^{ab} \cdot g^{cd} =g^{ab + cd}} =e(g,g)^{ab + cd}
$$
> 注意：配对操作是通过改变椭圆曲线来实现这些性质的，现在我们用的符号  $g^n$  就代表曲线上一个由 $g$ 自相加了 $n$ 次的点，而不是我们前面用到的乘法群生成元。
> [DBS04](https://medium.com/@imolfar/why-and-how-zk-snark-works-3-non-interactivity-distributed-setup-c0310c0e5d1c#0ea5) 这个 survey 提供了学习 Pairing 的 starting point

### Trusted Party Setup

有了 cryptographic pairings，我们现在就准备去设置安全公开且可复用的参数了。假定一下我们让一个诚实的参与方来生成秘密值  $s$  和  $\alpha$ .   一旦 $\alpha$  / $s$  的幂及其对应的 `α-shift` 被加密，那么原始数据就必须要被删除 (  $i \in \{0, 1,\ldots,d\}$ ) :
$$g^\alpha, \quad g^{s^i}, \quad g^{\alpha s^i}$$

这些参数通常被称为 _common reference string_ (CRS) .  CRS 生成后，任何的 $P$ 和 $V$ 都可以使用它来构造 `非交互式的` 零知识证明协议。CRS 的优化版本将包含目标多项式的加密值 $g^{t(s)}$   (While non-crucial)

把 `CRS` 分成两组  ：
1.  _proving key_ (alse called _evaluation key_) :  $(g^{s^i},\ \ g^{\alpha s^i}), \ \ i \in \{0, 1,\ldots,d\}$
2.  _verification key_ :  $(g^{t(s)},\ \ g^{\alpha})$

使用 Pairing 就可以将 `加密值相乘` (记得第一节说过, 加密值不能直接相乘, 会破坏同态的性质)， $V$  就可以在协议的最后一步验证多项式了 (有了 *verification key* ，$V$  就可以处理从 $P$ 那里得到的加密多项式的值  $g^p, \ g^h, \ g^{p'}$  : 

- 在加密空间中校验  $p(x) = t(x) \cdot h(x)$ ： 
$$e(g^p,g^1) \overset{?}{=} e(g^t,g^h) \quad \ \ which \ is \ equivalent \ to  \quad \ e(g,g)^p \overset{?}{=} e(g,g)^{t \cdot h}$$
- Chech polynomial **Restriction** : 
$$
\begin{aligned}
e(g^p,g^\alpha) &\overset{?}{=} e(g^{p'},g) \\
e(g,  g)^{\alpha \cdot  p} &\overset{?}{=} e(g, g)^{p'} \\
\end{aligned}
$$
Recall what is $\color{red}g^{p'}$  [[#Zero-Knowledge]] :  
$$\begin{aligned}
\textcolor{red}{g^{p'} = g^{\alpha p(s)}} &= \left(g^{\alpha s^0}\right)^{c_0} \cdot\left(g^{\alpha s^1}\right)^{c_1} \cdot \dotsc \cdot\left(g^{\alpha s^d}\right)^{c_d} \\
&=g^{c_0 \alpha s^0+c_1 \alpha s^1+\ldots+c_d \alpha s^d}=g^{\alpha \cdot \left(c_0 s^0+c_1 s^1+\ldots+c_d s^d\right)}
\end{aligned}$$

#### Trusted MPC

尽管受信任设置很有效率，但众多 CRS 用户也必须要相信生成者确实删除了 α 和 s ，这一点没有办法证明（_proof of ignorance_ 是一个正在积极研究的领域 [DK18](https://medium.com/@imolfar/why-and-how-zk-snark-works-3-non-interactivity-distributed-setup-c0310c0e5d1c#2823)），所以这种方法依然是无效的。因而很有必要去最小化或者消除这种信任。否则一个不诚实的参与方就可以构造假证明而不被发现。

一种解决办法就是由**多个参与方**使用前面小节中介绍的数学工具来生成一个组合式CRS，这样这些参与方就都不知道「秘密」了。下面是一个实现方案，我们假设有三个参与者 Alice，Bob 和 Carol ，对应为 A，B 和 C，其中  i 为 1, 2, …, d：

1. Alice 选择随机数  $s_A$  和  $\alpha_A$ ，然后公开她的 CRS：
    1. $(g^{s^i_A}，g^{α_A}，g^{α_As^i_A})$
2. Bob 选择他的随机数  $s_B$  和  $\alpha_B$  ，然后通过同态乘法结合 Alice 的 CRS：
    1. $((g^{s_A^i})^{s_B^i},(g^{α_{A}})^{α_B},(g^{α_As_A^i})^{α_Bs_B^i}) = (g^{(s_As_B)^i},g^{α_Aα_B},g^{α_Aα_B(s_As_B)^i})$
3. 然后公开两方 Alice-Bob 的 CRS 结果：
    1. $(g^{s^i_{AB}},g^{α_{AB}},g^{α_{AB}s^i_{AB}})$
4. Carol 用她的随机数  $s_C$  和  $\alpha_C$   做同样的事：
    1. $((g^{s^i_{AB}})^{s^i_C},(g^{α_{AB}})^{α_C},(g^{α_{AB}s^i_{AB}})^{α_Cs^i_C}) = (g^{(s_As_Bs_C)^i},g^{α_Aα_Bα_C},g^{α_Aα_Bα_C(s_As_Bs_C)^i})$
5. 然后公开 Alice-Bob-Carol 的 CRS:
    1. $(g^{s^i_{ABC}},g^{α_{ABC}},g^{α_{ABC}s^i_{ABC}})$
6. 这个协议最后我们就获得了一个混合的  $s^i$  和 $\alpha$ ：

$$
s^i=s^i_As^i_Bs^i_C, \ \ \  \alpha=\alpha_A \ \alpha_B \ \alpha_C
$$

除非他们串谋，否则参与者们互相之间并不知道其他人的秘密参数。实际上，一个参与者必须要和其它所有的参与者串谋才能得到 _s_ 和 _α_，这样在所有的参与者中只要有一个是诚实的，就没有办法伪造证明。 

> 注意：这个过程可以被尽可能多的参与者重复完成

有一个问题是如何验证参与者在生成 CRS 时用的随机数值是一致的，因为攻击者可以生成多个不同的随机数  $s_1, s_2, ... ,$  和   $\alpha_1, \alpha_2, ... ,$   ，然后代入这些不同的随机数去执行 _s_ 的不同次幂计算（或提供随机数作为一个 CRS 的扩充），从而使 CRS 无效或者不可用。

庆幸的是，因为我们可以使用配对来乘以加密值，所以我们就可以从第一个参数开始逐一执行一致性校验，并且确保了每个参数都源于前一个。

我们用 s 的 1 次幂作为标准来校验每一个其它次幂的值与之是否保持一致 : 
- $e(g^{s^i},g) = e(g^{s^1},g^{s^{i-1}})|_{i\in{2,3,4,\cdots,d}}$
- 例如 : 
    - 2 次幂： $e(g^{s^2},g) = e(g^{s^1},g^{s^1}) => e(g,g)^{s^2} = e(g,g)^{s^{1+1}}$
    - 3 次幂： $e(g^{s^3},g) = e(g^{s^1},g^{s^2}) => e(g,g)^{s^3} = e(g,g)^{s^{1+2}}$

我们现在再验证一下前面步骤中 α-变换后的值是否正确：
- $e(g^{s^i},g^α) = e(g^{αs^i},g)|_{i\in[d]}$
- 例如：
    - 3 次幂： $e(g^{s^3},g^α) = e(g^{αs^3,g}) => e(g,g)^{s^3 \cdot α} = e(g,g)^{αs^3}$

> $i \in [d]$ 是 $i \in 1, 2, \cdots, d$  范围的缩写形式，在后面会经常看到

当我们在验证每一个参与者秘密参数的一致性时，要注意参与者生成 CRS 的过程并没有强制后一个参与者（就是我们例子中的 Bob 和 Carol）都要使用前面已经公开的 CRS。因而如果一个攻击者是链上的最后一个参与者，他可以像链上的第一个参与者一样忽略前面的 CRS 随便构造一个有效的 CRS，这样他就变成了唯一一个知道秘密 _s_ 和 _α_ 的人。

为了解决这个问题，我们可以额外再要求除了第一个以外的每一个参与者去加密然后公开他的参数。例如，Bob 同样公开了 :  $(g^{s^i_B},g^{α_B},g^{α_Bs^i_B})|_{i\in[d]}$

这就可以去验证 Bob 的 CRS 是乘以了 Alice 的参数后正常获得的，$i \in [d]$ : 

$$
\begin{aligned}
e(g^{{s^i}_{AB}},g) &= e(g^{{s^i}_A},g^{{s^i}_B}) \\
e(g^{α_{AB}},g) &= e(g^{α_A},g^{a_B}) \\
e(g^{α_{AB} \ {s^i}_{AB}},g) &= e(g^{α_A{s^i}_A},g^{α_B{s^i}_B})
\end{aligned}
$$

同样的，Carol 也必须证明她的 CRS 是乘以了 Alice-Bob 的 CRS 后正常获得的。

这是一个健壮的 CRS 设置模式，它并不完全依赖于单个参与者。事实上，即使其它所有的参与者都串谋了，只要有一个参与者是诚实的，他能够删除并且永远不共享它的秘密参数，这个 CRS 就是有效的。所以在设置 CRS （有时候被称为仪式 [Wil16](https://medium.com/@imolfar/why-and-how-zk-snark-works-3-non-interactivity-distributed-setup-c0310c0e5d1c#191c)）的时候有越多不相关的参与者参与，伪造证明的可能性就越低。当有相互竞争的参与方参与的时候，就几乎不可能伪造证明了。这种模式能够包容其他一些怀疑这种 setup 可识别性的不受信方因为校验步骤确保了他们不会破坏（这里也包括很弱的 _α_ 和 _s_ 的使用）最终的 CRS。

>even@安比实验室: 现在有一些zkSNARK方案支持可升级的 CRS，任何怀疑CRS的参与方都可以对CRS 进行更新。此外还有一些 zkSNARK方案支持 Universal CRS，用不着对每一个电路进行受信任设置，而是只需要全局完成一次即可。除此之外，大量无需 Trusted Setup 的方案正在被充分研究。


### Succinct Non-Interactive Argument of Knowledge of Polynomial

We are now ready to **consolidate** the **evolved** _zk-SNARKOP_ protocol. (准备整合演进的 zk-SNARKOP 协议) , now  $\{s^i\}_{i\in[d]}$  denotes a set  $\{s^1, s^2, \cdots, s^d\}$ 

我们已经明确 target poly  $t(x)$ 和 $P$ 的多项式阶数 $d$  :

**Setup**
 - 挑选随机值  $s, \ \textcolor{orange}{\alpha}$
 - 计算加密值  $g^\alpha$  ,  $\{g^{s^i}\}_{i\in[d]}$  , $\{g^{αs^i}\}_{i\in\{0,\ldots,d\}}$
 - 生成 _proving key_： $(g^{s^i},\ \ g^{\alpha s^i}), \ \ i \in \{0, 1,\ldots,d\}$     (和上面相同)
 - 生成 _verification key_： $(g^\alpha, \ g^{t(s)})$    (和上面相同)

**Proving** 
 - 分配多项式系数 $\{c_i\}_{i\in\{0,…,d\}}$  (即知识),   $p(x) = c_dx^d +…+ c_1x^1 + c_0x^0$  
 - 自己求多项式  $h(x) = p(x)  \ / \ t(x)$      (一般用 FFT 完成 ?)
 - 代入 $\{g^{s^i}\}_{i\in[d]}$  计算多项式 $g^{p(s)}$  和  $g^{h(s)}$  的值
 - 代入 $\{g^{αs^i}\}_{i\in[d]}$  计算变换多项式  $g^{αp(s)}$ 的值
 - 选择随机数  $\delta$      ("零成本"的 zero-knowledge)
 - 构造随机化的证明(randomized proof) :   $π = (\textcolor{blue}{g^{\delta p(s)}}, \ \textcolor{green}{g^{\delta h(s)}}, \ \textcolor{red}{g^{\delta \alpha p(s)}})$

**verification**  :
- Parse proof(解析证明)  $\pi$  as   $(g^p,g^h,g^{p'})$
	- 我觉得这里的表述有问题, 因为 $V$ 是不知道 $\delta$ 的(也不需要知道) ,  $\delta$ 是 $P$ 用零知识武装自己的关键工具, $V$ 不需要解包或还原 $\delta$ ，$V$ 只需用 Pairing 验证证明的一致性 : 
- 验证多项式约束： $e(g^{p'},g) = e(g^p,g^\alpha)$
	- $e(g^{\textcolor{orange}{\alpha}} , \ \textcolor{blue}{g^{\delta p(s)}}) = e(\textcolor{red}{g^{\delta \alpha p(s)}}, \  g)$      
	- —————— $\textcolor{orange}{\alpha}$ 保证 $P$  确实用了 $Setup$ 提供的 $g^{s^i}$
- 验证多项式系数： $e(g^p,g) = e(g^{t(s)},g^h)$
	- $e(g^{\delta p(s)}, g) = e(g^{t(s)}, \ \textcolor{green}{g^{\delta h(s)}})$      
	- ——————  $\delta$  保护了 $P$ , 实现了零成本 Zero-knowledge

**Remark 3.3** 如果 pairing 的结果有可能在其它类似的乘法协议中被复用，那么这里就完全没有安全性可言了，因为这样的话 $P$ 可以自己构造  $g^{p'} = e(g^p,g^\alpha)$ , 
> 这里我理解就是 $e(g^p,g^\alpha)$   被 $P$ 拿到了并复用了,  然后他可以发送   $e(g^p,g^\alpha)$  作为 $g^{p'}$ 的值来 cheat   $V$

这样也可以通过“多项式约束”的检查： $e(e(g^p,g^\alpha)，g) = e(g^p,g^\alpha)$   —— 因为这是个恒等式, $V$ 去验证一个 "恒等式" 没有任何意义 —— 结果永远是 `Accept` .

### Conclusion

我们用 zk-SNARK 协议来解决多项式问题的知识，不过这是一个有局限的例子。因为大家可以说 $P$ 只要用另外一个有界的多项式去乘以  $t(x)$  就可以很容易得构造出一个能够通过测试的多项式 $p(x)$  ，并且这种结构也是有效的。

$V$ _知道_ $P$ 有一个有效的多项式，但是并不知道是哪一个。我们可以利用多项式的其他性质添加额外的证明，如: 被多个多项式整除，是某个多项式的平方。虽然可能会有一个服务能够接受，存储和奖励所有经过证明的多项式，或者有一个需求，加密计算某种形式的未知多项式。然而若有通用方案就可以支撑无数的应用。

> even@安比实验室:总结一下这篇文章中一步一步解决了下面的几个问题：
> 1. 保证 prover 的证明是按照规则正确构造的 ——> KEA   ( $a' = a^{\textcolor{orange}\alpha}  \pmod{n})$ )
> 2. 保证知识的零知性 ——> “无成本的” $\delta$  变换
> 3. 可复用证明 ——> 非交互式
> 4. 非交互中如何设置安全公开且可复用的参数 ——>  参数加密，verifier 借助 airing 进行验证
> 5. 保证参数的生成者不泄密 ——> MPC's Setup
> 
> 至此，一个用来证明多项式知识的完整的 zk-SNARK 协议就构造出来了，不过现在的协议在通用性上依然还有很多限制，后面的文章将继续介绍如何构造通用的 zk-SNARK。

Ref : 
 - https://secbit.io/blog/2020/01/01/learn-zk-snark-from-zero-part-two/
 - https://medium.com/@imolfar/why-and-how-zk-snark-works-2-proving-knowledge-of-a-polynomial-f817760e2805
 - https://medium.com/@imolfar/why-and-how-zk-snark-works-3-non-interactivity-distributed-setup-c0310c0e5d1c