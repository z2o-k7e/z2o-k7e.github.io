内积证明是一种多项式承诺方案 : 
1. Setup : 设置 Commitment 的初始参数 CRS : $\mathbb{G}, \ \mathbf{F_p}, \  \mathbf{G}, H$
	1. G 是通过 hash(Message) 生成的, 所有人都可以通过特定的消息生成这个承诺用的 Vec, 所以不需要在 $P$ 和 $V$  之间做可信设置 (两者生成的 $\mathbf{G}$ 是相同的)
2. Commit 生成一个多项式的承诺 Commitment $P$
3. Open : 输出该多像是在某个 Random Challenge 处的值 $v$
4. VerifyOpen : $P$ 能够证明 $P=Commit(a(X)) \wedge v =a(z)$
5. 

**Halo2 中的内积证明 :**

1. 承诺多项式 $\mathrm{a}(\mathrm{X})$ : 证明者发送 $P=\langle \mathbf{a}, \mathbf{G}\rangle+[r] H$ ，验证者选择随机挑战点 $\mathrm{x}$ , 证明者计算并发送 $v=a(x)$
2. 证明者需要证明 $P^{\prime}=P+[v] U,  \ \ U$ 为验证者随机选取的点
3. 令 $\mathbf{b}=\left(1, x, x^2, \ldots, x^{n-1}\right)$,  则 $\mathrm{a}(\mathrm{X})$ 多项式在 $\mathrm{x}$ 点的打开值 $v=\langle\mathbf{a}, \mathbf{b}\rangle$  ( 即 $\mathbf{a}, \mathbf{b}$ 的内积)
4. 证明过程持续 $\log (\mathrm{n})$ 轮, 每一轮中, 证明者计算并发送
$$
\begin{aligned}
& L_j=\left\langle\mathbf{a}_{{lo}}^{\prime}, \mathbf{G}_{{ }_{h i}}^{\prime}\right\rangle+\left[l_j\right] H+\left[\left\langle\mathbf{a}^{\prime}{ }_{{ }_{\mathrm{lo}}}, \mathbf{b}_{{ }_{\mathrm{hi}}}^{\prime}\right\rangle\right] U \\
& R_j=\left\langle\mathbf{a}^{\prime}{ }_{\mathrm{hi}}, \mathbf{G}^{\prime}{ }_{\mathrm{lo}}\right\rangle+\left[r_j\right] H+\left[\left\langle\mathbf{a}^{\prime}{ }_{\mathrm{hi}}, \mathbf{b}^{\prime}{ }_{\mathrm{lo}}\right\rangle\right] U \\
&
\end{aligned}
$$
5. 验证者每轮发送随机挑战 $u_j$ ,  证明者根据挑战计算新的 $a^{\prime}, b^{\prime}$ 和 $G^{\prime}$, 最后一轮后证明者发送 $a, r^{\prime}$
$$
\begin{aligned}
&\mathbf{a}^{\prime} \leftarrow \mathbf{a}_{\mathrm{hi}}^{\prime} \cdot u_j^{-1}+\mathbf{a}_{{ }_{\mathrm{lo}}}^{\prime} \cdot u_j \\
&\mathbf{b}^{\prime} \leftarrow \mathbf{b}_{{ }_{\mathrm{lo}}} \cdot u_j^{-1}+\mathbf{b}_{\mathrm{hi}} \cdot u_j \\
&\mathbf{G}^{\prime} \leftarrow \mathbf{G}_{{ }_{\mathrm{lo}}}^{\prime} \cdot u_j^{-1}+\mathbf{G}_{\mathrm{hi}}^{\prime} \cdot u_j
\end{aligned}
$$
最后一轮中, 验证者验证 
$$Q=\sum_{j=1}^k\left(\left[u_j^2\right] L_j\right)+P^{\prime}+\sum_{j=1}^k\left(\left[u_j^{-2}\right] R_j\right)$$
且

$$\begin{aligned}
Q & =[a] G+\left[r^{\prime}\right] H+[a b] U \\
& =[a](G+[b] U)+\left[r^{\prime}\right] H \quad \text { 其中 } r^{\prime}=\sum_{j=1}^k\left(l_j u_j^2\right)+r+\sum_{j=1}^k\left(r_j u_j^{-2}\right)
\end{aligned}$$

-----

