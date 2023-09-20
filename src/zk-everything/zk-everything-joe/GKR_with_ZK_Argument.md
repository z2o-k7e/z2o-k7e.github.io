# Thanks
<br />

- 感谢SecbitLabs @郭宇 前两个月分享的Spartan Overview (尽管当时也没太理解)， 以及@even 在研究方向上的指引(据说Hyrax 不太好啃)，不至于走太多弯路。

<br />

# 我的动机

<br />

缘于folding，缘于NOVA，缘于Setty，了解到了Spartan，但并不认识它，所以才有了本篇及接下来的关于它的一切(预备知识)...... 

![image.png](https://img.learnblockchain.cn/attachments/2023/09/PIzxPgw765066e60867af.png)

关于Spartan，在ZK领域可能时间上相对也有点儿远了，暂且不考虑它在某些方面的争议，它的一些思想其实已经影响到其它比较热门的方向了，比如当下的热点Lasso & Jolt，所以它的研究意义仍然很大。

<br />

# Overview 

<br />

- 本篇文章主要参考Hyrax 论文前半部分1-4节，即优化前的GKR zk argument

<br />

- GKR 协议本身是Sumcheck协议的一种应用，不带zk argument的GKR 就可以简单认为是多个sumcheck协议的叠加，带zk argument的GKR就会带来很多的细节问题，这也是Hyrax 的起源，所以弄清楚GKR with zk argument 的各个细节后自然也就清楚了Hyrax的意义

<br />

# 数据并行化下的GKR 协议

<br />

节选自PAZK 中的图

![image.png](https://img.learnblockchain.cn/attachments/2023/09/sc70bUls65066eafdb069.png)

<br />

何为数据并行化GKR？就是同一个电路描述应用在多组input 数据中的GKR 协议，这样prover 在最开始的claims 中就不再是针对单一电路的output，比如下面的 $$V_0 = (0, 2)$$﻿：

![image.png](https://img.learnblockchain.cn/attachments/2023/09/McgHrSHb65066f148e0a6.png)

<br />

而是多个子电路的output的汇总 $$V_0​=(0,2,3,1)$$﻿：​

![image.png](https://img.learnblockchain.cn/attachments/2023/09/KOUCgkBj65066f3d45df7.png)

<br />

在GKR协议中prover 要证明也不再是:

$$
\widetilde{V}_{i - 1}(q) = \sum_{h_L \in \{0, 1\}^{b_G}} \sum_{h_R  \in \{0, 1\}^{b_G}} \widetilde{add}_i(q, h_L, h_R)(\widetilde{V}_i(h_L) + \widetilde{V}_i(h_R)) + \widetilde{mul}_i(q, h_L, h_R)(\widetilde{V}_i(h_L) \sdot \widetilde{V}_i(h_R))
$$

<br />

而是：​

$$
\begin{aligned}

\widetilde{V}_{i - 1}(q', q) &= \sum_{h' \in \{0, 1\}^{b_N} } \sum_{h_L \in \{0, 1\}^{b_G}} \sum_{h_R  \in \{0, 1\}^{b_G}} P_{q', q, i}(h', h_L, h_R) \\

\\

P_{q', q, i}(h', h_L, h_R) &= \widetilde{eq}(q', h') \sdot [\widetilde{add}_i(q, h_L, h_R)(\widetilde{V}_i(h', h_L) + \widetilde{V}_i(h', h_R)) + \widetilde{mul}_i(q, h_L, h_R)(\widetilde{V}_i(h', h_L) * \widetilde{V}_i(h', h_R))]  \\

\end{aligned}
$$

<br />

另外需要备注一下各个notion的含义：


- N﻿ 代表子电路的个数

<br />

- G﻿ 代表单个子电路中每层Gate的个数

<br />

- $V_{i - 1}(q', q)$ 代表第$i−1$﻿ 层电路编码$q' \in \mathbb{F}^{b_N}$﻿ Gate编码$q \in \mathbb{F}^{b_G}$﻿ 上的evaluation 值，$\widetilde{V}_{i - 1}(q', q)$是$V_{i - 1}(q', q)$的MLE 

<br />

- $V_{i}(h', h_L)$代表第$i$﻿ 层电路编码$h' \in \mathbb{F}^{b_N}$﻿ Gate编码 $h_L \in \mathbb{F}^{b_G}$﻿ 上的evaluation 值，$\widetilde{V}_i(h', h_L)$是$V_i(h', h_L)$的MLE；$\widetilde{V}_{i}(h', h_R)$同理

<br />

- $\widetilde{add}_i(q, h_L, h_R)$和$\widetilde{mul}_i(q, h_L, h_R)$分别代表$\{q, h_L, q_R\} \in \mathbb{F}^{b_G}$上的加法和乘法Gate的MLE，**注意Gate的描述与电路的编码$q' \in \mathbb{F}^{b_N}$﻿ 无关**，也跟input witness无关，所以它的计算可以在preprocessing 阶段就开始了，没有必要等到协议中才开始

<br />

- $eq(q', h')$代表电路编码$q' \in \mathbb{F}^{b_N}$﻿ 与 电路编码$h' \in \mathbb{F}^{b_N}$﻿ 是否一致，$\widetilde{eq}(q', h')$是$eq(q', h')$的MLE

<br />

# GKR Protocol with ZK Argument

<br />

![image.png](https://img.learnblockchain.cn/attachments/2023/09/AnDKMzNE65067106e780d.png)

仍然以为个图为例来扮演整个协议的过程。其中电路的个数$N=2$﻿，所以$b_N​=1$﻿；有限域的moduler $p=5$﻿。​

<br />

## Step ZERO

<br />

假设前半部分为public input，后半部分为witness，对witness 的每个元素进行commit，并发送给verifier ：

$$
\text{commit}(2)、\text{commit}(3) 、\text{commit}(2) 、\text{commit}(4)
$$

<br />

## Step ONE

<br />

prover 发送电路的output 作为Sumcheck的初始claims$V_0 = (0, 2, 3, 1)$，verifier 根据给定的电路第0层的evaluation 值：


$$
\def\arraystretch{1.5}

\begin{array}{c:c:c}

b_N & b_G & V_0(b_N, b_G) \\ \hline

0 & 0 & 0 \\ \hdashline

0 & 1 & 2 \\ \hdashline

1 & 0 & 3 \\ \hdashline

1 & 1 & 1 \\ \hdashline

\end{array}
$$

<br />

可以插值出相应的多项式：

$$
\begin{aligned}

s_0(x_1, x_2) &= 0 \sdot (1 - x_1)(1 - x_2) + 2 \sdot (1 - x_1) x_2 + 3 \sdot x_1(1 - x_2) + 1 \sdot x_1 x_2 \\

\end{aligned}
$$

<br />

verifier 生成challenge factor$(q', q) = (2, 4) = (x_1, x_2)$，并发送给prover，接下来进入第1层电路的 sumcheck 协议，prover 需要证明：

$$
\begin{aligned}

\widetilde{V}_0(q', q) &= \sum_{h' \in \{0, 1\}^{b_N} } \sum_{h_L \in \{0, 1\}^{b_G}} \sum_{h_R  \in \{0, 1\}^{b_G}} P_{q', q, 1}(h', h_L, h_R) \\

&= \sum_{h' \in \{0, 1\}^{b_N} } \sum_{h_L \in \{0, 1\}^{b_G}} \sum_{h_R  \in \{0, 1\}^{b_G}} \widetilde{eq}_1(q', h') \sdot [\widetilde{mul}_1(q, h_L, h_R)(\widetilde{V}_1(h', h_L) * \widetilde{V}_1(h', h_R)) + \widetilde{add}_1(q, h_L, h_R)(\widetilde{V}_1(h', h_L) + \widetilde{V}_1(h', h_R))] \\

&\overset{?}= s_0(2, 4) = \textcolor{red}{2} \\

\end{aligned}
$$

<br />

## Step TWO

<br />

将第1层的sumcheck 多项式拆解成多个item ：


$$
\begin{aligned}

\widetilde{eq}_1(q', h') &= \widetilde{eq}_1(2, y_1) \\
&= 2y_1 + (-1)(1 - y_1) \\
&= 3 y_1 - 1 \\

\\

\widetilde{mul}_1(q, h_L, h_R) &= \widetilde{mul}_1(4, (y_2, y_3), (y_4, y_5)) \\
&= 4 \sdot y_2(1 - y_3) \sdot y_4 y_5 \\

\\

\widetilde{add}_1(q, h_L, h_R) &= \widetilde{add}_1(4, (y_2, y_3), (y_4, y_5)) \\
&= (-3) \sdot (1 - y_2)(1 - y_3) \sdot (1 - y_4) y_5 \\

\\

\widetilde{V}_1(h', h_L) &= (1 - y_1) \sdot [(1 - y_2)(1 - y_3) + 4(1 - y_2)y_3 + 2 y_2(1 - y_3) + y_2 y_3] \\
&+ y_1 \sdot [4(1 - y_2)(1 - y_3) + 4(1 - y_2)y_3 + y_2(1 - y_3) + y_2 y_3]  \\

\\

\widetilde{V}_1(h', h_R) &= (1 - y_1) \sdot [(1 - y_4)(1 - y_5) + 4(1 - y_4)y_5 + 2 y_4(1 - y_5) + y_4 y_5] \\
&+ y_1 \sdot [4(1 - y_4)(1 - y_5) + 4(1 - y_4)y_5 + y_4(1 - y_5) + y_4 y_5]  \\

\end{aligned}
$$

<br />

合并item ：​


$$
\begin{aligned}

\widetilde{V}_0(q', q) &= \widetilde{V}_0(2, 4)  \\
&= \sum_{h' \in \{0, 1\}^{b_N} } \sum_{h_L \in \{0, 1\}^{b_G}} \sum_{h_R  \in \{0, 1\}^{b_G}} \widetilde{eq}_1(2, h') \sdot [\boxed{\widetilde{mul}_1(4, h_L, h_R) \sdot (\widetilde{V}_1(h', h_L) * \widetilde{V}_1(h', h_R))} + \boxed{\widetilde{add}_1(4, h_L, h_R) \sdot (\widetilde{V}_1(h', h_L) + \widetilde{V}_1(h', h_R))}] \\

&= \sum_{y_1 \in \{0, 1\}} \sum_{y_2 \in \{0, 1\}} \sum_{y_3 \in \{0, 1\}} \sum_{y_4 \in \{0, 1\}} \sum_{y_5 \in \{0, 1\}} (3 y_1 - 1) \\

&* [ \\

&\boxed{ (4 y_2(1 - y_3) y_4 y_5) }\\

&\sdot [((1 - y_1) \sdot \boxed{((1 - y_2)(1 - y_3) + 4(1 - y_2)y_3 + 2 y_2(1 - y_3) + y_2 y_3)} + y_1 \sdot \boxed{(4(1 - y_2)(1 - y_3) + 4(1 - y_2)y_3 + y_2(1 - y_3) + y_2 y_3)}) \\

&* ((1 - y_1) \sdot \boxed{((1 - y_4)(1 - y_5) + 4(1 - y_4)y_5 + 2 y_4(1 - y_5) + y_4 y_5)} + y_1 \sdot \boxed{(4(1 - y_4)(1 - y_5) + 4(1 - y_4)y_5 + y_4(1 - y_5) + y_4 y_5)})]  \\

&+ \\

& \boxed{((-3) (1 - y_2)(1 - y_3) (1 - y_4) y_5)} \\

&\sdot [((1 - y_1) \sdot \boxed{((1 - y_2)(1 - y_3) + 4(1 - y_2)y_3 + 2 y_2(1 - y_3) + y_2 y_3)} + y_1 \sdot \boxed{(4(1 - y_2)(1 - y_3) + 4(1 - y_2)y_3 + y_2(1 - y_3) + y_2 y_3)}) \\

&+ ((1 - y_1) \sdot \boxed{((1 - y_4)(1 - y_5) + 4(1 - y_4)y_5 + 2 y_4(1 - y_5) + y_4 y_5)} + y_1 \sdot \boxed{(4(1 - y_4)(1 - y_5) + 4(1 - y_4)y_5 + y_4(1 - y_5) + y_4 y_5)})] \\

] \\

\end{aligned}
$$

<br />

### Round one

<br />

prover 计算本次round 验证需要用到的proof，也就是单变量多项式$s_1(y_1)$：

$$
\def\arraystretch{1.5}

\begin{array}{c:c}

y_2 y_3 y_4 y_5 & f(y_1) \\ \hline

0001 & (3 y_1 - 1) \sdot (-3) \sdot ((1 + 3y_1) + 4) \\ \hdashline

1011 & (3y_1 - 1) \sdot 4 \sdot (2 - y_1) \\ \hdashline

\end{array}
$$

<br />

> 备注：$y_2​y_3​y_4​y_5$​﻿ 其它编码取值对应的多项式为0，就没有一一枚举出来

则：

$$
\begin{aligned}

s_1(y_1) &=  (3 y_1 - 1) \sdot (-3) \sdot ((1 + 3y_1) + 4) + (3y_1 - 1) \sdot 4 \sdot (2 - y_1) \\

&= 2 + 2 y_1 + y_1^2 \\

&= c_{0, 1} + c_{1, 1} y_1 + c_{2, 1} y_1^2 \\

\end{aligned}
$$

<br />

prover 需要把多项式$s_1(y_1)$的commitment发送给verifier，也就是把该多项式的4个系数的commitment 之后发过去：​

$$
\delta_{c_{0, 1}} = \text{commit}(c_{0, 1}) = \text{commit}(2)  \\

\delta_{c_{1, 1}} = \text{commit}(c_{1, 1}) = \text{commit}(2)\\

\delta_{c_{2, 1}} = \text{commit}(c_{2, 1}) = \text{commit}(1)\\

\delta_{c_{3, 1}} = \text{commit}(c_{3, 1}) = \text{commit}(0)\\
$$

<br />

verifier 需要验证：​

$$
s_1(0) + s_1(1) \overset{?}= s_0(2, 4) = 2
$$

<br />

根据commitment 加法同态的性质，需要验证：​

$$
2 \delta_{c_{0, 1}} + \delta_{c_{1, 1}} + \delta_{c_{2, 1}} + \delta_{c_{3, 1}} \overset{?}= \text{commit}(s_0(2, 4)) = \text{commit}(2) \textcolor{green} {\checkmark}
$$

<br />

验证通过，verfier 发送challenge factor  $r_1 = y_1 = 3$，下一个round 需要验证的目标值为：​

$$
s_1(3) =  2 + 6 + 9 = 17\mod 5 = \textcolor{red} {2}
$$

<br />

### Round two

<br />

基于$y_1 = 3$﻿ ，prover 计算本次round 验证需要用到的proof，也就是单变量多项式$s_2(y_2)$：

$$
\def\arraystretch{1.5}

\begin{array}{c:c}

y_3 y_4 y_5 & f(y_2) \\ \hline

001 & 8 \sdot -3(1 - y_2) \sdot ((10 - 11y_2) + 4) \\ \hdashline

011 & 8 \sdot 4y_2 \sdot ((10 - 11y_2) * 1) \\ \hdashline

\end{array}
$$

> 备注：$y_3​y_4​y_5$​﻿ 其它编码取值对应的多项式为0，就没有一一枚举出来

<br />

则：​

$$
\begin{aligned}

s_2(y_2) &=  8 \sdot -3(1 - y_2) \sdot ((10 - 11y_2) + 4) + 8 \sdot 4y_2 \sdot ((10 - 11y_2) * 1) \\

&= 4 + 4y_2^2 \\

&= c_{0, 2} + c_{2, 2} y_2^2 \\

\end{aligned}
$$

<br />

prover 需要把多项式$s_2(y_2)$的commitment发送给verifier，也就是把该多项式的4个系数的commitment 之后发过去：​

$$
\delta_{c_{0, 2}} = \text{commit}(c_{0, 2}) = \text{commit}(4)  \\

\delta_{c_{1, 2}} = \text{commit}(c_{1, 2}) = \text{commit}(0)\\

\delta_{c_{2, 2}} = \text{commit}(c_{2, 2}) = \text{commit}(4)\\

\delta_{c_{3, 2}} = \text{commit}(c_{3, 2}) = \text{commit}(0)\\
$$

<br />

verifier 需要验证：

$$
s_2(0) + s_2(1) \overset{?}= s_1(3) = 2
$$

<br />

根据commitment 加法同态的性质，需要验证：​

$$
2 \delta_{c_{0, 2}} + \delta_{c_{1, 2}} + \delta_{c_{2, 2}} + \delta_{c_{3, 2}} \overset{?}= \text{commit}(s_1(3)) = \text{commit}(2) \textcolor{green} {\checkmark}
$$

<br />

验证通过，verfier 发送challenge factor$r_2 = y_2 = 4$给prover，下一个round 需要验证的目标值为:

$$
s_2(4) = 4 + 64 = 68\mod 5 = \textcolor{red} {3}
$$

<br />

### Round three

<br />

基于$y_1 = 3, y_2 = 4$，prover 计算本次round 验证需要用到的proof，也就是单变量多项式$s_3(y_3)$：

$$
\def\arraystretch{1.5}

\begin{array}{c:c}

y_4 y_5 & f(y_3) \\ \hline

01 & 8 \sdot 9(1 - y_3) \sdot ((26y_3 - 34) + 4) \\ \hdashline

11 & 8 \sdot 16(1 - y_3) \sdot ((26y_3 - 34) * 1) \\ \hdashline

\end{array}
$$

> 备注：$y_4​y_5$​﻿ 其它编码取值对应的多项式为0，就没有一一枚举出来

<br />

则：


$$
\begin{aligned}

s_3(y_3) &= 8 \sdot 9(1 - y_3) \sdot ((26y_3 - 34) + 4) + 8 \sdot 16(1 - y_3) \sdot ((26y_3 - 34) * 1) \\

&= 3 + 2 y_3 \\

&= c_{0, 3} + c_{1, 3} y_3 \\

\end{aligned}
$$

<br />

prover 需要把多项式$s_3(y_3)$的commitment发送给verifier，也就是把该多项式的4个系数的commitment 之后发过去：​


$$
\delta_{c_{0, 3}} = \text{commit}(c_{0, 3}) = \text{commit}(3)  \\

\delta_{c_{1, 3}} = \text{commit}(c_{1, 3}) = \text{commit}(2)\\

\delta_{c_{2, 3}} = \text{commit}(c_{2, 3}) = \text{commit}(0)\\

\delta_{c_{3, 3}} = \text{commit}(c_{3, 3}) = \text{commit}(0)\\
$$

<br />

verifier 需要验证：​

$$
s_3(0) + s_3(1) \overset{?}= s_2(4) = 3
$$

<br />

根据commitment 加法同态的性质，需要验证：​


$$
2 \delta_{c_{0, 3}} + \delta_{c_{1, 3}} + \delta_{c_{2, 3}} + \delta_{c_{3, 3}} \overset{?}= \text{commit}(s_2(4)) = \text{commit}(3) \textcolor{green} {\checkmark}
$$

<br />

验证通过，verfier 发送challenge factor$r_3 = y_3 = 2$给prover，下一个round 需要验证的目标值为:

$$
s_3(2) = 3 + 4 = 7\mod 5 = \textcolor{red} {2}
$$

<br />

### Round four

<br />

基于$y_1 = 3, y_2 = 4, y_3 = 2$，prover 计算本次round 验证需要用到的proof，也就是单变量多项式$s_4(y_4)$：


$$
\def\arraystretch{1.5}

\begin{array}{c:c}

y_5 & f(y_4) \\ \hline

1 & 8 \sdot -16 y_4 \sdot (18 * (4 - 3y_4)) + 8 \sdot -9 (1 - y_4) \sdot (18 + (4 - 3y_4)) \\ \hdashline

\end{array}
$$

> 备注：$y_5$​﻿ 其它编码取值对应的多项式为0，就没有一一枚举出来

<br />

则：


$$
\begin{aligned}

s_4(y_4) &= 8 \sdot -16 y_4 \sdot (18 * (4 - 3y_4)) + 8 \sdot -9 (1 - y_4) \sdot (18 + (4 - 3y_4)) \\

&= 1 + 4 y_4 + y_4^2 \\

&= c_{0, 4} + c_{1, 4} y_4+ c_{2, 4} y_4^2 \\

\end{aligned}
$$

<br />

prover 需要把多项式$s_4(y_4)$的commitment发送给verifier，也就是把该多项式的4个系数的commitment 之后发过去：​

$$
\delta_{c_{0, 4}} = \text{commit}(c_{0, 4}) = \text{commit}(1)  \\

\delta_{c_{1, 4}} = \text{commit}(c_{1, 4}) = \text{commit}(4)\\

\delta_{c_{2, 4}} = \text{commit}(c_{2, 4}) = \text{commit}(1)\\

\delta_{c_{3, 4}} = \text{commit}(c_{3, 4}) = \text{commit}(0)\\
$$

<br />

verifier 需要验证：​


$$
s_4(0) + s_4(1) \overset{?}= s_3(2) = 2
$$

根据commitment 加法同态的性质，需要验证：​


$$
2 \delta_{c_{0, 4}} + \delta_{c_{1, 4}} + \delta_{c_{2, 4}} + \delta_{c_{3, 4}} \overset{?}= \text{commit}(s_3(2)) = \text{commit}(2) \textcolor{green} {\checkmark}
$$

<br />

验证通过，verfier 发送challenge factor $r_4 = y_4 = 4$给prover，下一个round 需要验证的目标值为:​

$$
s_4(4) = 1 + 16 + 16= 33\mod 5 = \textcolor{red} {3}
$$

<br />

### Round five

<br />

基于$y_1 = 3, y_2 = 4, y_3 = 2, y_4 = 4$，prover 计算本次round 验证需要用到的proof，也就是单变量多项式$s_5(y_5)$：


$$
\def\arraystretch{1.5}

\begin{array}{c:c}

- & f(y_5) \\ \hline

- & 8 \sdot -64 y_5 \sdot (18 * (26 y_5 - 34)) + 8 \sdot 27 y_5 \sdot (18 + (26 y_5 - 34)) \\ \hdashline

\end{array}
$$

<br />

则：


$$
\begin{aligned}

s_5(y_5) &= 8 \sdot -64 y_5 \sdot (18 * (26 y_5 - 34)) + 8 \sdot 27 y_5 \sdot (18 + (26 y_5 - 34)) \\

&= 3y_5 \\

&= c_{1, 5} y_5 \\

\end{aligned}
$$

<br />

prover 需要把多项式$s_5(y_5)$ 的commitment发送给verifier，也就是把该多项式的4个系数的commitment 之后发过去：​

$$
\delta_{c_{0, 5}} = \text{commit}(c_{0, 5}) = \text{commit}(0)  \\

\delta_{c_{1, 5}} = \text{commit}(c_{1, 5}) = \text{commit}(3)\\

\delta_{c_{2, 5}} = \text{commit}(c_{2, 5}) = \text{commit}(0)\\

\delta_{c_{3, 5}} = \text{commit}(c_{3, 5}) = \text{commit}(0)\\
$$

<br />

verifier 需要验证：​

$$
s_5(0) + s_5(1) \overset{?}= s_4(4) = 3
$$

<br />

根据commitment 加法同态的性质，需要验证：​

$$
2 \delta_{c_{0, 5}} + \delta_{c_{1, 5}} + \delta_{c_{2, 5}} + \delta_{c_{3, 5}} \overset{?}= \text{commit}(s_4(4)) = \text{commit}(3) \textcolor{green} {\checkmark}
$$

<br />

验证通过，verfier 发送challenge factor$r_5 = y_5 = 1 $给prover，下一个round 需要验证的目标值为:​

$$
s_5(1) = \textcolor{red} {3}
$$

<br />

### Last Round

<br />

目前challenge factor 的组合为：

$$
(3, (4, 2), (4, 1)) = (y_1, (y_2, y_3), (y_4, y_5)) = (r', r_L, r_R)
$$

<br />

prover 根据第1层电路的evaluation 值很容易就能插值出相应的MLE 多项式：​

$$
\begin{aligned}

\widetilde{V}_1(x_1, x_2, x_3) &= (1 - x_1) \sdot [(1 - x_2)(1 - x_3) + 4(1 - x_2)x_3 + 2 x_2 (1 - x_3) + x_2 x_3] \\

&+ x_1 \sdot [4(1 - x_2)(1 - x_3) + 4(1 - x_2)x_3 + x_2 (1 - x_3) + x_2 x_3]

\end{aligned}
$$

<br />

prover 分别计算出三个claims 值的commitment：​


$$
\begin{aligned}

X &= \text{commit}(\widetilde{V}_1(r', r_L)) = \text{commit}(\widetilde{V}_1(3, (4, 2))) = \text{commit}(3) \\

Y &= \text{commit}(\widetilde{V}_1(r', r_R)) = \text{commit}(\widetilde{V}_1(3, (4, 1))) = \text{commit}(2) \\

Z &= \text{commit}(\widetilde{V}_1(r', r_L) \sdot \widetilde{V}_1(r', r_R)) = \text{commit}(3 * 2) = \text{commit}(1) \\

\end{aligned}
$$

<br />

verifier 拿着这三个commitment 完成第1层电路 sumcheck 协议的最后验证：​

$$
\begin{aligned}

&\widetilde{eq}_1(2, r') \sdot [\widetilde{mul}_1(4, h_L, h_R) \sdot \text{commit}(\widetilde{V}_1(r', h_L) \sdot \widetilde{V}_1(r', h_R)) \\

&+ \widetilde{add}_1(4, h_L, h_R) \sdot (\text{commit}(\widetilde{V}_1(r', h_L)) + \text{commit}(\widetilde{V}_1(r', h_R)))] \\

\\

&= 8 \sdot [-64 * \text{commit}(1) + 27 * (\text{commit}(3) + \text{commit}(2))] \\

&\overset{?}= \text{commit}(s_5(1)) = \text{commit}(3) \textcolor{green} {\checkmark} \\

\end{aligned}
$$

<br />

### mini-protocols ​
<br />

第一层电路evaluation 对应的MLE ：​

$$
\begin{aligned}

\widetilde{V}_1(x_1, x_2, x_3) &=  (1 - x_1) \sdot [(1 - x_2)(1 - x_3) + 4 \sdot (1 - x_2) x_3 + 2 \sdot x_2 (1 - x_3) + x_2 x_3] \\

&+ x_1 \sdot [4 \sdot (1 - x_2)(1 - x_3) + 4 \sdot (1 - x_2) x_3 + x_2 (1 - x_3) + x_2 x_3] \\

\end{aligned}
$$

<br />

上一个sumcheck 协议的Last Round中prover 新增加了两个claims，也就是：​


$$
\widetilde{V}_1(r', r_L) = \widetilde{V}_1(3, (4, 2)) = 3 \\

\widetilde{V}_1(r', r_R) = \widetilde{V}_1(3, (4, 1)) = 2 \\
$$

<br />

引入一个fold factor $t$﻿ 我们可以把两个claims fold到一起：​

$$
\begin{aligned}

f_H(t) &= \widetilde{V}_1(r', (1 - t) \sdot r_L + t \sdot r_R) \\

&= \widetilde{V}_1(3, (4, 2 - t)) \\

&= 18 - 26t = 3 + 4t

\end{aligned}
$$

<br />

它的非常重要的特性就是：​

$$
f_H(0) = 3 = \widetilde{V}_1(r', r_L) \\

f_H(1) = 2 = \widetilde{V}_1(r', r_R) \\
$$

<br />

prover 把多项式$f_H(t)$进行commit后发送给verifier，同样也是多个系数分别commit，该多项式degree 为2，也就是说最多有3个commitment：​

$$
\delta_{f_0} = \text{commit}(3) \\

\delta_{f_1} = \text{commit}(4) \\

\delta_{f_2} = \text{commit}(0) \\
$$

<br />

verifier 拿到多项式$f_H(t)$的commitment 后就可以计算出：

$$
\begin{aligned}

\text{commit}(f_H(0)) &= \delta_{f_0} \\

\text{commit}(f_H(1)) &= \delta_{f_0} + \delta_{f_1} \\

\end{aligned}
$$

<br />

这样就可以验证prover 之前发送的$\widetilde{V}_1(r', r_L)、\widetilde{V}_1(r', r_R)$的commitment 是否与当前多项式的commitment **是否一致**：

$$
\begin{aligned}

\text{commit}(\widetilde{V}_1(r', r_L)) &= \text{commit}(3) \overset{?}= \text{commit}(f_H(0)) = \delta_{f_0} \textcolor{green} {\checkmark} \\

\text{commit}(\widetilde{V}_1(r', r_R)) &= \text{commit}(2) \overset{?}= \text{commit}(f_H(1)) = \delta_{f_0} + \delta_{f_1} \textcolor{green} {\checkmark} \\

\end{aligned}
$$

<br />

为了验证prover 之前发送的$\widetilde{V}_1(r', r_L)、\widetilde{V}_1(r', r_R)$的commitment X、Y﻿**是否合法**，基于多项式$f_H(t)$的commitment $\delta_{f_0} 、\delta_{f_1}、\delta_{f_2}$， verifier 随机采样一个challenge factor $v$﻿ 并发送给prover，prover 自然可以计算出下一轮sumcheck协议需要证明的evaluation值$f_H(v)$，即：

$$
\begin{aligned}

\widetilde{V}_1(q', q) &= \sum_{h' \in \{0, 1\}^{b_N} } \sum_{h_L \in \{0, 1\}^{b_G}} \sum_{h_R  \in \{0, 1\}^{b_G}} P_{q', q, 2}(h', h_L, h_R) \\

&= \sum_{h' \in \{0, 1\}^{b_N} } \sum_{h_L \in \{0, 1\}^{b_G}} \sum_{h_R  \in \{0, 1\}^{b_G}} \widetilde{eq}_2(q', h') \sdot [\widetilde{mul}_1(q, h_L, h_R)(\widetilde{V}_2(h', h_L) * \widetilde{V}_2(h', h_R)) + \widetilde{add}_2(q, h_L, h_R)(\widetilde{V}_1(h', h_L) + \widetilde{V}_2(h', h_R))] \\

& \overset{?} = f_H(v)= \textcolor{red}{3 + 4v} \\

\end{aligned}
$$

<br />

同时verifier 计算下一轮sumcheck协议需要证明的$f_H(v)$ 的commitment：

$$
\text{commit}(f_H(v)) = \delta_{f_0} + \delta_{f_1} \sdot v + \delta_{f_2} \sdot v^2
$$

<br />

最后我们再明确一点：mini-protocol 的根本目的是把两个claims fold成一个claims，减少prover 的成本，不然prover要分别证明两个claims：​

$$
\begin{aligned}

&\text{commit}(\widetilde{V}_1(3, (4, 2 - v))) \overset{?}= \delta_{f_0} + \delta_{f_1} \sdot v + \delta_{f_2} \sdot v^2 \\

&\textcolor{red}{ OR } \\

&\text{commit}(\widetilde{V}_1(3, (4, 2)) \overset{?}= \text{commit}(3) \\

&\text{commit}(\widetilde{V}_1(3, (4, 1)) \overset{?}= \text{commit}(2) \\

\end{aligned}
$$

这样应该能make sense！

<br />

## Step THREE

<br />

同Step TWO 一样，这里我们省略掉N 行文字+公式... 直接进入到Final Step！

<br />

## Final Step

<br />

我们再回顾一下最开始的实例结构图：


![image.png](https://img.learnblockchain.cn/attachments/2023/09/dsmO1vLy65067a72a0eb9.png)

根据最下面一层(public input + witness)的值，我们可以插值出MLE：


$$
\begin{aligned}

\widetilde{V}_2(x_1, x_2, x_3) &= (1 - x_1) \sdot [(1 - x_2)(1 - x_3) + 2 \sdot (1 - x_2) x_3 + x_2 (1 - x_3) + 4 \sdot x_2 x_3] \\

&+ x_1 \sdot [2 \sdot (1 - x_2)(1 - x_3) + 3 \sdot (1 - x_2) x_3 + 2 \sdot x_2 (1 - x_3) + 4 \sdot x_2 x_3] \\

\end{aligned}
$$

<br />

Step THREE 的mini-protocol 同样也会归结到证明两个claims，为了方便描述我们**假设** $(r', r_L, r_R) = (2, (3, 2), (3, 3))$：


$$
\widetilde{V}_2(r', r_L) \overset{?}= \widetilde{V}_2(2, (3, 2)) = 0 \\

\widetilde{V}_2(r', r_R) \overset{?}= \widetilde{V}_2(2, (3, 3)) = 1 \\
$$

<br />

多项式$f_H(t)$：


$$
f_H(t) = t
$$

<br />

假设fold factor $v = 2$，把上面的两个claims合并成一个claim:​

$$
\widetilde{V}_2(2, (3, 4)) \overset{?}= f_H(v) = 2
$$

> 备注：简单一句话就是，证明最下面一层(public input+witness)电路、Gate编码为(2, (3, 4))， evaluation 值为2 ，组成的**点**在MLE 多项式上。

<br />

同样，verifier 基于prover 提供的$f_H(t)$的commitment，计算出$f_H(v)$ 的commitment:

$$
\begin{aligned}

\text{commit}(f_H(2)) &= \delta_{f_0} + \delta_{f_1} \sdot 2 + \delta_{f_2} \sdot 2^2 \\

&= \text{commit}(1) \sdot 2

\end{aligned}
$$

<br />

verifier 如何验证prover 提供的这个commitment的合法性？对于verifier 来说最下面一层电路的evaluation 分 public input p﻿和 witness w﻿，其中后者未知，**假设两者长度相等**，按照上图中的实例，也就是说前半部分为public input，后半部分为witness：

$$
(\underbrace{1, 2, 1, 4}_{\text{public input}}, \underbrace{2, 3, 2, 4}_{\textcolor{red}{\text{witness}}})
$$

<br />

因此，我们需要把$\widetilde{V}_2$拆解成两部分

$$
\widetilde{V}_2(x_1, x_2, x_3) = (1 - x_1) \sdot \widetilde{p}(x_2, x_3) + x_1 \sdot \widetilde{w}(x_2, x_3)
$$

<br />

最终是要计算出$\widetilde{V}_2(2, (3, 4))$的commitment，其中public input 部分因为是公开的，所以verifier 可以自行计算出相应的MLE 多项式$\widetilde{p}(x_2, x_3)$，并拿到$\widetilde{p}(3, 4)$的commitment；另外witness 部分因为在Step ZERO prover 已经把它们的commitment 全部都已经发给verifier 了，verifier 只需要基于此拿到$\widetilde{w}(x_2, x_3)$ 的commitment就可以了：

$$
\begin{aligned}

\widetilde{w}(x_2, x_3) &= 2 \sdot (1 - x_2)(1 - x_3) + 3 \sdot (1 - x_2)x_3 + 2 \sdot x_2 (1 - x_3) + 4 \sdot x_2 x_3 \\

&\Downarrow \\

\text{commit}(\widetilde{w}(x_2, x_3) ) &= \text{commit}(2) \sdot (1 - x_2)(1 - x_3) + \text{commit}(3) \sdot (1 - x_2)x_3 + \text{commit}(2) \sdot x_2 (1 - x_3) + \text{commit}(4) \sdot x_2 x_3 \\

\end{aligned}
$$

<br />

最后的最后，我们put it together ：​

$$
\begin{aligned}

2 \sdot \text{commit}(1) &\overset{?}= (1 - 2) \sdot \text{commit}(\widetilde{p}(3, 4)) + 2 \sdot \text{commit}(\widetilde{w}(3, 4)) \\

&= 4 \sdot \text{commit}(\widetilde{p}(3, 4)) + 2 \sdot [\text{commit}(2) \sdot 1+ \text{commit}(3) \sdot 2 + \text{commit}(2) \sdot 1 + \text{commit}(4) \sdot 2]

\end{aligned}
$$

<br />

## What's Next

<br />

到此为止，满足ZK argument的Vallina 版本的GKR协议也就完整了，紧接着我们再detail一下Hyrax 在此基础之上都做了些什么？接着再看看Spark 在Hyrax基础之上做了些什么？最后再看看Spartan 的整个全貌？

<br />

## 参考资料

<br />

【1】Hyrax 论文：https\://eprint.iacr.org/2017/1132.pdf

【2】PAZK by Thaler：https\://people.cs.georgetown.edu/jthaler/ProofsArgsAndZK.pdf

【3】trivial GKR 协议：https\://learnblockchain.cn/article/6199

【4】sumcheck 协议：https\://learnblockchain.cn/article/6188
