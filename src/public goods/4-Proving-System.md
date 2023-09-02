本文假设您对椭圆曲线运算及哈希函数等有着基础的了解

### 简洁的 Schnorr 协议

Alice 拥有一个秘密数字，`a`，我们可以把这个数字想象成「私钥」，然后把它「映射」到椭圆曲线群上的一个点 `a*G`，简写为 `aG`。这个点我们把它当做「公钥」。

-   `sk = a`  ( secret key = a ) 
-   `PK = aG`

> a secret key $a$ that corresponds to a public key $g^a$.

请注意「映射」这个词，给任意一个有限域上的整数 `r`，我们就可以在循环群中找到一个对应的点  `rG`，或者用一个标量乘法来表示 `r*G`。但是反过来计算是很「困难」的，这是一个「密码学难题」—— 被称为离散对数难题。

> 取模之后 , 就很难知道原来的指数是多少了。 事实上，如果模取得相当大，从运算结果倒推指数运算就不可行了；**现代密码学很大程度上就是基于这个问题的“困难”**

也就是说，如果任意给一个椭圆曲线循环群上的点 `R`，那么到底是有限域中的哪一个整数对应 `R`，这个计算是很难的，如果有限域足够大，比如说 256bit 这么大，我们姑且可以认为这个反向计算是不可能做到的

Schnorr 协议充分利用了有限域和循环群之间单向映射，实现了最简单的零知识证明安全协议：Alice 向 Bob 证明她拥有 `PK` 对应的私钥 `sk` 

![](http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-02-04-120945.png)

1. 第一步：为了保证零知识，Alice 需要先产生一个随机数 `r`，这个随机数的用途是用来保护私钥 $a$ 无法被 Bob 抽取出来。这个随机数也需要映射到椭圆曲线群上即 `rG`。  ( 映射之后 , Bob 就不可能通过 `rG` 推算出 `r ` )
2. 第二步：Bob 要提供一个随机数进行挑战，我们把它称为  `c`。
3. 第三步：Alice 根据挑战数 `c`  计算  `z = r + c * a (即sk)`，把 `z`  发给 Bob，Bob 在自己这边通过下式进行检验：

```rust
z*G ?= R + c*PK 
    ?= rG + c*(aG)
```

大家可以看到 Bob 在第三步「同态地」检验 `z` 的计算过程。如果这个式子成立，那么就能证明 Alice 确实有私钥 `a`。可是，这是为什么呢？

`z` 的计算和验证过程很有趣，有几个关键技巧：

1.  首先 Bob 必须给出一个「随机」挑战数 $c$ ，然后 Bob 在椭圆曲线上同态地检查 `z` 。如果我们把挑战数  $c$  看成是一个未知数，那么 `r+a*c=z` 可以看成是一个一元一次方程，其中 `r` 与 `a` 是方程系数。请注意在 `c` 未知的前提下，如果 `r + a*x = r' + a'*x` 要成立，那么根据 Schwatz-Zippel 定理，极大概率上 `r=r'`，`a=a'` 都成立。也就是说， Alice 在 `c` 未知的前提下，想找到另一对不同的 `r'`,`a'` 来计算 `z` 骗过 Bob 是几乎不可能的。这个随机挑战数 `c` 实现了`r` 和 `a` 的限制。虽然 Bob 随机选了一个数，但是由于 Alice 事先不知道，所以 Alice 不得不使用私钥 `a` 来计算 `z`。这里的关键： `c` 必须是个随机数。
2.  Bob 验证是在椭圆曲线群上完成。Bob 不知道 `r` ，但是他知道 `r`  映射到曲线上的点 `R` ；Bob 也不知道 `a`，但是他知道 `a` 映射到曲线群上的点 `PK`，即 `a*G`。通过同态映射与Schwatz-Zippel 定理，Bob 可以校验 `z` 的计算过程是否正确，从而知道 Alice 确实是通过 `r` 和 `a` 计算得出的 `z`，但是又不暴露 `r` 与 `a` 的值。
3.  还有，在协议第一步中产生的随机数 `r` 保证了 `a` 的保密性。因为任何一个秘密当和一个符合「一致性分布」的随机数相加之后的和仍然符合「一致性分布」。

