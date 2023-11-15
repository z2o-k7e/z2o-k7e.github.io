[TOC]

### 签名是不是一种零知识证明?



一般的数字签名应该不属于interactive proof 因为不具有soundness的性质 也就不属于ZKP 但是比如schnorr签名 存在soundness的性质 是一个zkp

@洪澄: 首先得看是对什么信息的签名，如果是对有意义的信息比如文档，聊天记录的签名那就不是零知识的，因为这个签名本身就算是一个额外的知识。

至于对于随机挑战的签名是不是对于“我有某个公钥对应的私钥”的zkp，是难于blackbox的分析的，得描述协议再论证。例如schnorr确实是。



@Kurt: Schnorr签名不存在soundness性质，要求的是「不可伪造」性质，
但是Schnorr identification protocol (Sigma protocol）具有special soundness性质（和HVZK性质）



### 零知识证明是不是一种多方计算?

[TODO]



### 图灵机 电路计 算模型



CPU就是一个用（布尔门）电路模拟出来的通用图灵机，机器指令就是用门电路逻辑实现的，FPGA就是再用电路去模拟特定的图灵机。
但谁说图灵机必须要用到电路？图灵机只是个计算模型，我用纸带和笔也能构造出来一台通用图灵机，再比如你不知道外星人是不是也用门电路来做“CPU”，但邱奇-图灵论题就认为全宇宙的计算模型都一定会和图灵机等价。
反过来说图灵机也可以用来模拟电路，Verilog语言以及量子电路编程语言都是这类。
Lambda Calculus也是另一个图灵完备的计算模型，日本过去的“五代机”计划就是在用集成电路直接实现Lambda Calculus计算模型的机器。
图灵完备的计算模型还有很多好玩的呢，比如递归函数，比如元胞自动机/生命游戏。
SNARK里道理也是一样，只是一般的target是VM这一级别，当然zkCPU理论上也没问题，再当然人脑通过训练学习去成为人工zk编译器也没啥不可能。





### 信任机器 如何体现

> 自从 2016年 《经济学人》提出来的「区块链是信任机器」的说法，我就在一直思考这个问题：「信任机器」如何体现。自从深入思考零知识证明以后，我慢慢发现他们殊途同归，只是各自的维度不一样，区块链解决的是「分布式计算的信任」，零知识证明解决的是「数据的信任」。如果再加上形式化验证，就可以解决「逻辑的信任」。
>
> 这三个点：**逻辑 <-> 计算 <-> 数据** 才真正构成了一个闭环，也许才能真正实现「信任机器」这一构想。

ref: <https://secbit.io/blog/2019/07/14/zkpod-short-intro/#back-to-top>

Kurt :

「计算」是对「数据」/「信息」的处理/变换过程；

「证明」是对「命题」/「符号」的推理/推导过程；

「逻辑」是正确的「推理」过程；

「程序」是写出来的「计算」过程（「算法」）；

「证明」是写出来的「逻辑」（「推理」）过程；

「知识」是「计算能力」；

Von Neumann architecture: 程序 == 数据 ；

Curry–Howard isomorphism: 程序 == 证明；

所以「证明程序的integrity」== 「证明推理符合正确的逻辑」



因为程序==证明，所以「formal verification」可以在计算机里实现。verification也是计算，但是是符合逻辑系统推理规则的计算过程（程序）；



「witness」就是「non-determinism」计算中的计算路径信息；「proof」就是「witness」的压缩编码；

NP-proof system中，proof generation 计算过程就是 verification计算过程的 non-deterministic version；

ZK-proof system相比而言添加了对witness的压缩编码。



> 那是不是说明zkproof的生成和验证过程就是formal verification?



本身不formal （formal的意思是符合特定逻辑系统，我不认为PIOP这种模型目前能达到formal的程度），要加 typing rule/ dynamic semantics rule等等。 比如见： https://eprint.iacr.org/2023/657.pdf



> 就是说零知识证明只能保证计算的完整性，如果你一开始写的智能合约逻辑就错了，就有漏洞的话，还还是一个有bug的合约。形式验证应该是可以保证算法逻辑就是正确的没有漏洞？



是的，计算完整性和计算正确性是两个东西：计算完整性是保证计算的「输出」是正确的（是和计算一致的），计算（语义）正确性是说计算本身是无bug的。（ZKP里的completeness是说真命题可以生成有效证明；MPC里的correctness是说协议可以正确实现functionality）

FV 形式化验证可以使用逻辑模型对程序逻辑进行部分检验，可以排除常见bug，但通用debugger是不存在的（因为「图灵停机问题不可判定」）。



### R1CS

> r1cs 的基本公式是 `AX*BX=CX`，`*`  是hadmard product，X是变量向量。
> 请教一个基本问题是，r1cs里的r1，是否指某个matrix的rank=1？如果是，那么是哪个矩阵的秩为1?



这里的 秩1约束 实际上和组合优化/凸优化 里面的秩1约束是一个东西。实际上就是说  这里的约束条件  $(x^T * A) (B^T * x) + C^T * x$ 是秩1的。其实就是   $rank(AB^T)=1$，因为A B都是向量。

正解，我再来重述一遍：考虑一个约束 $(a^T x) (b^T x) - (c^T x) = 0$ ，乘法部分可以重写为 $(x^T a) (b^T x) = x^T (a b^T) x$，即为变量为 x 的「二次型」。而矩阵 $(a b^T)$ 的秩一定为1 （ $Rank(a b^T) = 1$），因为向量 outer product 生成的矩阵每一列都线性依赖（正比）于特定一列（比如第一列），其rank一定等于1。这就是Rank 1的由来。



> 那么non-rank1 constraint指什么？

区分两个概念：rank-1 constraint vs degree-2 constraint。最终只出现一次乘法的都叫deg2 constraint，但只有二次型矩阵M=(ab^T)的才叫rank1 constraint。换句话说，一个deg2 non-rank1 constraint就是二次型矩阵M的秩大于1的情况（就不对应于一个fan-in 2的算术电路乘法门了），比如rank=2时可以称之为R2CS。Justin的意思是R1CS真正的局限并不在于没有R2CS不能custom constraint，而是在于不支持higher degree (>2) constraint，比如fan-in > 2 的乘法门。



> 能举个degree2+non rank1 的例子？

因为rank1就已经可以对fan-in 2 算术电路（的乘法门）进行约束了，那么在算术电路语境下就一定举不出来一个“例子”，因为没有必要性。二次型x^T M x的矩阵M并没有限制rank=1，所以rank>1也一定可以表示某种约束，只不过不是对算术电路的约束。也许可以举出凸二次优化里的例子，但我并不熟悉。



### extractor

Prover是一个黑盒，extractor以任意方式(包括存档取档)“把玩”这个黑盒，最后能提取出witness，这说明黑盒里面真的有witness。

整个目的是为了要证明证明者真的有witness，毕竟都能抽取出来



### 怎么理解 zk 的前后端?

有两种解释：
1. 前端=算术化/后端=证明系统
2. 前端=密码学编译器（比如多项式承诺）/后端=信息论证明系统（比如IOP）

yingtong有一个讲义 在论述这个 proof system的stack https://assets.super.so/9c1ce0ba-bad4-4680-8c65-3a46532bf44a/files/e11309fb-7356-42ad-9c78-565341abd80d.pdf

我是在想，或者说我的观察是，一旦一种表达形式确定了，它的 iop 基本上也被框定的，比如说AIR，可能就是FRI，假如转换成 pair based 大家会觉得我有病



### why Knowledge soundness is not a meaningful notion for the sumcheck protocol?

好问题！直观上说是因为 sumcheck 要证的statement 没有(多项式长度的）witness，要证的就是一个evaluation 的 summation = a value，verifier 有本事自己也能自己算一遍（可惜一般他没有这种计算能力）。 往深了说KS抽取一个witness这件事有一个前提，就是要「存在」一个多项式长度的witness，这正是NP的定义，所以一般KS性质都是针对NP语言内的讨论。而sumcheck可以证的语言比如UNSAT/\#SAT/TQBF，这三个语言都不是NP的（分别属于coNP/ \#P/ PSPACE)。



### 什么 snark 协议需要 Knowledge sound，哪些只需要sound就够了

- 直观上说是因为sumcheck要证的statement里没有(多项式长度的）witness，要证的就是一个evaluation 的 summation = a value，verifier有本事自己也能自己算一遍（可惜一般他没有这种计算能力）。
  往深了说KS抽取一个witness这件事有一个前提，就是要「存在」一个多项式长度的witness，这正是 NP 的定义，所以一般KS性质都是针对NP语言内的讨论。而sumcheck可以证的语言比如 UNSAT/#SAT/TQBF ，这三个语言都不是 NP 的（分别属于 coNP/#P/PSPACE )。
- 证明a statement is true和我「知道」「为什么」this statement is true中间是有gap的；反过来后者KS蕴含soundness，我「证明statement is true+证明知道为什么statement is true」蕴含「证明statement is true」。
- 去证明一个「数学定理」is true的时候，验证者其实不太关心一定要知识抽取的，只要让我相信定理is true就够了，上面sumcheck证的语言都是这种。
- 而对于KS必需的情况举一个例子：对一个地址，我用SNARK证明了对应的私钥（比如离散对数或哈希原像）是「存在」的。这完全不能用！我得去证明我「知道」这个地址相对应的私钥（而不仅仅是私钥存在），我才能去发起交易。
- 此外还有的情况下，KS性质都是不够的！比如可以对不包括在约束中的instance更改以进行[malleability attack](https://geometry.xyz/notebook/groth16-malleability)。如果还要在这种攻击存在下依然安全，那要满足simulation extractability (SE性质)。（UC-secure imply SE)。SE最新相关内容有一篇欧密23的文章 [Spartan and Bulletproofs are Simulation-Extractable (for Free!)](https://link.springer.com/chapter/10.1007/978-3-031-30617-4_18)

>  注： malleability attack 可见 the halo2 book