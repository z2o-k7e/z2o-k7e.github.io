[TOC]

> 💡 作为 zkp 新人，走了很多弯路，也整理下自己的学习路径和一些参考资料供大家入门。 希望本教程可以帮助减少一些盲目的打击和莫名的痛苦，节省一点点时间。
>
> ... 一起携手勇敢地走下去！

### 1. 建立对 zkp 的直观理解

① 在纵身潜入 ZKP 的海洋之前，可以先建立对它**最直观的理解**：

- [1. 初识「零知识」与「证明」](https://github.com/sec-bit/learning-zkp/blob/master/zkp-intro/1/zkp-back.md)
- [2. 理解「模拟」](https://github.com/sec-bit/learning-zkp/blob/master/zkp-intro/2/zkp-simu.md)
- [3. 寻找「知识」](https://github.com/sec-bit/learning-zkp/blob/master/zkp-intro/3/zkp-pok.md)


> 安比实验室（郭宇老师）所写的 zkp-intro 是公认目前全网最简洁易懂的 zkp 入门系列（而且还是中文的！！）  
> 
> 前 3 篇需要看懂，不了解的概念就 Google + chatgpt + 社群询问 ...
>
> Chap 4-5 主要是非交互的 Schnorr 和 CRS、哈密尔顿环路等，看不懂没关系，可以先放一放


### 2. 最小必要背景知识

在建立了对 ZKP 最直观的了解后，如果你还是打算要学下去的话，那么就开始准备一些最小必要的基础知识吧！

#### 2.1 椭圆曲线 ECC 

需要掌握椭圆曲线加密（ECC）原理（ 大概用时 30 min）  

 - [椭圆曲线推荐 Tutorial](https://blog.cloudflare.com/a-relatively-easy-to-understand-primer-on-elliptic-curve-cryptography)
 - [ECC 椭圆曲线](https://github.com/zkp-co-learning/zkp-co-learn/discussions/29)
 - [ECC 密码学椭圆曲线(简短、清楚)](https://www.bilibili.com/video/BV1v44y1b7Fd/?spm_id_from=333.337.search-card.all.click)  
 - [椭圆曲线 illustration](https://curves.xargs.org/)

#### 2.2 基础的群论、数论

初等数论 `Number Theory`
 - 10 分钟直观理解群: [3Blue1Brown - 欧拉公式与初等群论](https://www.bilibili.com/video/BV1fx41187tZ)
 - [直观理解 - 如何给高中生解释群论？](https://www.zhihu.com/question/27807675/answer/123680054)
 - [Group Theory 群论.pdf](https://github.com/Antalpha-Labs/zkp-co-learn/files/10980275/Group.Theory.pdf)  
 - [Number Theory 1.pdf](https://github.com/Antalpha-Labs/zkp-co-learn/files/10980285/Number.Theory.1.pdf)  

#### 2.3 密码学基础

需要掌握：群环域概念、循环群、拉格朗日插值

 - 视频课 (视频课 For 纯密码学小白)：[密码学的数学基础](https://www.bilibili.com/video/BV1qs4y1s7kv/?spm_id_from=333.1007.top_right_bar_window_custom_collection.content.click&vd_source=cdb8c15859fbdd5efddeed0be9186c4b)
 - [图解密码技术（第3版）](https://book.douban.com/subject/26822106/)  9.5 分密码学书籍，随便翻翻

#### 2.4 ZK-SNARK 初识 & 原理:  

需要掌握：直观理解循环群在 zk-snark 中是咋用的就可以，具体的算法细节可能要持续往后学，和 PLONK 的算法不断交叉回看才能懂。

推荐 [sec-bit](https://secbit.io/blog/2020/01/08/learn-zk-snark-from-zero-part-three/) ZK-SNARK 的系列文章，也可在微信公众平台搜索，对于初学者先看 Part 1 / 2 就够了。

 - [zk-snark Part 1](https://secbit.io/blog/2019/12/25/learn-zk-snark-from-zero-part-one/)
 - [zk-snark Part 2](https://secbit.io/blog/2020/01/01/learn-zk-snark-from-zero-part-two/)

### 3. 理论交叉学习，我反复入门啊！

有了以上基础的打底，可以尝试一套体系完整的系列课程：

[ZKIAP](https://zkiap.com/)
> ZKIAP 的课程是比较注重理论和实践结合的，第二课就有涉及到 Circom 写电路

[zk-learning](https://zk-learning.org/)
> 理论详实，但是缺少代码实践，session 5 的 PLONK 是 top-down 讲解，搭配郭老师的 理解 PLONK 系列会更佳

[crypto notes](https://crypto-notes-erhant.vercel.app/)
> 土耳其小哥整理的，非常赞的 notes

[ProofsArgsAndZK](https://people.cs.georgetown.edu/jthaler/ProofsArgsAndZK.html)
> author: justin thaler 

https://zkhack.dev/whiteboard

https://www.rareskills.io/zk-bootcamp


----

Others 优秀 Courses （随便看看）: 

[Zero Knowledge Canon, part 1 & 2 - a16z crypto](https://a16zcrypto.com/posts/article/zero-knowledge-canon/)

https://twitter.com/taikoxyz/status/1679468185291218944

1. PSE appliedzkp.org/projects 
2. Rust
3. complaints about learning rust
4. Dan Boneh
5. "The Different types of ZK-EVM" article 
6. who was the first ZK-EVM.
7. ZK Summit – Zero Knowledge Summit
8. Zac Williamson inventing PLONK, running @aztecnetwork
9. @zeroknowledgefm podcast
10. Fiat-Shamir Transformation 
11. The Moon Math Manual https://github.com/LeastAuthority/moonmath-manual
12. ZK-Rollups that provide privacy by default are sometimes called ZK-ZK-Rollups
13. Circom
14. The "Proofs, Arguments, and Zero-Knowledge" book by Justin Thaler https://people.cs.georgetown.edu/jthaler/ProofsArgsAndZK.pdf
15. Brecht – @taikoxyz CTO and @PrivacyScaling contributor
16. ZK HACK
17. Definition wars
18. how one should write zkEVM or ZK-EVM or Zk-EVM or zk-EVM
19. Lagrange interpolation

### 4. PLONK 协议の奥义

PLONK 无疑是目前最值得学习，需要彻底掌握的协议，halo2

> 出品：Secbit @郭宇 老师 , https://secbit.io/

《理解 Plonk》
[1-Plonkish Arithmetization](https://learn.z2o-k7e.world/plonk-intro-cn/plonk-arithmetization.html)
[2-多项式编码](https://learn.z2o-k7e.world/plonk-intro-cn/plonk-lagrange-basis.html)
[3-置换证明](https://learn.z2o-k7e.world/plonk-intro-cn/plonk-permutation.html)
[4-算术约束与拷贝约束](https://learn.z2o-k7e.world/plonk-intro-cn/plonk-constraints.html)
[5-多项式承诺](https://learn.z2o-k7e.world/plonk-intro-cn/plonk-polycom.html)
[6-实现 Zero Knowledge](https://learn.z2o-k7e.world/plonk-intro-cn/plonk-randomizing.html)
[7-Lookup Gate](https://learn.z2o-k7e.world/plonk-intro-cn/plonk-lookup.html)

是大家公认的全网（包括外网）最好的 PLONK Tutorial。
学习 PLONK，这个就够了（论文可以随便看一下）

PS: 如果文档看晕了，那么推荐郭老师的配套白板视频：
  - [【理解 PLONK - 1】 by 郭宇老师@安比实验室 05/02](https://github.com/zkp-co-learning/zkp-co-learn/discussions/86)
 - [【理解 PLONK - 2】 by 郭宇老师@安比实验室 05/03](https://github.com/zkp-co-learning/zkp-co-learn/discussions/89)
 - [【理解 PLONK - 3 - lookup】 by 郭宇老师@安比实验室 05/09](https://github.com/zkp-co-learning/zkp-co-learn/discussions/95)

-----

PLONK 协议代码实践：

- Rust 实现: https://github.com/adria0/plonk-by-fingers
- Harry 参考一下代码实现 : https://github.com/zkp-co-learning/ZKP/blob/main/5-plonk-intro.md 
- Dr. Qi zhou 分享: https://706community.notion.site/Python-finite-field-poly-over-Fq-FFT-1968732f468c4c3fa3886a5658c58773?pvs=4

### 5. 要不...来点代码?

恭喜你来到了 Zero-knowledge 的荒原！下面就可以自己根据兴趣和方向选择一些代码进行研究和实践了

#### halo2

使用了 halo2 的 Applications: 

- ZK Email https://github.com/zkemail  halo2   
- ZK Wordle: https://zordle.xyz/  halo2
- Hammster： https://github.com/ytham/hammster  halo2
 - zk-draw : Verifiable random draw with zero-knowledge of the random seed https://github.com/jae-cuz/zk-draw   halo2
- ZK Microphone: https://github.com/Miyamura80/ZKMicrophone 
- [Building a Zero Knowledge web app with Halo 2 and Wasm (part 1)](https://medium.com/@yujiangtham/building-a-zero-knowledge-web-app-with-halo-2-and-wasm-part-1-80858c8d16ee)
- zk-img: Fighting Deepfakes with Zero-Knowledge Proofs   https://medium.com/@danieldkang/zk-img-fighting-deepfakes-with-zero-knowledge-proofs-9b76c23e3789  尚未开源

大部分由 @Kurt Pan 博士整理

#### Circom

使用了 Circom 的 Applications: 
 - zkSudoku: https://zk-sudoku.vercel.app/  Circom
 - Tornado-Cash
 - Semaphore

学习路径：【EDITING...】

#### PSE demos

PSE Projects List : https://www.appliedzkp.org/projects

[Semaphore](https://semaphore.appliedzkp.org/)
> 对于 Circom + ZKP 的代码实践例子比较多，首推 PSE 的 Semaphore，是个包括 zuzalu pass、Worldcoin 都有使用的 zkp 协议


### Others:

巨量资料 FYI，大部分出自网络及群聊整理，出处无法详实覆盖，如需加上出处请联系我 ~

#### Nova

白菜: 有对NOVA-ML 感兴趣的不，这个课题可能比较大，可以考虑组个支队了 😂  
- https://github.com/socathie/nova-ml
- awesome-zkml : https://github.com/worldcoin/awesome-zkml
- Team Novi (Nova):  https://0xparc.notion.site/Team-Novi-2d81bc06b0aa4c99b61a9ee06166b3b6
- risc0-nova
	- https://github.com/hero78119/risc0-nova
- [Parallelizing Nova](https://hackmd.io/@9MPvD9czTQS18zEjAERVAg/Hy-vKK04h)
 -  [Nova: A New Chapter in Zero-Knowledge Proofs](https://huobi-ventures.medium.com/nova-a-new-chapter-in-zero-knowledge-proofs-108110cd816e)  (medium这糟糕的排版)

#### PSE

 - [Meeting PSE Slides]([https://docs.google.com/presentation/d/1zXAsGyyq_DZ2WdGjCow3cP0TVhTxkfRaa3q3Rz6z4U4/edit#slide=id.g1e4808542a4_1_17](https://docs.google.com/presentation/d/1zXAsGyyq_DZ2WdGjCow3cP0TVhTxkfRaa3q3Rz6z4U4/edit#slide=id.g1e4808542a4_1_17))
 - PSE Projects List : https://www.appliedzkp.org/projects
 - Make your first contribution to any open-source Web3 project : [https://www.useweb3.xyz/contribute](https://www.useweb3.xyz/contribute)
 - LIST： [https://www.appliedzkp.org/projects](https://www.appliedzkp.org/projects)

Some Project Boards:
- [https://github.com/orgs/semaphore-protocol/projects/10](https://github.com/orgs/semaphore-protocol/projects/10)
- [https://github.com/orgs/privacy-scaling-explorations/projects/3/views/9](https://github.com/orgs/privacy-scaling-explorations/projects/3/views/9)

**Proof of Innocence（清白证明）：**
 - [https://github.com/chainwayxyz/proof-of-innocence](https://github.com/chainwayxyz/proof-of-innocence)
 - Twitter : Follow @drCathieSo_eth  @AndyGuzmanEth


#### [Awesome ZKP](https://github.com/matter-labs/awesome-zero-knowledge-proofs)

Awesome 系列 : 

[Awesome Noir](https://github.com/noir-lang/awesome-noir)
[Awesome Circom](https://github.com/arnaucube/awesome-circom)
[Awesome Aleo](https://github.com/howardwu/awesome-aleo)
[Awesome Cairo](https://github.com/auditless/awesome-cairo)
[Awesome-Folding](https://github.com/lurk-lab/awesome-folding)
[Awesome HALO2](https://github.com/adria0/awesome-halo2)
[Awesome PLONK](https://github.com/fluidex/awesome-plonk)
[Awesome ZKML](https://github.com/worldcoin/awesome-zkml)
[Awesome zkEVM](https://github.com/LuozhuZhang/awesome-zkevm)
[Awesome Zero Knowledge](https://github.com/ventali/awesome-zk)


#### STARK 

@xhhhh: 总结了下 Stark的一些资料，很多来自于@Kurt Pan ，感谢潘老师: 
- https://www.notion.so/Stark-Cannon-0801f24ea9e543449e015bf4063bb71d?pvs=4
- https://literate-wolfsbane-bf0.notion.site/Stark-Cannon-0801f24ea9e543449e015bf4063bb71d?pvs=4

Kurt Pan : 
https://github.com/erhant/zkbrainfuck A Brainfuck zkVM with Circom.

再推荐这几个，你可以去整理一下：
https://aszepieniec.github.io/stark-anatomy/
https://aszepieniec.github.io/stark-brainfuck/
https://github.com/facebook/winterfell
https://github.com/lambdaclass
https://github.com/andrewmilson/ministark/

- https://github.com/aszepieniec/stark-brainfuck/blob/master/docs/engine.md
	- 老外写个一个简单版本的zkvm，不过是基于stark的
	- 并且也配有文档，适合研究基于stark zkvm的原理理解。

实现一个 Baby Snark
- 可以参考 https://github.com/initc3/babySNARK 。可以看到Andrew Miller，Ye Zhang这些大佬几年前也是这么一步步成长起来的
#### zkml

 - https://github.com/worldcoin/awesome-zkml
 - https://github.com/socathie/circomlib-ml
 - https://github.com/socathie/nova-ml
 - https://github.com/lyronctk/zator

ZKML slides： https://www.canva.com/design/DAFi3o7FiR4/d9LMeacr6QQwYy9C0BQHgA/view

这篇 ZKML 文章很顶:  https://mirror.xyz/sevenxventures.eth/3USbrj7kcK7lyq_7upA4iyWV5pWMII7KrM40z5zpEXo

#### Others...

 - zk 摩尔定律: [zkintro](https://zkintro.com/)  
 -  [Nova: A New Chapter in Zero-Knowledge Proofs](https://huobi-ventures.medium.com/nova-a-new-chapter-in-zero-knowledge-proofs-108110cd816e)  (medium这糟糕的排版)
 - [A survey of ZK Languages](https://zkvalidator.com/a-survey-of-zk-languages/)
 - @Qi Zhou : 话说，我们在做zkWASM的Cannon，也计划用各种foding + aggregate的方案，感兴趣的小伙伴可以报名😄
 - 另一个zkwasm - supernova, 估计9-10月发布
	 - https://twitter.com/powdr_labs/status/1679822931340173313
	 - https://twitter.com/HoumanShadab/status/1679155719805755392
- risc0-nova
	- https://github.com/hero78119/risc0-nova
- zkGeth, 看起来是把 geth转化成risv64指令集，然后做一个zkrisc出来。
	- https://hackmd.io/@HqESr6_rQbmdCj2v03vrcQ/HyMA2pkmh 
- Towards a Nova-based ZK VM 的作者开始写的新书 : 
	- [zkintro](https://zkintro.com/) 
- 网上有比较好的用bellperson或者bellman写z kp的例子吗？
	- 具体一点 我觉得lurk的Poseidon 实现里有不错可以借鉴的 https://github.com/lurk-lab/neptune/blob/main/src/sponge/circuit.rs
	- 因为Nova用的是relaxed r1cs所以你也可以直接参考nova的examples比如 https://github.com/microsoft/Nova/blob/main/examples/minroot.rs , Constraint 部分都是一样的
	- 想找一些bellman 入门介绍快速上手，目前看到的最详细的就是 https://electriccoin.co/blog/bellman-zksnarks-in-rust/ 了
	- 仍然记得sinka（高老师）说学习框架最好写一个排序，当时他用的是halo2

- 一个非常简洁的stark介绍，比vitalik版本更加适合初学者:
	- Zero Knowledge Virtual Machine step by step https://eprint.iacr.org/2023/1032.pdf

- Boojum：zkSync的高性能去中心化STARK证明系统 https://blog.csdn.net/mutourend/article/details/131770996

- 可以订阅下mutourend邹老师的csdn，这技术热点追踪翻译速度才叫快，“区块链媒体”们望尘莫及😂

- 密码学纯理论进展速度也同样相当的惊人，
 - 清华毕业的Yanyi Liu跟着Rafael Pass 揪着OWF不放疯狂输出，试图彻底搞清楚密码学的复杂性理论基础。
 - Yilei Chen回国后负责清华姚班的Cryptography课 http://www.chenyilei.net/cryptography-s2023.html， 下一代姚班选手的ZKP就都是他教的了，他在FS要不要密码学哈希，iO/lattice方面也有很多工作。
 - Lijie Chen就更不用说了，华人TCS之光，博士论文写了551页https://www.mit.edu/~lijieche/Lijie-Chen-thesis.pdf，已经是average-case hardness 和 derandomization领域世界级的专家了，可以去预测哪年得图灵奖了。

- snarkVM有啊，Aleo家的就是 https://github.com/AleoHQ/snarkVM。没记错底层proof sys用的Marlin。
	- 可能program execution是面向冯诺伊曼架构RAM模型，而AIR相比于电路抽象层次更高，设计STARK VM更直接，所以大多数项目会这么选吧。
	- 不过zkEVM都用SNARK堆出来了，同样的工程师去堆一个自定义ISA的VM对他们来说应该是个simpler task吧

Nova VM 来了, 看不过来了。。。。
- [Towards a Nova-based ZK VM](https://zkresear.ch/t/towards-a-nova-based-zk-vm/105)

一个华人团队搞的 zkwasm
 - https://github.com/DelphinusLab/zkWasm

我在找一个write a vm from scratch的课程
 - 要是只是vm不要求zkvm那就太多了，比如5天前的这篇 https://andreabergia.com/blog/2023/07/i-have-written-a-jvm-in-rust/

- [Foundations of Data Availability Sampling](https://eprint.iacr.org/2023/1079.pdf)
- [Boojum Upgrade: zkSync Era’s New High-performance Proof System for Radical Decentralization](https://zksync.mirror.xyz/HJ2Pj45EJkRdt5Pau-ZXwkV2ctPx8qFL19STM5jdYhc)

- https://github.com/aszepieniec/stark-brainfuck/blob/master/docs/engine.md
	- 老外写个一个简单版本的zkvm，不过是基于stark的
	- 并且也配有文档，适合研究基于stark zkvm的原理理解。

https://slush.dev/   tendermint + Cairo vm  定制L3 as service
https://0xparc.org/blog/parc-squad 这个有点意思



关于 20 年以后 zk 的综述, pan 老师有推荐吗 : 
- 20年以后形势就是社区为王，不少好东西先发hackmd了。综述当然也有不少，但都是更细分领域的了，比如 https://eprint.iacr.org/2023/671 ， https://eprint.iacr.org/2023/857  等等

（发现和另一个研究群人员不overlap再发一遍）过了一遍HyperNova和Customizable constraint systems，感觉这个工作非常让人兴奋。CCS用来统一R1CS AIR PLONKish，然后用Nova IVC的方式multi-folding生成SNARK。有一种ZK界LLVM的即视感。各位老师怎么看。

### Tools
 - [Youtube 字幕神器](https://github.com/zkp-co-learning/zkp-co-learn/discussions/11)
 - [常用的希腊字母及其发音](https://github.com/zkp-co-learning/zkp-co-learn/discussions/91)
 - 