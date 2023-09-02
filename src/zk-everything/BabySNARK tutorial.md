<!-- toc -->

零知识证明由于其本身陡峭的入门学习曲线，往往被初学者称为moon math。为了平缓学习曲线，减轻入门压力，babysnark[1]应运而生，本文将作为babysnark原理部分的一个解读版，帮助你更好的理解snark背后的一些基本概念和直觉。在阅读本文之前，希望你已经读过[# 从零开始学习 zk-SNARK](https://secbit.io/blog/2019/12/25/learn-zk-snark-from-zero-part-one/)系列的前4部分，对包括有限域、椭圆曲线等相关知识有一个基本的了解。

### R1CS
比如我们有这样一段程序：
```python
def qeval(x):  
	y = x**3  
	return x + y + 5
```
我们知道程序执行实际上是CPU中的乘法门和加法门组合运算得到的。那么上面的程序可以看成是类似是下面的这个图，有一些输入变量和中间运算过程，最后得到输出。
<p align="center">
  <img src="https://s1.ax1x.com/2023/08/27/pPUVfHg.png" alt="alt_text">
</p>
为了更好的表示中间过程是如何执行的，我们需要将上述程序拆分写成如下形式，左侧是中间运算的输出结果，右侧可以看成中间运算的输入：

```
sym_1 = x * x  
y = sym_1 * x  
sym_2 = y + x  
~out = sym_2 + 5
```

为什么我们输入一定要写成两个变量而不能是三个或者多个变量呢？具体限制原因可以从限制运算[3]中找到答案。简单来说，多项式的算数性质有**在某一个具体的点上，左操作数和右操作数相乘等于输出结果**。而这个约束特点使得每一次输入只能是两个数的形式，如果一次有多个变量作为输入，可以分别将其拆分成两两组合。

有了这样的直觉之后我们可以来看一下R1CS（Rank 1 constraint system）的具体定义:

给定三个m行n列的矩阵 $A, B, C \in \mathbb{F}^{m \times n}$, 和一个 $\mathrm{n}$ 维向量 $s \in \mathbb{F}^n, \mathrm{R} 1 \mathrm{CS}$ 定义了一组m个方 程，每个方程的形式如下:

$$
(A \cdot s)_i \cdot(B \cdot s)_i=(C \cdot s)_i
$$

其中 $i=1,2, \ldots, m$, ·表示矩阵和向量的乘积, $(A \cdot s)_i$ 表示 $A \cdot s$ 的第 $\mathrm{i}$ 个元素。
等价地，我们可以使用Hadamard积（逐元素相乘）来表示整个系统：

$$
A \cdot s \circ B \cdot s=C \cdot s
$$

其中○表示Hadamard积。

其中A可以看作是左操作数的全局结果的矩阵表示，B可以看成是右操作数全部结果的矩阵表示。C是运算结果的全部结果的矩阵表示。接下来让我们一步一步将上述4个等式转变成矩阵的Hadamard积的形式。

假设我们将上述4个等式的输入输出变量按如下顺序排列：

```
'~one', 'x', '~out', 'sym_1', 'y', 'sym_2'
```

那么对于第一个等式

```
sym_1 = x * x
```

左操作数*a*，右操作数*b*和最后结果*c*可以分别表示成如下向量形式

```
a = [0, 1, 0, 0, 0, 0]  
b = [0, 1, 0, 0, 0, 0]  
c = [0, 0, 0, 1, 0, 0]
```

然后向量和上述6个变量相乘，就可以还原出第一个等式了。类似的，我们对等式2，3，4做同样的处理，最终可以得到矩阵A,B,C:

```
A  
[0, 1, 0, 0, 0, 0]  
[0, 0, 0, 1, 0, 0]  
[0, 1, 0, 0, 1, 0]  
[5, 0, 0, 0, 0, 1]

B  
[0, 1, 0, 0, 0, 0]  
[0, 1, 0, 0, 0, 0]  
[1, 0, 0, 0, 0, 0]  
[1, 0, 0, 0, 0, 0]

C  
[0, 0, 0, 1, 0, 0]  
[0, 0, 0, 0, 1, 0]  
[0, 0, 0, 0, 0, 1]  
[0, 0, 1, 0, 0, 0]
```

通过上述操作，我们就将一段程序转换成了R1CS的形式。

### 多项式插值
在实际的零知识证明系统中，不管具体零知识证明算法是哪种，总要有一个validator发出一个随机数作为challenge，然后prover接受这个随机数作为系统输入，然后返回一个输出结果。validator拿到输出结果看是否和挑战的随机数满足某种对应关系，如果满足就认为prover确实掌握了某种知识。为了实现validator可以找任意随机数，所以我们就有必要R1CS的约束关系转换成多项式的形式。

比如对于之前的矩阵A而言，如果竖着按列看，其实其对应的就是之前文中所说的6个变量

```
'~one', 'x', '~out', 'sym_1', 'y', 'sym_2'
```

比如说，对于one变量而言，其在上述4个等式（即4种约束关系）中所组成的向量为

```
~one: [0, 0, 0, 5]
```

如果将其在笛卡尔坐标系中表示，假设我们选取x为1，2，3，4，那么该one所组成的多项式应该经过(1,0), (2,0), (3,0), (4,5)这4个点。在笛卡尔坐标系中，我们对于做操作数和有操作数以及结果的所有x坐标只要满足一致关系，他们所组成的多项式都满足R1CS约束关系。基于上述特点，我们可以对6个变量选定一致的x坐标然后使用插值的方式得到多项式的形式。下面是我们选定x坐标是1，2，3，4得到的矩阵A的多项式表示形式：

```
A polynomials  
[-5.0, 9.166, -5.0, 0.833]  
[8.0, -11.333, 5.0, -0.666]  
[0.0, 0.0, 0.0, 0.0]  
[-6.0, 9.5, -4.0, 0.5]  
[4.0, -7.0, 3.5, -0.5]  
[-1.0, 1.833, -1.0, 0.166]
```
即one可以表示为：

$$
0.833x^3 - 5x^2 + 9.166x - 5
$$

其他变量的R1CS转换也同理。

### QAP
这种转换成的多项式新形式称之为QAP（Quadratic Arithmetic Program）我们来看一下QAP的具体定义。

**定义(QAP)**: 一个在域 $\mathbb{F}$ 上的二次算术程序 $Q$ 包含三种 $m+1$ 多项式：
- $\mathcal{V} = \{ v_i(x) \}$
- $\mathcal{W} = \{ w_i(x) \}$
- $\mathcal{Y} = \{ y_i(x) \}$
其中 $i \in \{0,1, \ldots, m\}$，以及一个目标多项式 $t(x)$。

假设 $F$ 是一个算术程序，它以 $n$ 个 $\mathbb{F}$ 的元素为输入并输出 $n'$ 个元素，总共有 $N = n + n'$ 个I/O元素。那么，当且仅当存在系数 $\{ c_{N+1}, \ldots, c_m \}$ 使得 $t(x)$ 可以整除 $p(x)$ 时， $\{ c_1, \ldots, c_N \} \in \mathbb{F}^N$ 是 $F$ 的输入和输出的有效赋值，其中：

$$
p(x):=\left(v_0(x)+\sum_{i=1}^m c_i v_i(x)\right) \cdot\left(w_0(x)+\sum_{i=1}^m c_i w_i(x)\right)-\left(y_0(x)+\sum_{i=1}^m c_i y_i(x)\right)
$$

### 布尔电路
通常情况下一般的通用snark算法使用的是QAP来去表示程序，但如果程序是一些特殊问题，比如输入程序可以表示为布尔电路，那么QAP实现就可以更加简单一点。首先我们来看一下布尔电路的特点：

<p align="center">
  <img src="https://s1.ax1x.com/2023/08/27/pPUJN0s.png" alt="alt_text">
</p>

从图中可以看到不管是哪一种的门，最终的输出结果一定是落在[0, 2]区间之内。具体来说：任何一个2输入的二进制门电路 $g(a, b) = c$，其中输入为 $a, b$，输出为 $c$，都可以使用门电路的输入和输出的仿射组合 $L = \alpha a + \beta b + \gamma c + \delta$ 来指定，当输入输出满足门电路的逻辑规范时，它只能取两个值， $L = 0$ 或 $L = 2$。这导致了一个等效的单一的“平方”约束 $(L - 1)^2 = 1$ 。

### SSP
根据上述布尔电路的特点，一般的QAP约束在布尔电路中就转换成了SSP（Square Span Program）约束。我们来看一下SSP的具体定义：

**定义(SSP)**：在域 $\mathbb{F}$ 上的一个方形跨度程序(SSP)是由 $m+1$ 个多项式 $v_0(x), \ldots, v_m(x) \in \mathbb{F}[x]$ 和一个目标多项式 $t(x)$ 组成的元组，使得对所有 $i=0, \ldots, m$，都有 $\text{deg}\left(v_i(x)\right) \leqslant \text{deg}(t(x))$。我们说方形跨度程序SSP的大小为 $m$，并且度数为 $d=\text{deg}(t(x))$。当且仅当存在 $c_{N+1}, \ldots, c_m \in\{0,1\}$，使得 $t(x)$ 能够整除 $p(x)$ 时，我们称SSP接受输入 $c_1, \ldots, c_N \in\{0,1\}$，其中：


$$
p(x):=\left(v_0(x)+\sum_{i=1}^m c_i v_i(x)\right)^2-1 .
$$

我们说SSP校验了布尔电路 $C : \{0,1\}^N \rightarrow\{0,1\}$，如果它仅接受那些满足 $C\left(c_1, \ldots c_N\right)=1$ 的输入值 $\left(c_1, \ldots, c_N\right) \in\{0,1\}^N$。

再进一步，我们可以根据SSP而具体的布尔电路构造方形约束系统(Square Constraint System)。我们首先来看一下SCS的定义：

**定义SCS**: 方形约束系统由一个矩阵 $U: \mathbb{F}^{m \times n}$ 定义。如果满足以下条件 

$$ 
(U \vec{a}) \circ(U \vec{a})=\overrightarrow{1}, 
$$

其中 $\circ$ 表示Hadamard（逐元素）乘积，那么向量 $\ddot{a}: \mathbb{F}^m$ 是此系统的解。我们也将 $(U \vec{a}) \circ(U \vec{a})$ 写为 $(U \vec{a})^2$ 。

我们可以看一个具体的例子，比如我们有3个布尔元素分别是 $a, b, c$ ：
对于布尔元素而言，比如说 $a$ 要么为 0，要么为 1。注意到

$$
(2 a-1)^2=1
$$

这意味着 $(2 a-1) \in\{-1,1\}$，从而推导出 $a \in\{0,1\}$。其他元素也是同理。对于 $c=\neg(a \wedge b)=\text{NAND}(a, b)$ 为


$$
(2 a+2 b-5 c+4)^2=1 .
$$

综合上述内容，一个包括上述导线和门的方形约束程序将采取以下形式：

$$
\left(\left[\begin{array}{ccccc}
-1 & 2 & & & \\
-1 & & 2 & & \cdots \\
-1 & & & 2 & \\
4 & 2 & 2 & -5 & \\
\vdots & & & & \ddots
\end{array}\right] \cdot\left[\begin{array}{c}
1 \\
a \\
b \\
c \\
\vdots
\end{array}\right]\right)^2=\overrightarrow{1} .
$$

### babysnark
介绍了这么多，终于到babysnark了。babysnark是对布尔电路所构造的一种snark。相比于QAP而言，SSP更简单，所以实现整个snark所需的约束也更少。具体来说一共有两个约束，第一个是SSP约束：

$$
H(·)t(·) = V(·)^2 - 1
$$

不需要做太多解释，第二个约束是线性约束：

$$
B_w(·) = YV_w(·)
$$

这个和babysnark具体设计有一些关系。 $V_w$ 的值是由prover直接计算的，而 $B_w$ 的值来自于setup阶段。设置线性约束的目的是确保 $V_w$ 确实是由同一线性多项式计算出来的，防止prover作弊，恶意构造 $V_w$ 而不是赖在setup所提供的随机challenge构造的 $V_w$ ，最终破坏SSP约束。因为prover最后输出证明的时候同时提供了 $B_w$ 和 $V_w$ 在verify阶段添加 $\gamma$ 是为了防止证明者输出特别恶意构造的 B=YV，所以再做一次线性约束。

babysnark的随机挑战$\tau$采用的是 $[1, \tau, ..., \tau^m]$的形式，该构造形式的安全保证来自q-DLOG 假设。q-DLOG 假设确保即使敌手可以在多个点上观察到多项式的值，他们也无法从多项式的结构中提取任何信息。

至此，我们对babysnark的原理部分做了详细的探讨。希望通过深入浅出的方式介绍这一简易的snark，能为你的零知识证明学习之旅提供坚实的基石。

### Reference
[1]  [BabySnark do do do](https://github.com/initc3/babySNARK/tree/master)

[2]  [quadratic-arithmetic-programs-from-zero-to-hero](https://medium.com/@VitalikButerin/quadratic-arithmetic-programs-from-zero-to-hero-f6d558cea649)

[3]  [从零开始学习 zk-SNARK（三）——从程序到多项式的构造](https://secbit.io/blog/2020/01/08/learn-zk-snark-from-zero-part-three/)

[4]  [zk-SNARKs: A Gentle Introduction](https://www.di.ens.fr/~nitulesc/files/Survey-SNARKs.pdf)
