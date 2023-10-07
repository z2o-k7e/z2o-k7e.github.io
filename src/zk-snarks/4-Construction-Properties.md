> - 作者：Maksym Petkus
> - 翻译 & 注解：even@安比实验室（even@secbit.io）
> - 校对：valuka@安比实验室
> - [本系列文章](https://arxiv.org/pdf/1906.07221.pdf)已获作者中文翻译授权
> - [翻译原链接](https://secbit.io/blog/2019/12/25/learn-zk-snark-from-zero-part-one/)

[TOC]
### Construction Properties

#### Constant Coefficients  常量系数

在上文的构造中，我们通过对 _未赋值的变量多项式 (unassigned variable polynomials)_  的计算得到 0 或者 1 ，以此表示在运算中是否要用到这个变量。自然地想，我们也可以使用其它系数值，包括负数值，因为我们可以插值计算出经过任何必要的点（前提是没有两个计算使用了同一个 $x$ ）的多项式。如下是这种运算的一些例子：
$$
\begin{aligned}
&{\color{green}2a} \times {\color{blue}1b} = {\color{red}3r_1} \\
&{\color{green}-3a} \times {\color{blue}1b} = {\color{red}-2r_2}
\end{aligned}
$$

现在我们的程序就可以使用常量系数了，例如：

```bash
Algorithm 2: Constant coefficients
————————————————————————————————————————————————————————————

function calc(w, a, b)
    if w then
        return 3a × b
    else 
        return 5a × 2b
    end if
end function
```

在 $Setup$ 阶段这些系数类似于 0 或者 1 将被“硬编码”进去，之后就不能再修改了。现在我们将运算形式修改为： $\textcolor{green}{C_a \cdot a}  \times \textcolor{blue}{C_b \cdot b} =\textcolor{red}{C_r \cdot r}$

或者用更正式的参数 $V_n \in \{v_1, \ v_2 , \ v_3, \cdots v_n \}$ 表示：  
$$\textcolor{green}{C_l \cdot v_l}  \times \textcolor{blue}{C_r \cdot v_r} =\textcolor{red}{C_o \cdot v_o}$$

> $l  ，r  ，  o$  表示变量在运算中的位置 (左/右/输出)

#### Addition for Free (0 成本做加法)

看一下这个新结构，很显然在多项式的表示中，每一个不同 $x$ 所要代表的操作数都是所有 _操作数变量多项式(sum of all operand variable polynomials_ ) 的总和，其中只有一个被用到的变量是非零值而其它都为 0，下图就很好得表达了这个意思：

<img src="http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-07-28-143943.jpg" width="60%" />
我们可以利用这一个结构，加任何数量必要的 _变量_ 到每一个运算的操作符/输出中。例如在第一个运算中，我们可以首先做加法 _a+c_，然后就只需要用别的操作数与之相乘了，即  $(a + c) \times b=r$ ，可以表示为下图：
<img src="http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-08-05-061612.png" width="70%" />
因而也可以将这些变量中任意个，对它们先乘以任意的系数 , 再一并加入到一起作为单个操作数中，以此来根据相应程序的需要构造一个操作数值。这个特性实际上就允许将运算结构改为：

$$\textcolor{green}{(c_{1,a} \cdot a + c_{1,b} \cdot b + …) } \times \textcolor{blue}{(c_{r,a} \cdot a + c_{r,b}\cdot b + …)} = \textcolor{red}{(c_{o,a} \cdot a + c_{o,b} \cdot b + …)}$$
或者更正式一些用变量 $v_i \in \{ v_1, \ v_2, \ldots , \  v_n \}$  和操作数变量系数

$$
\begin{aligned}
&c_{l,i} \in \{c_{l,1}, c_{l,2}, \ \ldots \ ,c_{l,n}\},  \\
&c_{r,i} \in \{c_{r,1}, c_{r,2},\ \ldots \ ,c_{r,n}\}, \\ 
&c_{o,i} \in \{c_{o,1}, c_{o,2},\ \ldots \ ,c_{o,n}\},
\end{aligned}
$$
这个结构就是：
$$
\textcolor{green}{\sum_{i=1}^n {c_{l,i} \cdot v_i}} \times \textcolor{blue}{\sum_{i=1}^n {c_{r,i} \cdot v_i}} = \textcolor{red}{\sum_{i=1}^n {c_{o,i} \cdot v_i}}
$$
> _注意 ：每一个运算的操作数都有自己的一组系数  $c$
> 这里 乘法运算是关键，而加法运算都可以被合并到一个更大的乘法运算里面。


#### Addition, Subtraction and Division

到目前为止，我们一直专注于乘法操作。但是为了能够执行通用计算，真实环境下的程序也需要加法，加法和除法。

**加法** 前面我们确定:  可以在单个操作数的内容中将变量加起来，然后和另一个操作数相乘 —— 即  $(3a +b) \times d =r$ ，但是如果我们只是想做加法，没有乘法，例如一个程序中需要做 $(a + b)$ 的计算，我们可以按照下面的方式来表示： 
$$(a+b) \times 1 = r$$
> 为什么强行 × 1 呢?   
> 因为我们的结构中对于每一个操作数, 我们既需要常量系数也需要变量 $(c \cdot v)$  
>  `1`  这个值可以表示为 $c_1 \cdot v_1$ ，其中 $c_1 =1$  可以被“硬编码”到对应的多项式中，$v_1$  是一个变量可以给它分配任何值，那么我们就必须通过一些约束来限制 $v_1$  的值，这个在后面的章节中将会讲到

**减法** 减法与加法几乎一致，唯一的不同就是负系数， $a-b$ 也就是：
$$
\textcolor{green}{(a+-1 \cdot b)} \times \textcolor{blue}{1} = \textcolor{red}{r}
$$
**除法** 如果我们检查除法运算
$$
\frac{factor}{divisor} = result
$$
可以看到除法的结果是就是我们要得到一个结果值使其乘以 `divisor` 能够得到 `factor` 。所以我们也可以用乘法来表示出同一个意思：_divisor × result = factor_ . 这样就是说如果我们想要去证明除法运算 $a \ / \ b = r$，我们就可以把它表示为： $\textcolor{green}{b} \times \textcolor{blue}{r} = \textcolor{red}{a}$

> 运算的结构也称为 “约束” ，因为多项式结构代表的运算，并非是为了计算出结果，而是在 prover已经知晓的变量赋值的情况下，检验这个运算的过程是否正确。换句话说，即约束 prover 必须提供一致的值，无论这些值是什么。
> 
> 所有的算术计算（加减乘除）都已经有了，于是运算结构不再需要修改。


> even@安比实验室: 约束和运算有一定的关联性。算术电路的目的是为了实现「计算的验证」，而非「计算的过程」。
> 
> 上一篇文章中，我们提出了一种方法：把构造多项式的工作交给 $Setup$ 环节， $P$ 只要填上对应的数值就可以了。 这个方法不仅解决了同一个操作数运算符中不一致的问题，同时还带来了额外的便利：
> 
> 1） 允许执行计算的表达式中包含静态系数。
> 2）虽然  $l(x) \cdot r(x)=o(x)$  的关系中只有乘法，但利用这个方法也可以轻松的执行加法操作，继而也就解决了减法和除法的问题

### Example Computation

看不太懂上面说啥, 直接看例子吧 !!

有了一组通用的运算结构，我们就可以将我们原始的程序转换成一组运算，然后再转换成多项式的形式。我们先来想一下算法的数学形式（用变量  $v$  表示运算结果）：
$$
w (a b) + (1 - w) (a+b) = v \quad \quad ( \ w \in \{0, \ 1\} \ )
$$
这里有三个乘法，但是由于运算结构只支持一个乘法操作，所以这里至少就要做三次运算。我们先将它简化 : 
-      $w \times (\textcolor{green}{a \times b}) + \textcolor{blue}{ a + b } - w \times (\textcolor{green}{a + b}) = v$
- $\Rightarrow w \times (\textcolor{green}{a \times b - a - b}) = v \textcolor{blue}{-a- b}$

写出 : 
-  $\textcolor{green}{1 \cdot a }\times \textcolor{blue}{1 \cdot b}=\textcolor{red}{1 \cdot m}$
-  $\textcolor{green}{1 \cdot w }\times \textcolor{blue}{(1 \cdot m +  \  \ -1 \cdot a +  \  \ -1 \cdot b)}=\textcolor{red}{1 \cdot v +  \  \ -1 \cdot a +  \  \ -1 \cdot b}$
-  $\textcolor{green}{1 \cdot w }\times \textcolor{blue}{1 \cdot w}=\textcolor{red}{1 \cdot w}\quad \quad (restrict \ \ w \in \{0, \ 1\} \ \ \ \ \textcolor{brown}{by} \ \ \ \ w\cdot (w-1)  =0$ 

> 第 3 条是增加的约束使  $w$  必须为二进制，否则 $P$ 就可以代入任何值去执行恶意运算

现在一共有 5 个变量 ( 2 个左操作符 $\color{green}a, \ w$， 4 个右操作符 $\color{blue}a, \  b, \ m, \ w$ 和 5 个输出 $\color{red}v, \ a, \  b, \ m, \ w$ ) ,  操作符多项式为：

- $\textcolor{green}{L(x) = a \cdot l_a(x) + w \cdot l_w(x)}$
- $\textcolor{blue}{R(x) = m \cdot r_m(x) + a \cdot r_a(x) + b \cdot r_b(x)}$
- $\textcolor{red}{O(x) = m \cdot o_m(x) + v \cdot o_v(x) + a \cdot o_a(x) + b \cdot o_b(x)}$

在在三次运算中, 必须为每个*变量多项式* 都分别算出一个对应的系数, 或者如果这个多项式在计算的操作数或者输出中没有被用到的话, 系数就置为 0 : 

![](http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-07-28-152525.jpg)
如上图
- 对于左操作数 : a 在第 1 行约束出现, 所以 $L(1) = 1, \ L(2)/L(3) = 0$ ,  w 在第 2/3 行约束出现, 所以 $L(1) = 0, \ L(2)/L(3) = 1$ ,
- 对于右操作数 : **注意** 第 2 行约束的 $\color{red}r_a(2) =-1$ 

因为有三行约束, 所以 *target poly* 就是  $t(x) = (x – 1)(x –2)(x –3)$ 

Next we leverage  `polynomial interpolation` to find each  _variable polynomial_ :
 - $\textcolor{green}{l_a(x)} = \{(1,1),(2,0),(3,0)\} \  = \  \frac{1}{2}x^2 -\frac{5}{2}x +3$
 - $\textcolor{green}{l_w(x)} = \{(1,0),(2,1),(3,1)\} \  = \  -\frac{1}{2}x^2 + \frac{5}{2}x -2$
 - $\color{blue} r_m(x) =-x^2 + 4x - 3$
 - $\color{blue} r_a(x) = x^2 - 4x + 3$
 - $\color{blue} r_b(x) = \frac{3}{2}x^2 - \frac{13}{2} x + 6$
 - $\color{blue} r_w(x) = \frac{1}{2}x^2 - \frac{3}{2} x + 1$
 - $\color{red} o_m(x) = \frac{1}{2}x^2 - \frac{5}{2} x + 3$
 - ...

绘制出来就是：

![](http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-07-28-153215.jpg)


OK! Now we are ready to prove computation through polynomials.

首先，选择函数的输入值，例如： $\color{brown}w=1, \ a=3, \ b=2$ 。其次，计算过程中的中间变量值为：
-  $m=a \times b=6$
-  $v=w(m-a-b)+a+b = 6$

然后，我们把所有计算结果中的值赋值到 _变量多项式 (variable polynomial)_  中，然后相加得到操作数或者输出多项式的形式：

$$
\begin{array}{lllll}
&\textcolor{green}{L(x)}&=\textcolor{green}{3 \cdot l_a(x) +1 \cdot l_w(x)} \\
& &=3\cdot (\frac{1}{2}x^2 -\frac{5}{2}x +3) + -\frac{1}{2}x^2 + \frac{5}{2}x -2 \\
& &= \textcolor{green}{x^2 -5x+7} \\ 
&\textcolor{blue}{R(x)}&=\textcolor{blue}{6 \cdot r_m(x) + 3 \cdot r_a(x) + 2 \cdot r_b(x) + 1 \cdot r_w(x)} = \frac{1}{2}x^2 -  \frac{5}{2}x +4 \\ 
&\textcolor{red}{O(x)}&=\textcolor{red}{6 \cdot o_m(x) + 6 \cdot o_v(x) + 3 \cdot o_a(x)+ 2 \cdot o_b(x) + 1 \cdot o_w(x)}= 2 \frac{1}{2}x^2 - 12 \frac{1}{2}x +16 \\ 
& & = \frac{5}{2} x^2 - \frac{25}{2}x + 16
\end{array}
$$
> 注意:  $l_a(x)、l_w(x)$  的系数是其赋值, 比如  $l_w(x)、r_w(x)、o_w(x)$  的系数都是  `1` , $l_a(x)、r_a(x)、o_a(x)$  的系数都是  `3`   ...   $l_m(x)、r_m(x)、o_m(x)$  的系数则是 `2×3 => 6`

在图中就表示为：

<img src="http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-08-01-035020.png" width="90%" />

上图表示的,  就是 `上上图` 里的  $l_a(x)、l_w(x)$  等都被 **"拉长了"** , 拉长成了  $3 \cdot l_a(x)、6 \cdot o_m(x)$ 

把他们具体组合成 $\textcolor{green}{L(x)} 、 \textcolor{blue}{R(x)} 、 \textcolor{red}{O(x)}$  , 即相加对应操作数如 $\textcolor{green}{3 \cdot l_a(x) +1 \cdot l_w(x)}$  ...

<img src="http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-08-01-073018.png" width="70%" />

Recap 上图:   $\color{brown}w=1, \ a=3, \ b=2$ : 

 - $L(X)=\textcolor{green}{x^2 -5x+7}$  经过了  $\{(1, \ 3_a), \ (2,\ 1_w), \ (3, \ 1_w)\}$
 - $R(X)=\frac{1}{2}x^2 -  \frac{5}{2}x +4$  经过了  $\{(1, \ 2_b), \ (2,\ 6_m - 3_a -2_b), \ (3, \ 1_w)\}$
 - $O(X)$ 则 经过了  $\{(1, \ 6_m), \ (2,\ 6_v - 3_a -2_b), \ (3, \ 1_w)\}$

> $2_a$ 表示给 $a$ 这个变量赋值为 2

我们需要去证明  $L(x) \times R(x) - O(x) = t(x) h(x)$ ，因而我们先 `长除法` 找出 $h(x)$ :

$$
h(x) = \frac{L(x) \times R(x) - O(x)}{t(x)} = \frac{\frac{1}{2}x^4 - 5x^3 + \frac{35}{2}x^2 -25x +12}{(x-1)(x-2)(x-3)} = \frac{1}{2}x - 2
$$

以图的形式表示为：

<img src="http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-08-01-074425.png" width="50%" />

图示很明显, 多项式 $L(x) \times R(x) - O(x)$ 有根为 :  $x=1, \ x=2, \ x=3$ ，因而  $t(x)$  是它的因式，假如使用了和它不一致的变量值，情况就不是这样了

这就是一组能够正确计算的变量值，如何在多项式层面上证明出来的。下面 $P$ 还要再继续处理协议的密码学部分

### Verifiable Computation Protocol (可验证计算协议 )

我们基于前文中**多项式知识协议** 做了很多修改使它变得更通用 (general-purpose)，来看一下它现在的定义 

假设约定函数 `f(*)`，约定**其计算结果为证明对象(proof)**，其次数为 $d$ ，变量数为 $n$ ，其对应的系数: 
$$
\{c_{L,i,j}, \ c_{R,i,j}, \ c_{o,i,j}\}_{\ i \in \{1,\dotsc,d\},\ \  j \in \{i,\dotsc,d\}}
$$
**Setup :** 
-  为左操作数 $\{l_i(x)\}_{i \in \{1,…,n\}}$  (类似 $l_a(x), \ l_d(x) ,..$ ) 构造变量多项式(*variable polynomial*) 然后对于所有 $j \in \{1,…,d\}$  的运算都算出其对应的系数，即  $l_i(j) = c_{L,i,j}$ , 对右操作数和输出也做同样的事情。$类似 \ \  r_m(2)=-1; \ \ r_a(2)=1$  这样的
- 随机抽取 $s, \ \alpha$
- 计算  $t(x)= (x-1)(x-2)…(x-d)$   及   $g^{t(s)}$ 
- 计算 _proving key_：
    - $\left(\ \{g^{s^k}\}_{k \in[d]}, \quad \{g^{l_i(s)}, g^{r_i(s)}, g^{o_i(s)}, \ \ g^{\textcolor{red}{\alpha}l_i(s)}, g^{\textcolor{red}{\alpha}r_i(s)},g^{\textcolor{red}{\alpha}o_i(s)}\}_{i \in \{1,……,n\}}\right)$
- 计算 _verification key_ ： $\left(g^{t(s)}, \ g^{\alpha}\right)$ 

**Proving :**
- compute function `f(*)` and therefore corresponding variables values $\{v_i\}_{i \in \{1,…,n\}}$
    - 我理解这里就是 $n$ 个变量 每个变量的赋值即 witness
- 计算 $h(x) = \frac{L(x) \times R(x) - O(x)}{t(x)}$ ，其中 $\textcolor{green}{L(x) = \sum_{i=1}^n {v_i \cdot l_i(x)}}$  ，  $\textcolor{blue}{R(x)}、\textcolor{red}{O(x)}$ 也是类似处理 
- 给 $n$ 个变量赋值 $v_1,\ldots,v_n$  求和, 得到 *operand poly* ：
$$
\begin{align}
g^{L(s)} &= \left(g^{l_1(s)}\right)^{v_1} \cdot \ \ldots \  \cdot \left(g^{l_n(s)}\right)^{v_n}, \\
g^{R(s)} &= \prod_{i=1}^n{\left(g^{r_i(s)}\right)^{v_i}}, \\ 
g^{O(s)} &= \prod_{i=1}^n{\left(g^{o_i(s)}\right)^{v_i}} \\
\end{align}
$$
- assign variable values to the **shifted** poly :
$$
\begin{aligned}
&g^{\alpha L(s)} = \prod_{i=1}^n{\left(g^{\textcolor{red}{\alpha} l_i(s)}\right)^{v_i}}, \\ 
&g^{\alpha R(s)} = \prod_{i=1}^n{\left(g^{\alpha r_i(s)}\right)^{v_i}}, \\
&g^{\alpha O(s)} = \prod_{i=1}^n{\left(g^{\alpha o_i(s)}\right)^{v_i}}
\end{aligned}
$$
- 使用 $s$ 的幂加密值： $\{g^{s^k}\}_{k \in [d]}$   计算加密值  $g^{h(s)}$  给 $V$
- set proof  
$$\{g^{L(s)},g^{R(s)},g^{O(s)}, \ \ g^{\textcolor{red}{\alpha}L(s)},g^{\textcolor{red}{\alpha}R(s)},g^{\textcolor{red}{\alpha}O(s)}, \ \ \textcolor{blue}{g^{h(s)}}\}$$

**Verification :** 
- parse proof as  $\left(g^L,g^R,g^O,g^{L'},g^{R'},g^{O'},g^{h}\right)$
- `variable polynomial`  restriction check (要符合 `α-shifted` )
$$
\begin{align}
e(g^L,g^\alpha) &= e(g^{L'},g), \\ 
e(g^R,g^\alpha) &= e(g^{R'},g), \\ 
e(g^O,g^\alpha) &= e(g^{O'},g)
\end{align}
$$
- valid operation check (计算结果的有效性)
$$
\begin{align}
e(g^L,g^R) &\overset{?}{=} e(g^t,g^h) \cdot e(g^O,g) \\
{e(g,\ g)}^{L + R} &\overset{?}{=} g^{t\cdot h + O} \\
{e(g,\ g)}^{L + R} &\overset{?}{=} {e(g,\ g)}^{t\cdot h + O}
\end{align}
$$

The set of all the variable polynomials  $\{ l_i(x), \ r_i(x), o_i(x) \}$  for  $i \in \{1,\ \cdots, \ n \}$   and the target polynomial  $t(x)$  is called a  _quadratic arithmetic program_ (QAP, introduced in [Gen+12](https://medium.com/@imolfar/why-and-how-zk-snark-works-6-verifiable-computation-protocol-1aa19f95a5cc#8bfc) ).

虽然协议足够健壮，可以进行常规的计算验证，但这里依然还有两个安全考虑需要去解决。

### 2 security considerations

#### 1 | Non-Interchangeability of Operands and Output

**操作数和输出的不可替代性**

Because we use **the same**  $\textcolor{red}\alpha$  for all `operands` of _variable polynomials restriction check_ , there is nothing that prevents $P$  from  

- 使用其它的操作数中的可变多项式，即  $L'(s) =\textcolor{red}{o_1(s)} +r_1(s) + r_1(s) + \cdots$ 
- 完全交换操作数多项式， 也就是把 $O(s)$ 和 $L(s)$ 换成  $\textcolor{red}{O(s) × R(s) = L(s)}$
- 复用相同的操作数多项式，即 $\textcolor{red}{L(s) × L(s)} = O(s)$ 

可交换性就是指 $P$ 可以修改计算过程，并有效证明一些其它无关的计算结果。防止这种行为的一个很显然的方式就是在不同的操作数上使用不同的 $\alpha s$  ，具体协议就可以修改为：

选择随机数 $\alpha_l, \ \alpha_r, \ \alpha_o$  来代替  $\alpha$   

**Setup** :
 - sample random  $\textcolor{red}{\alpha_l, \ \alpha_r, \ \alpha_o}$  instead of   $\textcolor{red}{\alpha}$   
 - calculate corresponding `shifts` 
$$\{g^{\textcolor{pink}{\alpha_l} l_i(s)},g^{\textcolor{red}{\alpha_r}r_i(s)},g^{\textcolor{brown}{\alpha_o}o_i(s)}\}_{ \ i \in \{1 \ldots n\}}$$
 - Proving key : $\left(\ g^{t(s)},g^{\textcolor{pink}{\alpha_l}},g^{\textcolor{red}{\alpha_r}},g^{\textcolor{brown}{\alpha_o}}  \ \right)$
**Proving** :
 - ...
 - assign variables to the **shifted** poly :
$$
g^{\textcolor{pink}{\alpha_l} L(s)} = \prod_{i=1}^n{(g^{\textcolor{pink}{\alpha_l} l_i(s)})^{v_i}},  \quad g^{\textcolor{red}{\alpha_r} R(s)} = \prod_{i=1}^n{(g^{\textcolor{red}{\alpha_r} r_i(s)})^{v_i}}, \quad g^{\textcolor{brown}{\alpha_o} O(s)} = \prod_{i=1}^n{(g^{\textcolor{brown}{\alpha_o} o_i(s)})^{v_i}}
$$
It is now not possible to use *variable polynomials* from other operands since following  `α-s` are not known to the prover: 
 (这样就不能在一个操作数中使用其它操作数(operands) 的变量多项式了，因为 `prover` 没有办法去获知  $\alpha_l, \ \alpha_r, \ \alpha_o$   来满足  $\alpha_s$  变换关系 )

> even@安比实验室: 这里通过对  $l(x),r(x)$ 和  $o(x)$  进行分开 KEA 检查，就解决了上篇文章中提出的第二个缺陷问题——由于 prover 生成的证明中只有计算结果，左操作数，右操作数，输出在计算中混用也不会被发现。
> 
> 同样下面一节也解决了上篇文章中提出的第三个缺陷问题——由于左操作数，右操作数，输出是分开表示的，互相之间的关系无法进行约束

####  2 | Variable Consistency Across Operands(一致性校验和)

**跨操作数的变量一致性**

For any variable $\textcolor{green}{v_i}$ we have to _assign_ its value to a _variable polynomial_ for each corresponding operand, i.e.:
 ( 对于任意的变量 $\textcolor{green}{v_i}$ ，我们都必须将它的值 _分配_ 到每个相应操作数中的一个与之对应的 _变量多项式_ 上，即：)
$$
(g^{l_i(s)})^{v_i}, (g^{r_i(s)})^{v_i},(g^{o_i(s)})^{v_i}
$$
Because the validity of each of the _operand polynomials_ is checked separately, no enforcement requires to use same variable values in the corresponding _variable_ _polynomials_. This means that the value of variable  $\textcolor{green}{v_i}$  in left operand can differ from variable $\textcolor{green}{v_i}$  in the right operand or the output.
 ( 因为每一个 _operand polynomials_  的有效性是分开校验的，并不强制要求我们在对应的 _variable_ _polynomials_ 中使用相同的变量值。这就意味着在左操作数中变量 $\textcolor{green}{v_i}$ 的值可以与右操作数或输出中的变量值  $v_1$ 不同)

我们可以通过熟悉的限制多项式的方法（也就是限制变量多项式的方法）在操作数之间**强制变量值相等**。

If we can create a “shifted checksum” variable polynomial across all operands, that would restrain prover such that he can assign only same value. A verifier can combine polynomials for each variable into one, e.g.,
 ( 如果我们能够在所有的操作数之间创造一个作为“变换的校验和”(**shifted checksum**) 的变量多项式(variable polynomial)，(这里我理解就是创建一个包含了**所有 variable** 的 variable poly, 对这些所有的 variable 整体做 α-shift, 就一个都别跑都被约束住了)  那么就可以**限制 $P$ 使其只能够赋予(给每个变量)相同的值**。 $V$ 可以将这些每个变量的多项式加起来，即：
$$
shifted \ \ checksum  \ \ variable \ \ poly : \ \quad g^{l_i(s)+r_i(s)+o_i(s)}
$$
然后乘以一个额外的随机数 _β_ ，即
$$
g^{\textcolor{green}{\beta} \  (l_i(s)+r_i(s)+o_i(s))}
$$
提供这些 `β-shifted poly` 给 $P$ ，与变量多项式一起给它赋上变量值：
$$
(g^{l_i(s)})^{v_{L,i}},(g^{r_i(s)})^{v_{R,i}}, (g^{o_i(s)})^{v_{O,i}}, (g^{β(l_i(s)+r_i(s)+o_i(s))})^{v_{β,i}}
$$
然后加密 _β_ 并把 $g^{\beta}$ 加到 _verification key_ 中。现在如果所有的 $v_i$ 值相同，即, 
$$
v_{L,i} = v_{R,i} = v_{o,i} = v_{β,i} \quad 其中 \ i \in \{1,…,n\}
$$
等式就满足：
$$
\begin{align}
e(g^{v_{L,i} \cdot l_i(s)} \cdot g^{v_{R,i} \cdot r_i(s)} \cdot g^{v_{O,i} \cdot o_i(s)}, \ \ \ g^{\beta}) &= e(g^{v_{\beta,i} \ \cdot \  \textcolor{green}{\beta} \ \left(l_i(s)+r_i(s)+o_i(s)\right)},\ g) \tag{1} \\ 
{\textcolor{green}{\beta} \cdot \left( v_{L,i} \cdot l_i(s) \ + \  v_{R,i}  \cdot r_i(s)  \ + \  v_{O,i} \cdot o_i(s) \right)} &= v_{\beta,i}  \cdot  \textcolor{green}{\beta} \ \left(l_i(s)+r_i(s)+o_i(s) \right) \tag{2} 
\end{align}  
$$
> (2) 式的底 $e(g, g)$ 被暂时忽略省去了

如上,  $(1),(2)$ 式成立的条件是 : 当且仅当 $v_{L,i}、v_{R,i}、v_{O,i} \ 和 \  v_{\beta,i}$  **都相等**时，这个等式才会成立

尽管这个一致性校验很有用，但还是存在一定的概率  $l(s),\ r(s),\ o(s)$ 中至少有两项要么计算值相同, 要么一个多项式可以被另一个整除等情况，这就允许 $P$ 去分解 $v_{L,i},v_{R,i},v_{O,i},v_{β,i}$  这些值的关系，使得即使有至少两个不相等的值也依然能够保持等式成立，从而使上 $(2)$ 式的校验无效

例如，一个以  $l(x) = r(x)$  为例的单个运算。我们用 _w_ 来表示这 2 式的评估, 同时令 $o(s) = y$  。这个等式看起来就是：
$$
\begin{array}{lllll}
&\beta(v_L \textcolor{green}{w} + v_R \textcolor{green}{w}+v_O \textcolor{red}{y}) &\stackrel{?}{=} v_\beta \cdot \beta(\textcolor{green}{w+w}+ \textcolor{red}{y}) \\ 
\end{array} 
$$
  
对于任意的 $v_R$  和 $v_O$  ，这种形式可以令 $\color{red}v_\beta = v_o, \ \ v_L = 2v_o -v_R$ ，上式也就变换成：
$$
\begin{array}{lllll}
&\beta(2\textcolor{green}{v_o}w - v_Rw + v_Rw + \textcolor{green}{v_o} y) &\stackrel{?}{=} \textcolor{green}{v_o} \cdot \beta(2w+y) \\
&\beta(2\textcolor{green}{v_o}w + \textcolor{green}{v_o}y)  &\stackrel{?}{=}\textcolor{green}{v_o} \cdot \beta(2w+y) \\
&\textcolor{green}{v_o} \cdot \beta(2w+y) &=\textcolor{green}{v_o} \cdot \beta(2w+y)\ \ \leftarrow 恒成立
\end{array}  
$$
所以说, 如果 $P$  刻意让  $\color{red}v_\beta = v_o, \ \ v_L = 2v_o -v_R$  , 则这样的一致性策略是恒成立/无效的。缓解这种情况的一种方法是对每个操作数都使用**不同的 $\beta$ ，** 确保操作数的 _变量多项式_ 中包含无法预测的值

以下是修改后的协议：

**Setup**
- ...随机数 $\beta_l, \ \beta_r, \  \beta_o$   
- 对 *variable consistency poly(变量一致性多项式)* 进行计算，加密并添加到 `proving key` 中： 
$$
\begin{equation*}
{\Big\{g^{\beta_ll_i(s)+ \beta_rr_i(s)+ β_oo_i(s)}\Big\}} _{i \in \{1,…,n\}}
\end{equation*}
$$

-  对  $\beta_S$  加密并将其加到 _verification key_ 中：$(g^{\beta_l}, \ g^{\beta_r}, \ g^{\beta_o})$ 

**Proving**
- …assign variable values to *variable consistency poly* :
$$
g^{z_i(s)} = \left(g^{β_ll_i(s)+ β_rr_i(s)+ β_oo_i(s)}\right)^{v_i} \quad for \quad i \in \{1,…,n\} \quad (别忘了\ n \ 是变量个数)
$$
- add assigned polys in encrypted space : (将赋值的多项式加`加密空间`中):
	- 对每一个变量 $v_i$ ​，都要计算它的一致性校验和(shifted-checksum)  $z_i​(s)$ ，然后我们将所有的 $z_i​(s)$ 的值相乘, 得到 $g^{Z(s)}$ 
$$
g^{Z(s)} = \prod_{i=1}^n{g^{z_i(s)}} \ \ \ = \color{red}{ \ g^{β_lL(s) + β_rR(s)+ β_oO(s)}}
$$
- add to the proof： $g^{Z(s)}$
> 在这后面增加吗? $\{g^{L(s)},g^{R(s)},g^{O(s)}, \ \ g^{\textcolor{red}{\alpha}L(s)},g^{\textcolor{red}{\alpha}R(s)},g^{\textcolor{red}{\alpha}O(s)}, \ \ \textcolor{blue}{g^{h(s)}}\}$

**Verification**
- …校验提供的 _操作数多项式_ 和 “校验和”多项式之间的一致性：
$$
\begin{array}{llllll}
e(g^L,g^{β_l}) \cdot e(g^R,g^{β_r}) \cdot e(g^O,g^{β_o}) &\stackrel{?}{=} e(g^Z,g) \\
e(g,g)^{β_lL + β_rR + β_oO} &\stackrel{?}{=} e(g,g)^Z
\end{array}
$$
PS:  Pairing 公式参考 : 
$$
e(g^a,\ g^b) \cdot e(g^c,\  g^d) = \textcolor{red}{g^{ab} \cdot g^{cd} =g^{ab + cd}} =e(g,g)^{ab + cd}
$$

这个构造中, 同一个变量值就无法乱用了，因为不同的 $β_S$ 使得相同多项式无法兼容，但是这里还存在与 **remark 4.1** 相同的缺陷，**由于 $(g^{\beta_l}, \ g^{\beta_r}, \ g^{\beta_o})$  是公开可见的**， 攻击者可以修改任意变量多项式的零索引系数(modify the zero-index coefficient of any of the variable polynomials)，因为它并不依赖于 $s$ ，i.e.
$$g^{\beta_l s^0}=g^{\beta_l}$$

### 变量非延展性和变量一致性多项式

(Non-malleability of Variable and Variable _Consistency_ Polynomials)
#### 1 | 变量多项式的延展性

> Recall 第三章的 **Remark 4.1** :  $P$ 可以在操作数多项式上分配一个 $\color{red}{v'}$ ，而 $V$ 不能检测到 , 下面具体描述了 $P$  对多项式进行特定加（或减）操作的能力，而这种操作不会影响 $V$ 配对验证 , 因而可以修改多项式使其超出 $V$ 的预期或 _prove a_ _different statement_，后面的章节我们将会解决掉这个问题 :
> $$\begin{aligned}
&P: \quad g^{vl(s)} \cdot g^{\color{red}{v'}} = g^{vl(s)+v'} \\
&V: \quad g^{ \color{green}{\alpha vl(s)}} \cdot (g^ {\color{green}{\alpha}} )^{\color{red}{v'}} = g^{\color{blue}{\alpha (vl(s)+v')}} \\
&V: \quad e(g^{\color{green}{\alpha(vl(s)+v'})},\ g) = e(g^{\color{blue}{vl(s)+v'}},g^{\color{blue}{\alpha}}) \quad \leftarrow 恒等
\end{aligned}$$
> 由于 verification key 中包含了加密了的  $α$  : $g^\alpha$ ， 所以 $P$ 可以用多项式加（或者减）任意一个值 $v'$ 而不会破坏 Pairing 的成立.  后面我们会解决掉这个 bug

举一个 **remark 4.1** 有关的例子，看一下下面的两个运算：
$$
\begin{aligned}
& \textcolor{green}{a} \times 1 =  \textcolor{red}{b} \\ 
& \textcolor{green}{3a} \times 1 =  \textcolor{red}{c}
\end{aligned}
$$
预期的结果 _b = a_ 和 _c = 3a_ , 即 _c = 3b_。这就是说 _left_ _operand’s variable_ polynomial 的计算结果为 $l_a(\textcolor{red}{1}) = \textcolor{purple}{1}$ 和 $l_a(\textcolor{blue}{2}) = \textcolor{green}{3}$   (第 $\textcolor{red}{1}$ 行约束的系数为 $\textcolor{purple}{1}$ , 第 $\textcolor{blue}{2}$ 行的约束为 $\textcolor{green}{3}$ )

先不管 $l_a(x)$ 的形式， $P$ 都可以不按照上述的比例用另一个修改了的多项式  $\color{red}{l_a}'(x)=a {l_a}(x) + 1$  来给 $a$ 赋值。这样运算就变成了 ${l_a}'(1)= a + 1$ 和 ${l_a}'(2)= 3a + 1$ ,  结果也就是 _b = a + 1_ 和 _c = 3a + 1_，其中 _c ≠ 3b_ ，这意味着 $a$ 的取值的实际意义在不同运算中是不一样的

但是因为 $P$ 已经拿到了 $g^{\alpha_l}$ 和 $g^{\beta_l}$ ，所以他依然能够正确地通过 _correct operand polynomials_ 和 _variable values consistency_ 的校验：

**…Proving：**
- 用分配不成比例的变量 _a_ 来建立左操作数多项式：  $L(x) = \textcolor{green}{a} \cdot l_a(x) +1$
- 按照常规的方式构造右操作数多项式和输出多项式： $R(x) = r_1(x),\ \  O(x)=b \cdot o_b(x) + c \cdot o_c(x)$
- 计算除数：  $h(x) = \frac{L(x) \cdot R(x) - O(x)}{t(x)}$
- 计算加密值：$g^{L(s)} = (g^{l_a(s)})^{\color{green}{a}} \cdot g^1$  ，并按照常规方式计算 $g^{R(s)},g^{O(s)}$ 
- 计算 α-shifts 的加密值： $g^{αL(s)} = (g^{αl_a(s)})^{\color{green}{a}} \cdot g^α$ ，并按照常规方式计算  $g^{αR(s)},g^{αO(s)}$
- 计算变量一致性多项式：

$$
g^{Z(s)} = \prod_{i \in \{1,a,b,c\}}{\left(g^{\beta_ll_i(s)+\beta_r r_i(s)+\beta_oO_i(s)}\right)^{\color{red}{i}} \cdot g^{\beta_l}} = g^{\beta_l(L(s)+1)+ \beta_rR(s) +\beta_oO(s)}
$$

其中下标 $i$ 代表对应变量的符号, 指数 $\textcolor{red}{i}$ 代表变量的值；以及未定义的变量多项式的值为 0。
- set proof : 
$$
(g^{L(s)},g^{R(s)},g^{O(s)},g^{α_lL(s)},g^{α_rR(s)},g^{α_oO(s)},g^{Z(s)},g^{h(s)})
$$

**Verification：**
 - variable poly restriction check : 
$$
\begin{array}{lllll}
e\left(g^{L^{\prime}}, g\right) &\stackrel{?}{=}e\left(g^L, g^\alpha\right) \Rightarrow \\ 
e\left(g^{\alpha \textcolor{green}{a} \cdot l_a(s)+\alpha}, g\right)  &\stackrel{?}{=} e\left(g^{\textcolor{green}{a} l_a(s)+1}, g^\alpha\right)
\end{array}
$$
   and as usually for $g^{R'}, \ g^{O'}$ 

 - 变量多项式约束检查：
$$
\begin{array}{lllll}
&e(g^L,g^{β_l}) \cdot e(g^R, g^{β_r}) \cdot e(g^O,g^{β_o}) &= e(g^Z,g) \\ 
&e(g,g)^{(a \cdot l_a +1)β_l +Rβ_r + Oβ_o} &= e(g,g)^{β_l(L+1)+ β_rR +β_oO}
\end{array}
$$
 - 有效计算检查：
$$
e(g^L,g^R) = e(g^t,g^h) \cdot e(g^O,g)
$$

#### 2 | Malleability of Variable Consistency Polynomials(变量一致性多项式的延展性)

Moreover, $(g^{\beta_l}, \ g^{\beta_r}, \ g^{\beta_o})$  allows to use different values of same `variable` in different `operands`. For example, if we have an operation:
(而且 $(g^{\beta_l}, \ g^{\beta_r}, \ g^{\beta_o})$  的存在允许我们在不同操作数的相同变量上使用不同的值。例如，如果我们有一个运算)：
$$
\textcolor{green}a \times \textcolor{blue}a = \textcolor{red}b 
$$

Which can be represented by the variable polynomials:
$$
\begin{aligned}
&\textcolor{green}{l_a(1) = 1}, \ \  \textcolor{blue}{r_a(1) = 1}, \ \  \textcolor{red}{o_a(1) = 0} \\
&\textcolor{green}{l_b(1) = 0}, \ \ \  \textcolor{blue}{r_b(1) = 0}, \ \  \textcolor{red}{o_b(1) = 1}
\end{aligned}
$$
简单插值, 得到
$$
\begin{aligned}
&\textcolor{green}{l_a(x) = x}, \ \  \textcolor{blue}{r_a(x) = x}, \ \  \textcolor{red}{o_a(x) = 0} \\
&\textcolor{green}{l_b(x) = 0}, \ \ \  \textcolor{blue}{r_b(x) = 0}, \ \  \textcolor{red}{o_b(x) = x}
\end{aligned}
$$
尽管我们期待的输出是 $b=a^2$ ，但我们可以设置不同的 $a$ 值，例如：设置  $a=2$ (left operand), $a=5$ (right operand) , $b=10$  如下：

**Proving：**
- …用 $a=2$ 设置**左**操作数多项式  $L(x) = 2l_a(x) + 10 \ l_b(x)$
-  用 $a=5$ 设置**右**操作数多项式  $R(x) = 2r_a(x)+3 + 10 \ r_b(x)$
	- $\because \ 2 \cdot r_a​(x)=2$ (在 $x=1$ 时) , 所以  `+3` 是为了确保我们在 $x=1$ 处得到正确的操作数 5
-  用 $b=10$ 设置输出多项式 : $O(x)=2o_a(x)+ 10o_b(x)$
- ... 计算加密值 : 
$$
\begin{aligned}
&g^{L(s)} = (g^{l_a(s)})^2 \cdot (g^{l_b(s)})^{10} = g^{2l_a(s)+10l_b(s)} \\
&g^{R(s)} = (g^{r_a(s)})^2 \cdot (g)^3 \cdot (g^{r_b(s)})^{10} = g^{2r_a(s)+3+10r_b(s)} \\ 
&g^{O(s)} = (g^{O_a(s)})^2 \cdot (g^{O_b(s)})^{10} = g^{2o_a(s)+10o_b(s)}
\end{aligned}
$$
- 计算变量一致性多项式：
$$
\begin{aligned}
g^{Z(s)} &= (g^{β_ll_a(s)+β_rr_a(s)+β_oo_a(s)})^2 \cdot  (g^{\beta_r})^{3} \cdot  (g^{\beta_l l_b(s)+ β_r r_b(s)+ β_o o_b(s)})^{10} \\ 
&= g^{β_l(2l_a(s)+10l_b(s))+ β_r(2r_a(s)+3+10r_b(s)) + β_o(2o_a(s)+3+10o_b(s))}
\end{aligned}
$$
**Verification：**
- ……变量值的一致性检查，应满足：
$$
e(g^L,g^{β_l}) \cdot e(g^R, g^{β_r}) \cdot e(g^O,g^{β_o}) = e(g^Z,g)
$$
> 注意：多项式  $o_a(x)，l_b(x)，r_b(x)$ 其实可以被忽略掉的，因为这几项对于任何 $x$ 的取值，计算结果都为 0，但是为了保持完整性我们依然要保留这几项
> 
> even@安比实验室：这种能力会危害到协议的可靠性。很显然，加密的  $\beta_s$ 不应该对 Prover 可见

#### 3 | **Non-Malleability** 非延展性

解决延展性(Malleability) 问题的一个方法就是，在 setup 阶段将 encrypted space中的 $\color{red}\beta_s$ 项与随机秘密值 $\gamma$ (gamma) 相乘,  从而使  _verification key_ 中加密的 $\color{red}\beta_s$ 与加密值 $Z(s)$ 不兼容：
$$g^{β_l \gamma}, g^{β_r\gamma}, g^{β_o\gamma}$$
相应的这种被修饰过的加密值，就能阻止修改加密值 $Z(s)$  的可行性，因为 $Z(s)$ 中没有  $\gamma$ ，即：
$$
g^{Z(s)} \cdot g^{v' \cdot \beta_l \ \textcolor{red}\gamma} = g^{β_l(L(s)+ \textcolor{red}{v' \gamma})+β_rR(s) + β_oO(s)}
$$
> 注 : $g^{Z(s)}  = g^{β_lL(s)+ β_rR(s) +β_oO(s)}$

因为变值 $\gamma$  是随机的,  $P$ 并不知道它的值。所以这个修改就需要我们用 $Z(s)$ 乘以 $\gamma$  来平衡协议中的变量值一致性校验等式：

**Setup：**
 - …随机数 $\beta_l, \ \beta_r, \  \beta_o, \  \ \gamma$
 - …设置 _verification key_：$(\cdots , g^{\beta_l \gamma}, \ g^{\beta_r \gamma}, \ g^{\beta_o \gamma}, \ g^{\gamma})$ 
**Proving：** …
**Verification：**
 - … 变量值一致性检查应满足：
$$
e(g^L,g^{β_l\gamma}) \cdot e(g^R,g^{β_r\gamma}) \cdot e(g^O,g^{β_o\gamma}) = e(g^Z, g^{\gamma})
$$
这里很重要的一点是我们排除了变量多项式为 0-阶的例子（e.g. $l_1(x) = 1 x^0$ ），否则就可以从 _proving key_ 的 _variable consistency polynomials_ (变量一致性多项式) 中揭露出加了密的 $\color{red} \beta$ 值
$$
\Big\{g^{β_ll_i(s) +β_rr_i(s) + β_oo_i(s)}\Big\}_{i \in \{1,…,n\}}
$$
比如这个例子中当操作数(Operand) / 输出(Output) 中的任意两项为 $0$ 时,  e.g.  $l_1(x) = 1, r_1(s) = 0, o_1(s) = 0$  , this will result in : 
$$
g^{\beta_l l_1(s) + \beta_r r_1(s) +\beta_oo_1(s)} = g^{\beta_l}
$$
如此 $g^{\beta_l}$  就直接被 exposed 出来了

我们同样也可以通过"修饰"(mask) α-s 项来解决 变量多项式 的延展性问题。但是这就没有必要了，因为对于 _变量多项式_ 的任何修改，都需要被映射到变量的 _一致性多项式_ 中，而一致性多项式是无法修改的

### 变量值一致性检查的优化

现在 _variable values consistency_ check 是有效的，但是这里在 _verification key_ 中增加了 4 个昂贵的 Pairing 操作和 4 个新的项。文献 [Par+13](https://secbit.io/blog/2020/01/15/learn-zk-snark-from-zero-part-four/#bd7c) 中的 Pinocchio 协议用了一个很聪明的方法优化，通过选择不同的生成元 $g$  ，从而对每个 operand 实行“移位”：

**Setup**
 - …选择随机值 $\beta, \ \ \gamma, \ \ \rho_l, \ \ \rho_r$  , and set  ${\rho}_o = \rho_l \cdot  \rho_r$
 - set generators   $\textcolor{red}{g_l=g^{\rho_l}, \ g_r=g^{\rho_r}, \ g_o=g^{\rho_o}}$
 - set  _proving key_： 
$$
\left(\big\{g^{s^k}\big\}_{k \in [d]}, \ \ \big\{g_l^{l_i(s)}, g_r^{r_i(s)}, g_o^{o_i(s)}, g_l^{α_ll_i(s)}, g_r^{α_rr_i(s)}, g_o^{α_oo_i(s)}, g_l^{βl_i(s)}, g_r^{βr_i(s)}, g_o^{βo_i(s)}\big\}_{i \in [n]}\right)
$$
 - 设置 _verification key_：
$$
\begin{array}{lllll}
\left( \ {g_o}^{t(s)},\ \ g^{\alpha_l}, \ \ g^{\alpha_r}, \ \ g^{\alpha_o}, \ \ g^{\beta \gamma}, \ \ g^\gamma \ \right)
\end{array}
$$

**Proving**
 - …assign variable values :
$$
\begin{array}{lllll}
g^{Z(s)}& = \prod_{i=1}^n{\left(g_l^{\beta \ l_i(s)} \cdot g_r^{\beta \ r_i(s)} \cdot g_o^{\beta \ o_i(s)}\right)^{v_i}}  \\
&= \left(g_l^{\beta \ L_i(s)} \cdot g_r^{\beta \ r_i(s)} \cdot g_o^{\beta \ o_i(s)}\right)
\end{array}
$$
**Verification**
 - …变量多项式约束检查：
$$e\left({\color{red}{g_l}}^{L'},g\right) = e\left({\color{red}{g_l}}^L,g^{α_l}\right)$$ 
      & 对 $g_r^{R},g_o^{O}$  做同样的检查
 - 变量值约束检查：
$$
e({g_l}^L \cdot {g_r}^R \cdot {g_o}^O , \ \ g^{βγ}) = e(g^Z,g^γ)
$$
 - 有效运算检查：
$$
\begin{array}{lllll}
&e({g_l}^L , {g_r}^R) &= e({g_o}^t,g^h) \ e({g_o}^O,g) \\
&e(g,g)^{\rho_l \ \rho_r \ L \ R} &= e(g,g)^{\rho_l \ \rho_r\ 
 t \ h \ + \ \rho_l \ \rho_r \ O}
\end{array}
$$

生成元的这种随机化进一步增加了安全性，使得如 **remark 4.1** 中**描述的 _variable polynomials_ 延展性无效**。因为对于故意的修改，它必须要么是 $\rho_l , \ \ \rho_r \ 或 \  \rho_o$   原始值的倍数 , 要么就是不可直接用的加密值的倍数（假定, 如上文所述我们不去处理可能曝光加密后的值的 0 阶可变多项式）

这个优化使得 _verification key_ 减少了 2 个项,  i.e.  $g^{\beta \ \gamma}$  instead of $g^{\beta_l \gamma}, \ g^{\beta_r \gamma}, \ g^{\beta_o \gamma}$   : 
$$
对比 \ \Bigg\{
\begin{array}{lllll}
\left( \ {g_o}^{t(s)},\ \ g^{\alpha_l}, \ \ g^{\alpha_r}, \ \ g^{\alpha_o}, \ \ g^{\beta \gamma}, \ \ g^\gamma \ \right)  \\
\left(\ \cdots , g^{\beta_l \gamma}, \ g^{\beta_r \gamma}, \ g^{\beta_o \gamma}, \ g^{\gamma} \ \right)
\end{array}
$$
，并且去除了 verification 步骤中的两个配对运算 : 
$$
对比 \ \Bigg\{
\begin{array}{lllll}
e(g^L,g^{β_l\gamma}) \cdot e(g^R,g^{β_r\gamma}) \cdot e(g^O,g^{β_o\gamma}) = e(g^Z, g^{\gamma}) \\
e({g_l}^L \cdot {g_r}^R \cdot {g_o}^O , \ \ g^{βγ}) = e(g^Z,g^γ)
\end{array}
$$

> 注意：这在 Jens Groth 2016 年的 paper [Gro16](https://secbit.io/blog/2020/01/15/learn-zk-snark-from-zero-part-four/#2923)  中有更进一步的改进

> even@安比实验室: 至此，通用 zk-SNARK 协议的已经几乎构造完成了，本文可以归纳为以下几点：
> - 协议中是如何增加可变系数的和如何做加减乘除运算的
> - 协议如何保证操作数和输出的不可替代性
> - 协议如何保证跨操作数的可变一致性
> - 协议如何处理非延展性变量和变量一致性
> - 协议中变量值一致性检查优化


Reference : 
 - https://secbit.io/blog/2020/01/15/learn-zk-snark-from-zero-part-four/
 - https://medium.com/@imolfar/why-and-how-zk-snark-works-5-variable-polynomials-3b4e06859e30
 - https://medium.com/@imolfar/why-and-how-zk-snark-works-6-verifiable-computation-protocol-1aa19f95a5cc