**均摊成本(Amortization)**
$$
\begin{align}
Q&=\sum_{j=1}^k\left(\left[u_j^2\right] L_j\right)+P^{\prime}+\sum_{j=1}^k\left(\left[u_j^{-2}\right] R_j\right) \tag{1} \\
Q  &=[a] G+\left[r^{\prime}\right] H+[a b] U \\
 &=[a](G+[b] U)+\left[r^{\prime}\right] H \quad \tag{2}
\end{align}
$$
$(1)$ 式需要 $O(log(n))$ 的时间计算，$(2)$ 式计算 $G$ 需要 $O(n)$ 时间，其他项需要 $O(1)$ 时间

每增加一个证明，会增加一次 $(1)$ 式  $P_i'$  的验证时间，但右边 $G_i$ 的计算可以利用随机挑战的线性组合，令第三方进行计算，验证者只需在最后验证最新的 $G'$ 等于前面 $G_i$ 的线性组合即可

也就是说，验证者最终只需要计算一次 $G'$ ,  即: 增加一个内积证明的验证，总验证时间只边际增加了 $O(log(n))$

-----

**多点打开** 

 - 背景: 在电路的约束中，经常需要用到同一列 column 上不同位置的值
 - 为节省验证开销，需要将同个多项式在不同点的打开整合成一个
 - 更进一步，可以将不同多项式在相同的打开点集处的证明整合成一个
$$\begin{aligned}
& \operatorname{gate}_0(X)=a_0(X) \cdot a_1(X) \cdot a_2\left(X \omega^{-1}\right)-a_3(X) \\
& \operatorname{gate}_1(X)=f_0\left(X \omega^{-1}\right) \cdot a_2(X) \\
& \operatorname{gate}_2(X)=f_0(X) \cdot a_3(X) \cdot a_0(X)
\end{aligned}$$
如上, 对 $a_2$  /  $f_0$  这 2 个 column 都有 2 个打开点 : $X  \ / \  X \omega^{-1}$  

1. 根据打开电集的不同, 对多项式进行分类
2. 多每个多项式的集合, 构造一个集合多项式
3. 多每个多项式的集合, 构造一个低次多项式 $r_i(x)$ , 使得该低次多项式在该集合内所有点处和 $q_i(x)$ 的取值相同
> r_1 在 x 处的取值和 q_1 是相同的, 但是 r_1 的次数比 q_1 小很多

4. 对每个多项式的集合, 构造一个检查多项式 $f_i(x)$
> 检查 r_1 和 q_1 是不是在特定的 x 处的取值相同

5. 将检查多项式进行线性组合 :

6. 最后将检查多项式和集合多项式进行线性组合, 得到最后需要证明的多项式 : 

----

协议 :

协议描述共26步，按功能划分为4部分
1. 按打开点集的不同， 将多项式划分为不同的集合并进行承诺
2. 将 vanishing argument 中的 $h(X)$ 多项式，按协议支持的最大度数 $n$ 进行划分和承诺
3. 使用多点打开方案，构造最终的多项式 $final\_poly$
4. 使用内积证明方案，承诺/验证最终的多项式

![[Pasted image 20230824130453.png]]


![[Pasted image 20230824133914.png]]

![[Pasted image 20230824134015.png]]

![[Pasted image 20230824134411.png]]


## Commitment Scheme

和 Bulletproof 算法类似，Halo2里采用了基于Inner Product Argument的Polynomial Commitment Scheme，并且做了一些优化。关于Inner Product Argument的原理，可参考：

1. [Bulletproof--range proof 1](https://zhuanlan.zhihu.com/p/97029970)

2. [Bulletproof--range proof 2](https://zhuanlan.zhihu.com/p/97221875)

3. [Bulletproof--range proof 3](https://zhuanlan.zhihu.com/p/97676457)

简单来说，Inner Product Argument把Polynomial Commitment的交互复杂度由O(n)降低到O(log(n))。但是，verifier需要对每一个Commitment proof都要进行一个0(n)级别的操作，即计算b和G。Halo2里推出了一个新的解决方案，对于b的计算，可以利用Laurent Polynomial去解决，它是一个特殊的多项式，可以在对数时间内完成b的计算；对于G的计算，刚好可以用到前面讲到的基于Inner Product Argument的Polynomial Commitment, 但是如前面所说，每个Commitment proof都需要一个O(n)级的操作，因此Halo2里提出了一个Accumulation Scheme，即把这些0(n)级别的操作进行Accumulate，把多个验证proof的O(n)操作累加到的一个proof上，在Recursive的最后，进行一次O(n)的计算，这样，多个proof摊销了一个O(n)的计算时间，在Halo2里，定义为Nested  Amortization。