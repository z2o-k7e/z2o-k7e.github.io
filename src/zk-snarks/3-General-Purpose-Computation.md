> - 作者：Maksym Petkus
> - 翻译 & 注解：even@安比实验室（even@secbit.io）
> - 校对：valuka@安比实验室
> - [本系列文章](https://arxiv.org/pdf/1906.07221.pdf)已获作者中文翻译授权
> - [翻译原链接](https://secbit.io/blog/2019/12/25/learn-zk-snark-from-zero-part-one/)

[TOC]

### Computation

Let us consider a simple program in pseudocode:

```bash
Algorithm 1: Operation depends on an input 
—————————————————————————————————————————————————————————
function calc(w, a, b)         
    if w then         
        return a × b         
    else         
        return a + b         
    end if         
end function
```

Therefore we need to find a way to convert a **program** into the **polynomial form** , like this
$$
f(w,a,b) = w(a · b)+(1-w)(a+b) 
$$

Executing  $calc(1, 4, 2)$  and evaluating   $f  (1, 4 , 2)$  will yield the same result: 8. 
$calc(0, 4, 2)$ and   $f (0 , 4 , 2)$  would both be resolved to 6. We can express any kind of finite program in such a way.

> 猜想一下，是否只要是能够用多项式表示的程序都可以做证明？

### Single Operation

Any **computation** at it is core consists of elemental operations of the form:

$$
左操作数 \quad {运算操作符} \quad 右操作数 = 输出
$$

If we can represent operand values as polynomials (and we indeed can as outlined) then through the arithmetic properties, we will be able to get the result of an operation imposed by an operand. (**如果我们可以将操作数的值表示为多项式(我们也确实可以这么做)，那么利用算术属性，我们就能够得到操作数的计算结果了。**)

> @Even : 回忆一下，在本系列的第一篇——多项式的性质与证明中，我们曾经说过“任何多项式在任意点的计算结果都可以看做是其唯一身份的表示。”
> 
> 反过来当我们知道某个多项式的时候，是不是也就意味着我们知道多项式上某个点的取值。这就是借助多项式来完成证明的依据。

### Enforcing Operation

如果一个 prover 声称有某 2 个数字的乘积，verifier 要怎样去验证呢？

Recap computation form, 我们也可以将其表示为一个运算多项式 : 

$$
\begin{aligned}
&左操作数 \quad &\textbf{运算符} \quad &右操作数 &= &输出  \\
&l(x) \quad &\textbf{operator} \quad  &r(x) &= &o(x) \\  
\end{aligned}
$$

在计算过程中, 如果**操作数(operands)** 和 **结果(output)** 都能用**多项式**的形式正确地表示出来，那么 $l(a) \ \  \textbf{operator} \ \  r(a) = o(a)$  就应该成立

也就表明, 当取值为 $a$  时, 多项式   $l(a) \ \  \textbf{operator} \ \  r(a) - o(a) =  0$   成立,

即该多项式一定有一个根  $a$ , 因此，这个多项式里面一定包含因式(cofactor)  $\color{red}(x-a)$ , 这就是我们要证明的目标多项式(target polynomial) ，即  $t(x) = (x-a)$ 

For example, let us consider operation:  $3 \times 2 = 6$

可以用一个简单的多项式表示它：  $l(x) = 3x, \ \ r(x) = 2x,\ \  o(x) = 6x$ 
取  $a=1$ ，即   $l(1) = 3; \ \ r(1) = 2; \ \ o(1) = 6$

运算多项式就变成了 : 

$$
\begin{aligned}
&l(a) \ \  \times \ \  r(a)  =  o(a) \\
&3x \times 2x = 6x \\ 
&6x^2 - 6x = 0
\end{aligned}
$$

因而如果  $P$  用多项式  $l(x), \ r(x), \ o(x)$  来代替 $p(x)$ ，**因其依然可被 $t(x)$ 整除，所以 $V$ 就认可其是有效的**

相反，如果 prover 尝试用 $4$ , 即  $o(1) = \color{red}4$ 来代替输出值去欺骗 verifier ，即 $o(x) = 4x$ ，那么运算多项式就变成了 $6x^2 \  – \ 4x= 0$  ,  这个多项式并没有 $x=1$ 的解，因而 $l(x) \ × \ r(x) \ –  \ o(x)$  不能被  $t(x)$  整除：

<p float="left">
<img src="http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-07-28-035012.jpg" width="70%" />
</p>
因而 $V$ 不会接受这个计算结果（就像**因式分解**这一章描述的那样）

> 在前面的协议中，我们要证明的多项式是 $p(x) = t(x)h(x)$ ，这里我们把  $p(x)$  替换成  $p(x) = l(x)r(x) \ - \ o(x)$ , 这仍然是被 $V$ 承认有效的。这里目标多项式 $t(x)$ 的根就是对应能够计算出数学表达式的值的 $x$
>
> 上面例子里面取 $x=1$ 作为运算编码的位置,  1 可以换成任何别的值，比如说 $x=2，3，或101$ .. 在 [GGPR] 与 [PHGR] 论文中，这个取值是一个随机值，被称为 “root”

### Proof of Operation

前面**多项式的 SNARK**一章，我们已经能够证明多项式 $p(x)$ 的知识了，只不过现在要计算的是三个多项式  $l(x), r(x), o(x)$  的知识。我们可以定义 $p(x) = l(x) \times r(x) \ – \ o(x)$ ，但这里存在两个争议点。
1. ① 在我们的协议中, 证明阶段是不能做加密值乘法计算的 (即  $l(s) \times r(s)$  )，因为 Pairing 只能用一次(不能复用, 会有安全风险?)  —— Pairing 要用在校验多项式的约束上
2. ② 这里给证明者留下了一个可以修改多项式结构(修改知识) 但依然保留有效因式 $t(x)$ 的机会，for example   $\color{pink}p(x) = l(x)$  or  $\color{pink}p(x) = l(x) - r(x)$  or even $\color{pink}p(x) = l(x) \times r(x) + o(x)$  ——  只需要 $p(x)$ 有一个根 $(x=a)$  就可以骗过 $V$ , 这样是不行的 !

所以 $P$ 必须要 **分别提供** 多项式  $l(s), r(s), o(s)$  值的证明，即**协议必须修改要证明的多项式的知识**( _knowledge of polynomial_ must be adjusted.) 

In essence(本质上),  $V$ 在加密空间中要验证的是  $\color{red}l(s) \times r(s) \ – \ o(s) = t(s) \ h(s)$  . 

即使  $V$  可以用 `Pairing` 来执行乘法(multiplication)，但在 `Pairing` 中做减法  ( $– \  o(x)$ ) 是非常昂贵的计算（would require to find inverse of $g^{o(s)}$ ），所以咱们把 $o(x)$ 移到右边：  
$$l(x)r(x) = t(x)h(x) + o(x)$$

在加密空间中，$V$ 的验证就可以转换成：
$$
\begin{aligned}
e\left(g^{l(s)}, g^{r(s)}\right) & =e\left(g^{t(s)}, g^{h(s)}\right) \cdot e\left(g^{o(s)}, g\right) \\
e\left(g, g\right)^{l(s) r(s)} & = \textcolor{red}{e\left(g, g\right)^{t(s) h(s)} \cdot e\left(g, g\right)^{o(s)}} \\
e(g, g)^{l(s) r(s)} & =e(g, g)^{t(s) h(s)+o(s)}
\end{aligned}
$$

> Red Part:   recall that the result of cryptographic `pairings` supports encrypted addition through **multiplication**,  see [section on pairings](https://medium.com/@imolfar/why-and-how-zk-snark-works-3-non-interactivity-distributed-setup-c0310c0e5d1c#f62b).

$$
Recall: \quad e(g^a,\ g^b) \cdot e(g^c,\  g^d) = \textcolor{red}{g^{ab} \cdot g^{cd} =g^{ab + cd}} =e(g,g)^{ab + cd}
$$

保持 setup 阶段不变，协议更新为：

![](http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-07-28-041907.jpg)

这个协议就能够证明两个值相乘的计算结果是正确的了。

你可能注意到了在这个新的协议中我们放弃了 $\delta$  - _零知识_ 部分。这么做是为了简化协议,  后面的章节我们会再变回零知识 ~

> even@安比实验室：上面例子里面取 $x=1$ 这个特殊值作为运算编码的位置。当然这里的 1 可以换成任何别的值，比如说换成 $x=2, \ 3, \ 或 \ 101$  等等。在[GGPR] 与 [PHGR] 论文中，这个取值是一个随机值，被称为 "root"

#### 名词定义

operand :  符号左边叫 left operand , right operand
 - 是具体的操作数, 比如  $a \times b$  里的 a & b ;     $2 \times 3$ 里的 2 & 3

_oprand polynomials :  l_(_x_) and   _r_(_x_).
 - _left operand polynomial_ (green)  几个约束等式的操作数左边竖列, 构成的 poly 叫  _left operand polynomial_  
 - _right operand polynomial_ (blue)   ....

_output polynomials_ :  等式右边的所有 Output 操作数 竖列 构成的 poly 叫 _output polynomials_

#### Multiple Operations

We can prove a **single operation,** but how do we scale(拓展) to prove  $\color{red}multiple \  operations$  (which is our ultimate goal)?   Let us try to add just one another operation. Consider the need to compute the product:  $\color{red}a \times b \times c \times d$  :

来看一个有三个乘法运算的例子 `2 × 1 × 3 × 2`，它按照下面的步骤执行：

$$
\begin{aligned}
&{\color{green}2} \times {\color{blue}1} = {\color{red}2} \\
&{\color{green}2} \times {\color{blue}3} = {\color{red}6} \\
&{\color{green}6} \times {\color{blue}2} = {\color{red}12}
\end{aligned}
$$

我们要把它们表示为多项式，对于  $x \in \{1, \ 2, \ 3\}$  ， $l(x)$ 相应的要  $= \ 2，2  \ 和 \  6$  。 即通过点 $(1, 2), (2, 2), (3, 6)$ ， 同样的:   $r(x) \in (1, 1),(2, 3),(3,2)$ ， $o(x) \in (1, 2), (2, 6), (3, 12)$

we use **Polynomial Interpolation** to represent these.

Interpolation Result : 

$$
\textcolor{green}{l(x)}= 2x2-6x+6 ; \quad 
\textcolor{blue}{r(x)}=\frac{-3x^2+13x-8}{2}; \quad \textcolor{red}{o(x)}=x^2+x
$$

#### Multi-Operation Polynomials

Now we have operand polynomials  $l(x),..,o(x)$ ,  let us see step-by-step how the correctness of each operation is verified. 

Recall that a verifier is looking for equality   $p(x) = \color{red}{l(x)  \times r(x) \  –  \ o(x) = t(x)h(x)}$ . 

本例中，计算是在点  $x \in \{1,2,3\}$  处被表示出来的，所以 target poly  $t(x)$ 在这些点处必须 evaluation 为 $\color{red}0$，换句话说， $t(x)$  的根 root 必须是 1，2 和 3，它的基本形式就是：
> 在实际过程中, $x$ 一般是放到单位根 root of unity  ——  $\omega$ 里的
$$
t(x) = (x-1)(x-2)(x-3)
$$

Firstly,  $l(x) \times r(x)$  are multiplied which results in:
<p><img src="http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-07-28-052328.jpg" width="70%" /></p>


Secondly, the  $o(x)$  is subtracted from the result of  $l(x) \times r(x)$  which is  $\color{red}l(x)  \times r(x) \  –  \ o(x)$:

<p><img src="http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-07-28-052050.jpg" width="70%" /></p>

已经可以看出每一个 operands multiplication 都对应了正确的结果。最后一步 $P$ 要算出一个有效因式：

$$
h(x) = \frac{l(x) \times r(x) -o(x)}{t(x)} = \frac{-3x^4 +22x^3 -57x^2 +63x -24}{(x-1)(x-2)(x-3)}
$$

通过长除法(long division) 可以算出： $h(x) = -3x + 4$ : 

<img src="http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-07-28-055417.jpg" width="60%" />
$V$ 自己代入  $h(x) = -3x + 4$  ，$V$ 可以自己计算 $t(x) h(x)$  : 
$$
\begin{aligned}
&\textcolor{red}{h(x)} = -3x + 4 \ ; \quad 
\textcolor{red}{t(x)} = (x-1)(x-2)(x-3) \\
&\textcolor{blue}{t(x) h(x)} = -3x^4 + 22x^3 -57x^2 + 62x -24
\end{aligned}
$$
> PS: 这里为了简化过程, 省略了完整协议中的 `δ-zero-knowledge` 和 `α-shift`

现在显然  $p(x) = \color{red}l(x) \times r(x) \ – \  o(x) = t(x)h(x)$  ，这就是我们要证明的内容。

> 这里只用了一组多项式   $l(x), \ r(x), \ o(x)$  就将所有计算的约束关系表示出来了，有几步计算, 也就对应着目标多项式  $t(x)$  要有几个根                (这里我这么理解: 计算的步数多了, 那么 $t(x)$ 的根也就多了, 比如可能是 $(x-\omega^0) \cdot (x-\omega^1) \cdot \ldots \cdot  (x-\omega^9)$   , 因为约束等式的行数多了, 也就需要同步约束这些等式符合所有的计算完整性验证 )
> 
> 当前的协议似乎存在一些缺陷，多项式只能证明 $P$ 拥有一组多项式  $l(x), \ r(x), \ o(x)$ ，在  $t(x)$  的几个根的取值处  $l(x) \cdot r(x)  \ - \ o(x)=0$ ，但无法证明这组多项式符合我们要证明的数学表达式：
> 1）多个计算关系之间也是分开表示的，这些算式之间的关系也同样无法进行约束
> 2）由于 $P$  生成的证明中只有计算结果，左操作数，右操作数，输出在计算中混用也不会被发现
> 3）由于左操作数，右操作数，输出是分开表示的，他们互相之间的关系无法进行约束


### Variable Polynomials
  
现在我们可以一次证明多个运算（如上百万个甚至更多）了，但是前文结尾提到了几个关键缺点(critical downside)

**如果证明中执行的"程序"在不同运算中使用了相同的变量作为操作数或输出**，例如：

$$
\begin{aligned}
&{\color{green}a} \times {\color{blue}b} = {\color{red}r_1} \\
&{\color{green}a} \times {\color{blue}c} = {\color{red}r_2}
\end{aligned}
$$

然而，因为我们的协议中是允许 $P$ 为**多项式设置任何系数**的，所以他可以不受限制得为不同计算中的  $\textcolor{green}a$  设置不同的值，如：

<img src="http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-07-28-080707.jpg" width="70%" />
This freedom breaks **consistency** and allows $P$  to prove the execution of some other program which is not what verifier is interested in. (**这种自由打破了一致性**, 允许 $P$ 去证明一些无关的程序执行)

Therefore we must ensure that any variable can only have **a single value** across every operation it is used in. (因而我们必须要确保每一个变量在所有运算中出现的地方都只有一个取值。)

> 注意：文中的 `variable` 与常规的计算机科学中 `variable` 的定义不同，这里的变量是不可改变的(immutable),  而且每次执行都只赋值一次(only assigned once per execution)     (即示例伪代码中的那些不会被修改的变量)
> 
> zkSNARK 论文中，这个「变量」其实有一个对应的名词叫做 `assignment`，是算术**电路**的「赋值」，对应的是问题结构或者说算术电路的结构。而所有的 assignments 是一个算术电路可满足性问题的解，包含了算术电路的输入值以及电路运算过程中输出的中间结果值 (没看懂这里)


#### Single-Variable Operand Polynomial

那么, How to ensure  每一个变量在所有运算中出现的地方都只有一个取值?  究其原因,  $P$  可以设置不同值是因为他可以任意控制 $x$ 的系数 
$$
\begin{align}
&{\color{green}a} \times {\color{blue}b} = {\color{red}r_1} \tag{①} \\
&{\color{green}a} \times {\color{blue}c} = {\color{red}r_2} \tag{②}
\end{align}
$$
a **malicious** $P$  可以分别为第一第二行的  $\color{green}a$  分配不同的值, 比如分别分配 $2$ 和 $5$  : 
- assign  $2$  for  $\color{green}a$  in raw ① , 那么此处 $l(x)$ 的系数为 $2$ , 即函数通过点 $(1,2)$ 
- assign  $5$  for  $\color{green}a$  in raw ②, 那么此处 $l(x)$ 的系数为 $5$ , 即函数通过点 $(2,5)$ 

此时就出现了不一致问题 —— 那么如果对于同一个变量  $\color{green}a$  , 这些系数是固定的，就可以解决问题了

如下是 2 个**包含相等值的多项式** : 它们分别都表示了有两个相等值对应的运算（即在 $x=1$ 和 $x=2$ 处），第一个多项式的取值为 1，第二个多项式的取值为 2：

<img src="http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-07-28-081653.jpg" width="70%" />

> 注意: 这 2 个多项式的相应的系数是成比例的 —— 第 2 个多项式的系数是第 1 个的 **2** 倍

那么由于多项式的算术性质，如果我们想要**同时地改变多项式中所有的值** , 我们就需要改变它的比例，如果我们用一个数字 $n$ 乘以多项式，那么多项式值就会等比例变换为 $n$ 倍

Consequently, if a verifier needs to **enforce the prover to set the same value in all operations**, then it should only be possible to modify the proportion and not the individual coefficients.  
(因此，**如果 $V$ 需要在所有计算中强制 $P$ 设置相同的值，他就要限制 $P$ 只能改动多项式的比例, 而不是恣意篡改某处操作的单个系数**)

怎么**保持系数比例不变**呢？对于这个问题我们可以先思考一下在 左运算多项式 (left operand polynomial) 中我们提供的 Proof  是什么 —— 是  $l(x)$  在一些秘密值  $s$  处的加密值：$g^{l(s)}$  

上文中, 我们已经知道怎样通过 `α-shift` 去限制 $P$ 只能使用 $V$ (或 $Setup$ ) 提供的  $\color{red}s$  的幂做计算，来使得单个运算能够满足同态乘法    (such that homomorphic multiplication is the single operation available.)

和限制单个求幂值相似， $V$ 可以**一次限制完整的多项式** 。而不只是提供单独的加密及其   `α-shift` : 

$${g^s}^1, \ {g^s}^2, \ {g^s}^3, \ \ldots, {g^s}^d, \ \ \quad  {g^{\alpha s}}^1, \  {g^{\alpha s}}^2 ,\ \ldots ,  {g^{\alpha s}}^d$$

协议的过程是：

**Setup :**
 - 使用多项式对应的系数构造相应的 operand polynomial  $l(x)$ 
 - 创造随机 secret   $\alpha , \ \ s$ 
 - 使用加密的  $l(s)$  和它的 ”α-shifted“ :  $\left(g^{l(s)}, \ \ \ g^{\alpha \ l(s)}\right)$  来设置 _proving key_
 - 设置 _verification key_：  $\left( \ g^{\alpha} \ \right)$

**Proving** : 
  - 注意这个语境下,  $l(x)$ 对应的操作数只有 $a$  一个 (Recall ① ② 两个等式) , 所以若 $a$ 赋值为 $v$ , 则第① 第② 个等式里, 都要保持 $a$  的赋值为 $v$ 
  -  $P$ 对 $l(x)$ 的操作数 $a$ 的赋值为 $v$  
	  - 将其乘以操作数多项式:  $\left(\   g^{l(s)} \ \right)^{v}$ 
	  - 乘以 `α-shifted` 后的 operand polynomial :  $\left(\   g^{\alpha \ l(s)} \ \right)^{v}$
 - 提供 operand polynomial **multiplication** Proof : $\left(\ g^{v \ l(s)} ,  \ \ \ g^{v \ \alpha \ l(s)}   \ \right)$
> 这里的 `multiplication`  就是指通过 $v$  来限制 $P$  对 operand polynomial  只能提供相同的 assignment

**Verification**
 - Parse the Proof as $\left(\  g^l , \ \ g^v  \ \right)$
 - Pairing 验证比例 $e\left(g^{l'},g\right) = e\left(g^l,g^\alpha \right)$

-----

<img src="http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-07-28-082623.jpg" width="80%" />

前面反复提到,  $P$  needs to respond with the same `α-shift` , and because he **cannot recover _α_ from the proving key** ,  the only way to **maintain the shift** is to multiply both encryptions by the same value :
$$
g^{l(s)} \quad and \quad  g^{\alpha \ l(s)}
$$

同样的道理, 用这种方法可以限制 $P$ 让其无法修改 $l(x)$ 的单个系数 (modify individual coefficients of $l(x)$ ) , 如果多项式为  $l(x) = ax^2 + bx+ c$ ，$P$  只可以用一个值  $v$  去 multiply 整个多项式一次： 
 
$$v \cdot (ax2 + bx+ c) = v \cdot ax2 + v  \cdot  bx+ v \cdot c$$ 
Multiplication by another polynomial is not available since `pairings`, and `α-shifts` of individual exponents of _s_ are not provided.   Prover cannot add or subtract either since:

$$\color{red}g^{\alpha(l(x)+a'x^2+c')} \not= g^{\alpha l(x)} \cdot g^{a'x^2} \cdot g^{c'}$$

> This requires the knowledge of unencrypted  $α$  , 这里也同样需要未加密的 $α$ 的知识(才能运算)

详细解释一下 上式 : 
 - 考虑 $l(x) = ax^2 + bx+ c$ ，如果 $P$ 能够任意修改多项式的系数，他可能会想要构造一个新的多项式  $l'(x) = ax^2 + a'x^2 + bx + c'$  来欺骗 $V$
 - 但是， $P$ 无法实现这样的操作，因为他不知道确切的 $\alpha$ 的值 , 最后的等式是为了展示这一限制：
	 - 左侧： $P$ 需要满足 α-shift, 通过这种方式修改 $l(x)$ 的各个系数
	 - 右侧： $Setup$ 提供了 $\left(g^{l(s)}, \ \ \ g^{\alpha \ l(s)}\right)$ , 但没有提供 $\left( g^{\alpha \ a'x^2},  \ \ g^{\alpha \ c'}  \right)$ , 所以 $P$ 无从得知  $\left( g^{\alpha \ a'x^2},  \ \ g^{\alpha \ c'}  \right)$ , 所以  $P$  只能提供可怜的 $g^{a'x^2} \cdot g^{c'}$  , 而这是无法通过 Pairing 验证的
$$
\color{red}g^{\alpha(l(x)+a'x^2+c')} \not= g^{\alpha l(x)} \cdot g^{a'x^2} \cdot g^{c'}
$$

----

现在有了这个协议，不过怎么去构造  _operand polynomial_  $l(x)$  呢？由于任何整数都可以通过乘以 1 得到它本身，所以多项式中对应的每个计算结果都应该为 1 ，即：

<img src="http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-07-28-092412.jpg" width="50%" />
> (这里的表述不清晰, 很多人没看懂, 我个人觉得类似拉格朗日基, 后面多变量会讲到: 用到了就设置为 1, 用不到就设为 0)

然后再让 prover 在其上”分配“一个值 a ：

<img src="http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-07-28-092644.jpg" width="50%" />

**Remark 4.1** :  $P$ 可以在操作数多项式上分配一个 $\color{red}{v'}$ ，而 $V$ 不能检测到 , 下面具体描述了 $P$  对多项式进行特定加（或减）操作的能力，而这种操作不会影响 $V$ 配对验证 , 因而可以修改多项式使其超出 $V$ 的预期 或 _prove a_ _different statement_，后面的章节我们将会解决掉这个问题 :

$$
\begin{aligned}
&P: \quad g^{vl(s)} \cdot g^{\color{red}{v'}} = g^{vl(s)+v'} \\
&V: \quad g^{ \color{green}{\alpha vl(s)}} \cdot (g^ {\color{green}{\alpha}} )^{\color{red}{v'}} = g^{\color{blue}{\alpha (vl(s)+v')}} \\
&V: \quad e(g^{\color{green}{\alpha(vl(s)+v'})},\ g) = e(g^{\color{blue}{vl(s)+v'}}, g^{\color{blue}{\alpha}}) \quad \leftarrow 2  \ \ are \ identically \ \  equal
\end{aligned}
$$


>  identically equal: 恒等

由于 verification key 中包含了加密了的  $α$  : $g^\alpha$ ， 所以 $P$ 可以用多项式加（或者减）任意一个值 $v'$ 而不会破坏 Pairing 的成立.  后面我们会解决掉这个 bug

##### Summary/Recap

这一小节是解决这样一个问题，算术电路中**一个 input wire 或者 output wire可能同时会作为多个门的输入 wire**，如何确保约束这些公用 wire 的问题。 

由于**要证明的数学表达式是公开的**，那么各个算式之间的约束关系也就是公开的，那么我们就可以把构造多项式的工作交给 $setup$ 环节，这样 $P$ 只要填上对应的数值就可以了。

上文这个方法就限制了在**同一个操作数**多项式上，不同的计算式中使用的同一个值的约束关系；同样若一个操作数多项式中用到了多个值，也可以将这些值全都加起来，如下文所述。


#### Multi-Variable Operand Polynomial

如上文, 因为只有当所有的左操作数使用同一个变量  $\textcolor{green}a$  的时候我们才可以设置一个值。但是如果左操作数中再多一个值  $\textcolor{green}d$  要怎么做呢 ?

$$
\begin{aligned}
&{\color{green}a} \times {\color{blue}b} = {\color{red}r_1} \\
&{\color{green}a} \times {\color{blue}c} = {\color{red}r_2} \\
&{\color{green}d} \times {\color{blue}c} = {\color{red}r_3} \\
\end{aligned}
$$

<img src="http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-07-28-093445.jpg" width="70%" />

Therefore we can separate the `operand polynomial`   $l(x)$   into 2  `operand variable polynomials` : 
$$
l_a(x) \quad and \quad l_d(x)
$$

变量 $\color{red}a$ 和 $\color{red}d$ 可以被分别 **赋值 & 约束**，然后**加在一起** , 来表示所有的**左操作数**变量。

如果 `多变量多项式` 在一个对应运算中被用做操作数，那么这一项就置为 1，否则就置为 0 , 0 跟任何值相乘结果都是零，当把他们相加在一起的时候也就可以忽略掉这一项 (类似 Lagrange Basis 的作用) 在我们的例子中, 这些变量多项式必须满足以下计算：

$$
\begin{aligned}
&l_a(1) =\textcolor{red}{1}, \ \  l_a(2)=\textcolor{red}{1}, \ \ l_a(3)=0 \\
&l_d(1)=0, \ \ l_d(2)=0, \ \ l_d(3)=\textcolor{red}{1}
\end{aligned}
$$

<img src="http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-07-28-102112.jpg" width="70%" />


于是我们就可以将每个变量分开 _assigned_ value ，然后把他们加在一起来计算出操作数多项式，例如当 $a = 3$  和  $d= 2$ 时 , 得到  $\color{red}3 \cdot l_a(x) + 2 \cdot l_d(x)$ ：

<img src="http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-07-28-102209.jpg" width="70%" />


> 上图中的  $3_a$ 代表:  用 3 实例化的变量 $a$  ( _variable  $a$  instantiated with value 3_ )

从现在起 , 我们用大写的  $\color{red}L(x)$  来表示这个复杂的操作符多项式，即

$$L(x) = a \ l_a(x) + d \ l_d(x)$$

>  $L(s)$ 仅当每一个 _operand variable polynomial_  是由  $V$  约束的 (restricted by the $V$ )，结果才有效

以 left operand 部分而言, 协议部分更改为：

**Setup:**
 - 构造 $l_a(x), \ \ l_b(x)$  - 使得它能够在对应的 `"operation x"` 处为 1，其他地方为 0
 - 选择随机数 $s,  \ \ \alpha$ 
 - 计算并加密 _未赋值的变量多项式 (unassigned variable poly)_： $\left(g^{l_a(s)}, \ \ g^{l_d(s)}\right)$
 - 计算 shifts of these polys :   $g^{\alpha \ l_a(s)}, \ \ g^{\alpha \ l_a(s)}$
 - set proving key : $\left(\  g^{l_a(s)},\ \  g^{l_d(s)},\ \ g^{αl_a(s)},\ \ g^{αl_d(s)}  \ \right)$ 
 - set _verification key_：  $\left( \ g^{\alpha} \ \right)$

**Proving** : 
 - assign values  $\textcolor{green}{a, \ d}$  to the variable polys : ${\left(g^{l_a(s)}\right)}^\textcolor{green}{a}, \ \ {\left(g^{l_d(s)}\right)}^\textcolor{green}{d}$
 - assign same values to the `α-shifted` poly  :    ${\left(g^{\textcolor{red}{\alpha} l_a(s)}\right)}^{\textcolor{green}{a}}, \ \ {\left(g^{\textcolor{red}{\alpha} l_d(s)}\right)}^{\textcolor{green}{d}}$ 
 - add all **assigned**  variable polys to form a operand poly : 
$$
g^{L(s)}=g^{\textcolor{green}{a} \ l_a(s)} \cdot g^{\textcolor{green}{d} l_d(s)} = g^{\textcolor{green}{a} l_a(s)+\textcolor{green}{d} l_d(s)}
$$
 - add **shifted assigned** variable polys to form a **shifted operand poly** : 
$$

g^{\textcolor{red}{\alpha} \ L(s)}=g^{ \textcolor{green}{a} \ \textcolor{red}{\alpha} \ l_a(s)} \cdot g^{\textcolor{green}{d} \ \textcolor{red}{\alpha} \ l_d(s)} = g^{\textcolor{red}{\alpha}(\textcolor{green}{a} l_a(s)+\textcolor{green}{d} l_d(s))}
$$
 - Proving proof of valid assignment of left operand : $\left(\ g^{L(s)}, \ \   g^{\textcolor{red}{\alpha} \ L(s)} \ \right)$

**Verification :**
 - Parse Proof  $\left(\ g^{L(s)}, \ \   g^{\textcolor{red}{\alpha} \ L(s)} \ \right)$  i.e.   $\left(\  g^L, \ \ g^{L'}  \ \right)$
 - 验证提供的多项式是否是 最初提供的多个 _未赋值的变量多项式 (unassigned variable poly)_ 的和：
$$
\begin{aligned}
e\left(g^{L'},g \right) &= e\left({g^{L}}^{},g^\alpha \right)  \quad  \quad which \ checks \ that :\\
\textcolor{red}{\alpha} \ \textcolor{green}{a} \ l_a(s) + \textcolor{red}{\alpha}\ \textcolor{green}{d} \ l_d(s) &= \textcolor{red}{\alpha} \times (\textcolor{green}{a} l_a(s) + \textcolor{green}{d}l_d(s))
\end{aligned}
$$

Note: $L(s)$  and  $\alpha L(s)$  represent all variable polynomials **at once** and since  $\alpha$  is used only in evaluation of variable polynomials, the prover has no option but to use provided evaluations and **assign same coefficients** to original and shifted variable polynomials.
 ( 注意：这里用 $L(s)$  和  $\alpha L(s)$ 同时代表了所有的变量多项式,  并且由于 $\alpha$ 只用在计算变量多项式中，所以 $P$ 没有别的选择只能在  $Setup$  提供的原始加密值和变换后的加密值上赋予相同的系数做计算 )

As a consequence(因此) the **prover** :
- is not able to modify provided _variable polynomials_ by changing their coefficients, except “assigning” values (除了“分配”值外，不能再修改它们的系数进而来修改 _变量多项式_ ), because prover is presented only with encrypted evaluations of these polynomials, and because necessary encrypted powers of _s_ are unavailable separately with their _α_-shifts (因为 Prover 仅提供这些多项式的加密评估，也因为 s 必要次幂的加密值不能与它们的 α 变换值一起使用 )
- is not able to add another polynomial to the provided ones because the _α_-ratio will be broken  (不能通过另一个多项式相加去提供一个结果因为这样 _α_-比例关系将会被破坏掉)
- is not able to modify operand polynomials through multiplication by some other polynomial  $u(x)$ ,  which could disproportionately modify the values because encrypted multiplication is not possible in pre-pairings space     (不能通过与其他的一些多项式 $u(x)$ 相乘来修改操作数多项式，这样可能会使得修改后的值不成比例因为在预配对空间中无法进行加密乘法)


尽管 prover 被限制了多项式的使用，他还有拥有一些可允许范围内的自由度：

 - 当 prover 决定不加入一些变量多项式  $l_i(x)$  来构造操作符多项式  $L(x)$  时依然是可以接受的，因为这和为它分配值为 0 是一样的：  

$$g^{a l_a(x)} = g^{a \ l_a(x) + 0 \cdot l_b(x)}$$
 - 如果 $P$ 添加同一个 `变量多项式` 很多次也是可以接受的 , 因为这和一次分配多个值的和是一样的： 

$$g^{a l_a(x)} \cdot g^{a l_a(x)} \cdot g^{a l_a(x)} =g^{3 a  l_a(x)}$$
#### Summary/Recap 

总结一下本文证明协议的大致思路为：

1. 将要证明的程序转换为数学语言表达的形式（即加减乘除的计算）
2. 用多项式在某处的取值来进行计算以此表示数学计算，进而进行证明
3. 用多项式在多处的取值来进行计算表示多个数学运算，进而加以证明
4. 对证明的“程序”在不同计算中使用的相同的变量进行约束

当前的协议约束只解决了部分问题，还有诸多可以改进的地方，在下一节我们将对这些改进项展开讨论并给证明协议进行优化。

### **Reference :**
 - https://secbit.io/blog/2020/01/08/learn-zk-snark-from-zero-part-three/
 - https://medium.com/@imolfar/why-and-how-zk-snark-works-4-general-purpose-computation-dcdc8081ee42
 - https://medium.com/@imolfar/why-and-how-zk-snark-works-5-variable-polynomials-3b4e06859e30