看懂了这个图就看懂了 !!!!! 
![](http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-02-04-120945.png)


是 Sigma 零知识证明的一个特例

#### Schnorr 的非交互式版本

Schnorr 协议的非交互式版本可以避免 Prover 与 Verifier 的交互，但这要求 Prover 使用哈希函数，这样他就无法预测哈希函数的输出，非交互式版本的验证器实现非常简单，因为它不需要随机数生成器

(Making the protocol non-interactive)

首先定义:  $a$  是 $sk$ 即私钥 ;  $PK$  是 Public key 即公钥 ;    $g^a = PK$ 

1. Prover 生成一个随机数 $r$  并创建一个承诺  $comm = g^r$ , Prover 对   $g、comm、PK$  进行哈希处理以获得挑战值 $c$  ,   $c = hash(g, PK, comm)$ 
2. Prover 创建对挑战的响应 $s = r + c * a$ , 然后将元组 `(comm, s)` 发送给验证者。

$$
Prover \ \ generate  \ \ \begin{aligned}
& \left\{
   \begin{array}{l}
     \quad comm \\
     \quad  s = r + c * a
   \end{array} \right.
\end{aligned}
$$

Verifier 自己计算 $c' = Hash(g, \ PK, \ comm)$  , 然后验证 : 

$$
\begin{array}{l}
g^s \quad &\overset{?}{=} \quad {\color{red}PK^{c'}} + {\color{blue}comm} \\ 
 &\overset{?}{=} {\color{red}PK^ {c'}} + {\color{blue}g^r} \\
 &\overset{?}{=} g^{ {\color{orange} {a} \cdot c'}} + g^{{\color{orange}r}} \quad (\because PK = g^a, \ a \ is \ sk) \\ 
g^{r \ + \  c * a} &\overset{?}{=} g^{ r \  + \ {a} \cdot c'  }
\end{array}
$$

如果 Verifier 自己验证这个等式相等, 则 Prover 就通过 $r + a \cdot c$  这种方式隐藏了私钥 $a$ , 同时又能让对方确信自己真的有这个私钥 $a$  .


1. The prover generates a random number `r` and creates a commitment `com = gʳ`. The prover hashes  `g`, `com` and `y` to get challenge `c`. `c = Hash(g, y, t)`.
2. The prover creates a response to the challenge as `s = r + c*x`. The prover sends tuple `(t, s)` to the verifier.

The verifier now generates the same challenge `c` as `Hash(g, y, t)` and again checks if `gˢ` equals `yᶜ.t`. [Python code demonstrating this protocol](https://github.com/lovesh/python-rubenesque/blob/f202f82104e161566793002f4857961f7cf12744/test/test_sigma.py#L8).


#### Schnorr 的问题 

对不同的消息, 如果不幸选了相同的随机数  $r$   私钥就会泄露

如果 Alice 在两次交互过程中使用了同一个 `K`，那么 Bob 可以通过发送两个不同的 `c` 和 `c'` 来得到 `s` 和 `s'`，然后通过下面的公式算出私钥 `a`：

```python
s  = (c +a*e)/k , 
s' = (c'+a*e)/k , 两式相减, 求出 k 

k = (c - c')/(s - s')
a = (k * s - c)/e
```


### ECDSA

> Bitcoin 和 ETH 都支持 ECDSA signature.


#### why need ECDSA?

除了显而易见的“我需要对一份文件/合同进行签名”，还有一个非常流行的应用场景：让我们以一个不想自己的数据被用户修改或者破坏的应用程序为例，比如一个只允许你载入官方地图和不可修改的模块的游戏，或者一部只允许你安装官方应用程序的手机或其它设备。

在这些案例当中，相关文件（应用程序、游戏地图、数据等）会用 ` ECDSA` 进行签名，公钥会随应用程序/游戏/设备一起捆绑并用来验证签名来确保数据没有被修改，而私钥在本地一个私密的地方进行保存。由于你可以用公钥对签名进行验证，但是不能用它创建或者伪造新的签名，你可以无所顾忌地将公钥随应用程序/游戏/设备一起分发。

这与`AES`相比，区别是显而易见的。`AES`加密系统允许你对数据进行加密，但是你需要用密钥来解密，这就要求你将密钥与应用程序一起捆绑，破坏了对数据进行保护防止数据被用户修改的目的。

一个很好的例子就是PS3的控制台，它被大量的破解，所有的文件可以解密，所有的密钥可以从解密的文件当中抽取，但是为了能够在最新的固件上面运行程序，你还需要破解一个`ECDSA`的数字签名。

当你想要对一个文件进行签名的时候，你会用这个私钥 / 随机数 / 文件的哈希组成一个魔法数学方程，这将给出你的签名。签名本身将被分成两部分，称为 `R` 和 `S` 

 - 选择随机数  $k \in F_r$  ,  计算承诺 :  $R={\color{red}k^-1} \cdot G$ 
 - 挑战 : 取  $R$  的横坐标为  ${\color{orange}r} = R_{X} \pmod{F_r}$      (先 mod  $F_p$  ,  再 mod $F_r$ )
 - 响应 :  ${\color{orange}s} :={\color{red}k} \cdot (m+{\color{orange}r} \cdot x) \ \ \pmod{F_r}$     

**为了验证签名的正确性，你只需要公钥（用私钥在曲线上面产生的点）并将公钥和签名的一部分 `S` 一起代入另外一个方程，如果这个签名是由私钥正确签名过的数字签名，那么它将给出签名的另外一部分  `R` 。**

$$
R' := (s^{-1} \cdot m) \cdot G  \ \ + \ \  (s^{-1} \cdot r)  \cdot PK
$$

简单来说，一个数字签名包含两个数字，`R` 和  `S`，然后你使用一个私钥来产生  `R`  和  `S`  ，如果将公钥和  `S`  代入被选定的魔法数学方程给出  $R'$ , 且  $R==R'$  的话，这个签名就是有效的。仅仅知道公钥是无法知道私钥或者创建出数字签名。

#### Algorithm

初始化： 椭圆曲线生成元为 $G$，标量域为 $F_r$，基域为 $F_q$ 

> **基域** 理解为椭圆曲线点的横纵坐标的取值范围
> **标量域** 即做倍点运算的标量的取值范围, 比如 $PK =x \cdot G$  里的 $x$ , 其不会超过椭圆曲线的阶 $q$  

密钥生成：  私钥  $x \in F_r$  和公钥  $PK =x \cdot G$

签名:   输入任意消息 $M$ ， 计算  $m:=Hash(M) \ \ \ \pmod{|Fr|}$  
 - 选择随机数  $k \in F_r$  ,  计算承诺 :  $R={\color{red}k^-1} \cdot G$ 
 - 挑战 : 取  $R$  的横坐标为  ${\color{orange}r} = R_{X} \pmod{F_r}$      (先 mod  $F_p$  ,  再 mod $F_r$ )
 - 响应 :  ${\color{orange}s} :={\color{red}k} \cdot (m+{\color{orange}r} \cdot x) \ \ \pmod{F_r}$         ( k 增加了 ECDSA 的难度)

则签名为 $({\color{orange}r, \ \ s})$

> $k^{-1}$   是 $k$   的乘法逆元

我们是如何对一个文件或者一个信息进行签名的呢？

1. 你需要知道签名本身是 40 字节，由各20字节的两个值来进行表示，第一个值叫作 $R$，第二个叫作 $S$。
2. 值对  $(R, S)$  放到一起就是你的  `ECDSA`  签名


#### 验证 : 

验证它，也非常的简单，你只需要 [公钥] 和导出这个公钥的曲线参数就可以了。你用以下方程来计算  $R'$ ：


Verifier  : 
1. 输入消息 $M$ , 计算  $m := Hash(M)$   
2. 校验  $r, \ s \in F_r$   
3. 计算  

$$
R' := (s^{-1} \cdot m) \cdot G  \ \ + \ \  (s^{-1} \cdot r)  \cdot PK
$$

取  $R'$  的横坐标为  $r' = R'_{X} \pmod{F_r}$    ,  校验等式  $r ==r'$   : 如果相等, 则接受 , 否则拒绝

公式推导过程如下: 

$$
\begin{aligned}
&R' =  (s^{-1} \cdot m) \cdot G  \ \ + \ \  (s^{-1} \cdot r)  \cdot PK \\
&=(s^{-1} \cdot m) \cdot G  \ \ + \ \  (s^{-1} \cdot {\color{red}r\cdot x) \cdot G} \\
&=(s^{-1} \cdot (m+rx)) \cdot G \\
&=k^{-1} \cdot G \\
&=R
\end{aligned}
$$

这里知道 $k$  还是可以推算私钥,  所以 EIP-32 要求 :  $k = Hash(sk, \ \ message, \ \ content)$


### EdDSA

> 以太坊 BN256 曲线已经支持了 EdDSA

EdDSA 正是为了解决 Schnorr 签名私钥泄露的问题 :  他不是选择随机数, 而是计算随机数

初始化 : 椭圆曲线生成元为 $G$ ,  阶为  $q$
密钥生成 : 私钥为 $x$ ,  公钥为  $PK = x \cdot G$

签名： 消息为 $m$，计算随机数  $\color{red}r=hash(x,m)$**，计算承诺  $R=r·G$，

计算挑战  $e:=hash(R,PK,m)$

计算响应  $s:=(r +e·x) \mod{q}$

签名为(R,s)

验证： 重新计算挑战  $e:=hash(R,PK,m)$ ，然后校验  $sG==R+e·PK$

与 ECDSA 最大的区别在于  $r$  是算出来的, 没有使用随机数
这样产生的签名结果是确定性的，即对同一消息, 签名结果相同, 不会额外泄露信息

一般说来随机数是安全措施中重要的一种方法，但是随机数的产生也是安全隐患，著名的索尼公司产品 PS3  密钥泄露事件，就是随机数产生的问题导致的 (写死在了代码里, 晕)。


### zk-SNARK

在聊 zk-SNARKs 之前, 首先来看 NARK(Non-interactive ARgument of Knowledge) :

![](http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-07-20-023801.png)
-  C : 电路 Circuit 
-  $x$ : 公开声明 public statement
-  $w$ : $\color{brown}secret$ _witness_ 
-  **预处理(Preprocessing)** 也称为 **Setup**, 它以电路的描述作为输入,然后输出这些公开参数,我们称之为 $pp$ 和 $vp$ : 
-  $pp$ 表示**公开的**参数,供证明者使用。
-  $vp$ 表示**公开的**参数,供验证者使用。


![](http://imagesoda.oss-cn-beijing.aliyuncs.com/Sodaoo/2023-07-20-024037.png)

证明者和验证者各自会输入 : 
- prover takes the  $x$ (public statement)   &  $pp$ (public (circuit)params)  &  the Witness
- Verifier takes $vp$  &  $x$ (public statement) 

然后,证明者试图向验证者证明:  **It knows some  $w$  such that  $C(x, \ w)=0$**

**NARK Definition** :  A pre-processing NARK is a triple  $(S,P,V)$ , where : 
- $S(C) \rightarrow$  generate the Circuit's  $pp \ \ \&  \ \ vp$  as public params for P & V.
- $P(pp, {\color{green}x} ,{\color{brown}w} ) \rightarrow$ :  proof  $\pi$
- $V(vp, {\color{green}x} ,{\color{blue} \pi} ) \rightarrow$ :  $\color{green}Accept$ _or_ $\color{red}Reject$

> 所有算法和对手都可以访问 *随机预言机 (random oracle)* 


zk-SNARKs 条件是苛刻的, 因为要让 Verifier 在如此短的时间内完成某些验证, 我们需要一些新的方法来去处理计算, 比如多项式承诺 (polynomial commitment)

(To be continued ...)





### Reference : 

Vitalik ZK_Snark
zk-learning Lectures
安比 zk-snarks
https://vitalik.ca/general/2021/01/26/snarks.html
[Zero Knowledge Proofs with Sigma Protocols](https://medium.com/@loveshharchandani/zero-knowledge-proofs-with-sigma-protocols-91e94858a1fb)
