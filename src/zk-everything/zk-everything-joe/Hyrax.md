## Reduced Sumcheck Verification

仍然以这个图为例，$N = 2$，则$b_N = 1$；第0层，$G = 2$，则$b_G = 1$；第1层，$G = 4$，则$b_G = 2$；第2层，$G = 4$，则$b_G = 2$。

<br />

### Number of Sumcheck Commitments

<br />

为了简单起见，上一章节Sumcheck 协议每次round prover 发送给verifier 的多项式系数的commitment的个数我们固定都是4，也就是说多项式的degree全为3。其实prover 需要commit的多项式的degree是有变化的。

$$
\begin{aligned}

\widetilde{V}_0(q', q) &= \sum_{h' \in \{0, 1\}^{b_N} } \sum_{h_L \in \{0, 1\}^{b_G}} \sum_{h_R  \in \{0, 1\}^{b_G}} P_{q', q, 1}(h', h_L, h_R) \\

&= \sum_{h' \in \{0, 1\}^{b_N} } \sum_{h_L \in \{0, 1\}^{b_G}} \sum_{h_R  \in \{0, 1\}^{b_G}} \widetilde{eq}_1(q', h') \sdot [\widetilde{mul}_1(q, h_L, h_R)(\widetilde{V}_1(h', h_L) * \widetilde{V}_1(h', h_R)) + \widetilde{add}_1(q, h_L, h_R)(\widetilde{V}_1(h', h_L) + \widetilde{V}_1(h', h_R))] \\

\end{aligned}
$$

<br />

当round $i <= b_N$ 时，prover commit的多项式的degree为3，也就是说commitment的个数为4：

$$
\begin{aligned}

\widetilde{V}_0(q', q) &= \sum_{h' \in \{0, 1\}^{b_N} } \sum_{h_L \in \{0, 1\}^{b_G}} \sum_{h_R  \in \{0, 1\}^{b_G}} P_{q', q, 1}(h', h_L, h_R) \\

&= \sum_{h' \in \{0, 1\}^{b_N} } \sum_{h_L \in \{0, 1\}^{b_G}} \sum_{h_R  \in \{0, 1\}^{b_G}} \textcolor{red}{\widetilde{eq}_1(q', h')} \sdot [\widetilde{mul}_1(q, h_L, h_R)(\textcolor{red}{ \widetilde{V}_1(h', h_L) }* \textcolor{red}{ \widetilde{V}_1(h', h_R)) } + \widetilde{add}_1(q, h_L, h_R)(\widetilde{V}_1(h', h_L) + \widetilde{V}_1(h', h_R))] \\

\end{aligned}
$$

<br />

当round $i > b_G$ 时，prover commit的多项式的degree为2， 也就是说commitment的个数为3:

$$
\begin{aligned}

\widetilde{V}_0(q', q) &= \sum_{h' \in \{0, 1\}^{b_N} } \sum_{h_L \in \{0, 1\}^{b_G}} \sum_{h_R  \in \{0, 1\}^{b_G}} P_{q', q, 1}(h', h_L, h_R) \\

&= \sum_{h' \in \{0, 1\}^{b_N} } \sum_{h_L \in \{0, 1\}^{b_G}} \sum_{h_R  \in \{0, 1\}^{b_G}} \widetilde{eq}_1(q', h') \sdot [\widetilde{mul}_1(q, h_L, h_R)(\textcolor{red}{ \widetilde{V}_1(h', h_L) }* \textcolor{red}{ \widetilde{V}_1(h', h_R)) } + \widetilde{add}_1(q, h_L, h_R)(\widetilde{V}_1(h', h_L) + \widetilde{V}_1(h', h_R))] \\

\end{aligned}
$$

<br />

### Sumcheck Verifications

<br />

我们试图把verifier sumcheck 协议中所有round的校验等式且一个矩阵点乘运算表示：

$$
\bold{M} \sdot \textcolor{red} {\bold{\pi}} \overset{?}= \textcolor{red}{\bold{Q}}
$$

<br />

其中每个round prover发送的message 为：

$$
\begin{aligned}

\pi_1 &= (c_{0,1}, c_{1,1}, c_{2,1}, c_{3,1}) \\

\pi_2 &= (c_{0, 2}, c_{1, 2}, c_{2,2}) \\

\pi_3 &= (c_{0, 3}, c_{1, 3}, c_{2,3}) \\

\pi_4 &= (c_{0, 4}, c_{1, 4}, c_{2,4}) \\

\pi_5 &= (c_{0, 5}, c_{1, 5}, c_{2,5}) \\

\pi_{\text{last}} &= (V_1(r', r_L), V_1(r', r_R), V_1(r', r_L) \sdot V_1(r', r_R)) \\

\end{aligned}
$$

<br />

把它们聚合到一个向量里：

$$
\textcolor{red} {\pi} = 

\begin{bmatrix}

&c_{0, 1} \\
&c_{1, 1} \\
&c_{2, 1} \\
&c_{3, 1} \\
&c_{0, 2} \\
&c_{1, 2} \\
&c_{2, 2} \\
&c_{0, 3} \\
&c_{1, 3} \\
&c_{2, 3} \\
&c_{0, 4} \\
&c_{1, 4} \\
&c_{2, 4} \\
&c_{0, 5} \\
&c_{1, 5} \\
&c_{2, 5} \\
&V_1(r', r_L) \\
&V_1(r', r_R) \\
&V_1(r', r_L) \sdot V_1(r', r_R) \\

\end{bmatrix}
$$

<br />

其中每个round verifier 需要验证时用的参数：

$$
\begin{aligned}

M_1 &= (\boxed{2, 1, 1, 1}, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0) \\

\\

M_2 &= (\boxed{-1, -r_1, -r_1^2, -r_1^3, 2, 1, 1}, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0) \\

\\

M_3 &= (0, 0, 0, 0, \boxed{-1, -r_2, -r_2^2, 2, 1, 1}, 0, 0, 0, 0, 0, 0, 0, 0, 0) \\

\\

M_4 &= (0, 0, 0, 0, 0, 0, 0, \boxed{ -1, -r_3, -r_3^2, 2, 1, 1}, 0, 0, 0, 0, 0, 0) \\

\\

M_5 &= (0, 0, 0, 0, 0, 0, 0,  0, 0, 0, \boxed{-1, -r_4, -r_4^2, 2, 1, 1}, 0, 0, 0) \\

\\

M_{\text{last}} &= (0, 0, 0, 0, 0, 0, 0,  0, 0, 0, 0, 0, 0, \boxed{-1, -r_5, -r_5^2, \widetilde{eq}(2, r') \sdot \widetilde{add}(4, r_L, r_R), \widetilde{eq}(2, r') \sdot \widetilde{add}(4, r_L, r_R), \widetilde{eq}(2, r') \sdot \widetilde{mul}(4, r_L, r_R)}) \\

\end{aligned}
$$

把它们聚合到一个矩阵里：

$$
M = 

\begin{bmatrix}

M_1 \\

M_2 \\

M_3 \\

M_4 \\

M_5 \\

M_{\text{last}} \\

\end{bmatrix}
$$

<br />

每一个round verifier 校验的结果：

$$
\textcolor{red} {Q} = 

\begin{bmatrix}

s_0 \\

0 \\

0 \\

0 \\

0 \\

0 \\

\end{bmatrix}
$$

> 备注：其中，$s_0$ 是第1个round 需要校验的sumcheck 值，是verifier 随机采样的第0层电路编码的evaluation值，也是prover 第1个round要证明的值。

<br />

汇总一下就是：

$$
\begin{aligned}

M \sdot \textcolor{red}{\pi} = 

\begin{bmatrix}

M_1 \\

M_2 \\

M_3 \\

M_4 \\

M_5 \\

M_{\text{last}} \\

\end{bmatrix} 

\sdot 

\begin{bmatrix}

c_{0, 1} \\
c_{1, 1} \\
c_{2, 1} \\
c_{3, 1} \\
c_{0, 2} \\
c_{1, 2} \\
c_{2, 2} \\
c_{0, 3} \\
c_{1, 3} \\
c_{2, 3} \\
c_{0, 4} \\
c_{1, 4} \\
c_{2, 4} \\
c_{0, 5} \\
c_{1, 5} \\
c_{2, 5} \\
V_1(r', r_L) \\
V_1(r', r_R) \\
V_1(r', r_L) \sdot V_1(r', r_R) \\

\end{bmatrix}

\overset{?}= 

\begin{bmatrix}

s_0 \\

0 \\

0 \\

0 \\

0 \\

0 \\

\end{bmatrix}

\end{aligned}
$$

<br />

矩阵$M$ 需要verifier 自行计算，用红色标记的向量$\pi$ 和 向量$Q$ 都是需要prover 进行commit。 如果说仍然是一个field对应一个commitment,那么commit之后的校验就变成了:

$$
\begin{aligned}

M \sdot \delta = 

\begin{bmatrix}

M_1 \\

M_2 \\

M_3 \\

M_4 \\

M_5 \\

M_{\text{last}} \\

\end{bmatrix} 

\sdot 

\begin{bmatrix}

\delta_{0, 1} \\
\delta_{1, 1} \\
\delta_{2, 1} \\
\delta_{3, 1} \\
\delta_{0, 2} \\
\delta_{1, 2} \\
\delta_{2, 2} \\
\delta_{0, 3} \\
\delta_{1, 3} \\
\delta_{2, 3} \\
\delta_{0, 4} \\
\delta_{1, 4} \\
\delta_{2, 4} \\
\delta_{0, 5} \\
\delta_{1, 5} \\
\delta_{2, 5} \\
X \\
Y \\
Z \\

\end{bmatrix}

\overset{?}= 

\begin{bmatrix}

C_0 \\

0 \\

0 \\

0 \\

0 \\

0 \\

\end{bmatrix}

\end{aligned}
$$

有没有觉得向量$\pi$ size太大了(19个commitment)？是的，它直接影响着协议过程中的communication cost，所以需要进行压缩处理。


<br />

### Reducing Sumcheck Commitments


一个field 对应一个commitment:
$$

c_{f_i} = [f_i]_g + [r_{f_i}]_h

$$
每次commit的时候还需要一个blind factor $r_{f_i}$.

<br />


这样的话Sumcheck 协议中的commitment个数就会与要commit的多项式的degree成线性关系。如果把一个多项式所有参数的commitment压缩成一个commitment：
$$

c_f = \sum_{i = 1}^n [f_i]_{g_i} + [r_f]_h

$$

这样的话就需要多个generator $g_i$ 了，但blind factor 变成了一个$r_f$。

<br />

我们用矩阵第一行的校验为例：
$$

\begin{aligned}

\lang M_1, \delta_{, 1} \rang = 2 \sdot \delta_{0, 1} + \delta_{1, 1} + \delta_{2, 1} + \delta_{3, 1} = C_0 \\

\end{aligned}

$$

如果把commitment $\delta_{i, 1}$ 压缩成一个commitment $\delta_1$，verifier 就无法直接通过上面的等式来进行校验。这其实就转换成了大家所熟知的IPA 证明，即Inner Product Argument verification。 接下来简单描述一下IPA协议的执行过程

<br />

### IPA protocol

prover 要证明query 向量$y$ 满足:
$$
\lang \textcolor{red} {u}, y \rang \overset{?}= \textcolor{red} {v}
$$

step 1: prover 生成多项式的commitment，并发送给verifier

$$
\begin{aligned}

C_u &= \sum_i [u_i]_{g_i} + [r_u]_h \\

C_v &= [v]_{g} + [r_v]_h \\

\end{aligned}
$$

step 2: prover 采样一个与向量$u$ 等长的向量$d$，对它进行commit；同样也与query 向量$y$ 交互，结果也进行commit，最后同样也发送给verifier

$$

\begin{aligned}

C_d &= \sum_i [d_i]_{g_i} + [r_1]_h \\

w &= \lang d, y \rang \\

C_{w} &= [w]_{g} + [r_2]_h \\

\end{aligned}

$$

step 3: verifier 发送一个challenge factor $e$ 给prover，prover 计算
$$
\begin{aligned}

u' &= e \sdot u + d \\

r_{u'} &= e \sdot r_u + r_1 \\

r_{v'} &= e \sdot r_v + r_2 \\

\end{aligned}
$$

并把它们全部发送给verifier。

step 4: 根据commitment 同态性质，verifier 验证：
$$

\begin{aligned}

\sum_i [u_i']_{g_i} + [r_{u'}]_h &= e \sdot \sum_i [u_i]_{g_i} + e \sdot [r_u]_h + \sum_i [d_i]_{g_i} + [r_1]_h \\

&\overset{?}= e \sdot C_u + C_d \\

\\

[\lang u', y \rang]_g + [r_{v'}]_h &= e \sdot [\lang u, y\rang]_g + [\lang d, y \rang]_g + [r_{v'}]_h \\

&\overset{?}= e \sdot C_v + C_w + [r_{v'}]_h

\end{aligned}

$$

<br />

### Reducing Sumcheck into IPA 
最后我们看看Hyrax 中Sumcheck 协议被reduced成IPA 协议后的执行过程：

#### Step One

把多个verification fold成一个：
$$
\begin{aligned}

J &= \sum_{i = 1}^{6} \rho_i \sdot M_i \\

&= 

\begin{bmatrix}

(2\rho_1 - \rho_2) \\
(\rho_1 - r_1 \rho_2) \\
(\rho_1 - r_1^2 \rho_2) \\
(\rho_1 - r_1^3 \rho_2) \\
(2\rho_2 - \rho_3) \\
(\rho_2 - r_2 \rho_3) \\
(\rho_2 - r_2^2 \rho_3) \\ 
(2\rho_3 - \rho_4) \\
(\rho_3 - r_3 \rho_4) \\
(\rho_3 - r_3^2 \rho_4) \\ 
(2\rho_4 - \rho_5) \\
(\rho_4 - r_4 \rho_5) \\
(\rho_4 - r_4^2 \rho_5) \\ 
(2\rho_5 - \rho_6) \\
(\rho_5 - r_5 \rho_6) \\
(\rho_5 - r_5^2 \rho_6) \\ 
\rho_6 \sdot (\widetilde{eq}(2, r') \sdot \widetilde{add}(4, r_L, r_R)) \\ \rho_6 \sdot (\widetilde{eq}(2, r') \sdot \widetilde{add}(4, r_L, r_R)) \\ \rho_6 \sdot (\widetilde{eq}(2, r') \sdot \widetilde{mul}(4, r_L, r_R)) \\

\end{bmatrix}^T \\


\\

\Longrightarrow \lang J, \pi \rang &\overset{?}= \sum_{i = 1}^6 \rho_i \sdot Q_i = \rho_0 \sdot s_0 \\

\end{aligned}
$$


对$\pi$ 进行commit：
$$
\begin{aligned}

\alpha_1 &= \sum_{i = 1}^4 [c_{i, 1}]_{g_i} + [r_{\alpha_1}]_h \\

\alpha_2 &= \sum_{i = 1}^3 [c_{i, 2}]_{g_i} + [r_{\alpha_2}]_h \\

\alpha_3 &= \sum_{i = 1}^3 [c_{i, 3}]_{g_i} + [r_{\alpha_3}]_h \\

\alpha_4 &= \sum_{i = 1}^3 [c_{i, 4}]_{g_i} + [r_{\alpha_4}]_h \\

\alpha_5 &= \sum_{i = 1}^3 [c_{i, 5}]_{g_i} + [r_{\alpha_5}]_h \\

X &= [v_0]_g + [r_X]_h \\

Y &= [v_1]_g + [r_Y]_h \\

Z &= [v_0 \sdot v_1]_g + [r_Z]_h \\

\end{aligned}
$$

对$Q$ 进行commit：
$$

C_0 = [s_0]_g + [r_{C_0}]_h \\

$$

commit 之后，prover要证明的变成了：
$$

\lang \vec{J}, (\vec{\alpha}, X, Y, Z)\rang \overset{?}= \rho_0 \sdot C_0 \\

$$

把两组commitment $\alpha = (\alpha_1, \alpha_2, \alpha_3, \alpha_4, \alpha_5,X, Y, Z)$ 和 $C_0$ 全部发送给verifier。

<br />

定义 $J^{*} = J[:-3]$，$\pi^{*} = \pi[:-3]$，即剔除掉最后三个元素，则：
$$
\lang \vec{J^{*}}, \vec{\alpha} \rang + \lang J_X, X \rang + \lang J_Y, Y \rang + \lang J_Z, Z\rang = \rho_0 \sdot C_0 \\

\Downarrow \\

\sum_{i = 1}^5 J_i \sdot r_{\alpha_i} = \rho_0 \sdot r_{C_0} - J_X \sdot r_X - J_Y \sdot r_Y - J_Z \sdot r_Z \\ 
$$


#### Step Two

prover 随机生成一个与 $\pi^{*}$ 等长的向量$d$，同$\pi^{*}$ 一样计算它的commitment，及$\lang J^{*}, d \rang$ 的commitment：
$$
\begin{aligned}

\delta_1 &= \sum_{i = 1}^4 [d_{i, 1}]_{g_i} + [r_{\delta_1}]_h \\
\delta_2 &= \sum_{i = 1}^3 [d_{i, 2}]_{g_i} + [r_{\delta_2}]_h \\
\delta_3 &= \sum_{i = 1}^3 [d_{i, 3}]_{g_i} + [r_{\delta_3}]_h \\
\delta_4 &= \sum_{i = 1}^3 [d_{i, 4}]_{g_i} + [r_{\delta_4}]_h \\
\delta_5 &= \sum_{i = 1}^3 [d_{i, 5}]_{g_i} + [r_{\delta_5}]_h \\

\\

C &= [\lang J^{*}, d \rang]

\end{aligned}
$$

把两组 commitment $\delta = (\delta_1, \delta_2, \delta_3, \delta_4, \delta_5)$ 和 $C$ 全部发送给verifier。

<br />

#### Step Three

verifier 发送一个challenge factor $e$ 给prover，prover 计算：
$$
\begin{aligned}

\text{\textcircled 1 } &\vec{z} = \begin{cases}
\vec{z_1} &= e \sdot [c_{0, 1}, c_{1, 1}, c_{2, 1}, c_{3, 1}] + [d_{0, 1}, d_{1, 1}, d_{2, 1}, d_{3, 1}] \\
\vec{z_2} &= e \sdot [c_{0, 2}, c_{1, 2}, c_{2, 2}] + [d_{0, 2}, d_{1, 2}, d_{2, 2}] \\
\vec{z_3} &= e \sdot [c_{0, 3}, c_{1, 3}, c_{2, 3}] + [d_{0, 3}, d_{1, 3}, d_{2, 3}] \\
\vec{z_4} &= e \sdot [c_{0, 4}, c_{1, 4}, c_{2, 4}] + [d_{0, 4}, d_{1, 4}, d_{2, 4}] \\
\vec{z_5} &= e \sdot [c_{0, 5}, c_{1, 5}, c_{2, 5}] + [d_{0, 5}, d_{1, 5}, d_{2, 5}] \\
\end{cases} \\

\\

\text{\textcircled 2 } &\begin{cases}
z_{\delta_1} &= e \sdot r_{\alpha_1} + r_{\delta_1} \\
z_{\delta_2} &= e \sdot r_{\alpha_2} + r_{\delta_2} \\
z_{\delta_3} &= e \sdot r_{\alpha_3} + r_{\delta_3} \\
z_{\delta_4} &= e \sdot r_{\alpha_4} + r_{\delta_4} \\
z_{\delta_5} &= e \sdot r_{\alpha_5} + r_{\delta_5} \\
\end{cases} \\

\\

\text{\textcircled 3 } z_C &= e \sdot \sum_{i = 1}^5 J^{*}_i \sdot r_{\alpha_i} + r_C \\
&= e \sdot (\rho_0 \sdot r_{C_0} - J_X \sdot r_X - J_Y \sdot r_Y - J_Z \sdot r_Z) + r_C \\

\end{aligned}
$$

#### Step Four

verifier 验证：
$$
\begin{aligned}

\text{\textcircled 1 } &\begin{cases}

\sum_{i = 1}^4 [z_{i, 1}]_{g_i} + [z_{\delta_1}]_h \overset{?}= e \sdot \alpha_1 + \delta_1 \\

\sum_{i = 1}^3 [z_{i, 2}]_{g_i} + [z_{\delta_2}]_h \overset{?}= e \sdot \alpha_2 + \delta_2 \\

\sum_{i = 1}^3 [z_{i, 3}]_{g_i} + [z_{\delta_3}]_h \overset{?}= e \sdot \alpha_3 + \delta_3 \\

\sum_{i = 1}^3 [z_{i, 4}]_{g_i} + [z_{\delta_4}]_h \overset{?}= e \sdot \alpha_4 + \delta_4 \\

\sum_{i = 1}^3 [z_{i, 5}]_{g_i} + [z_{\delta_5}]_h \overset{?}= e \sdot \alpha_5 + \delta_5 \\

\end{cases} \\

\\

\text{\textcircled 2 } &[\lang \vec{J^{*}}, \vec{z} \rang]_g + [z_C]_h \overset{?}= e \sdot (\rho_0 \sdot C_0 - J_X \sdot X - J_Y \sdot Y - J_Z \sdot Z) + C \\ 

\end{aligned}
$$

