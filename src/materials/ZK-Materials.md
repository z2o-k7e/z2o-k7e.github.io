> - Last update: 2023-10-23 [^1]
> - 📢 Feel free to contribute!  Pls fork [zkp-co-learning.github.io](https://github.com/zkp-co-learning/zkp-co-learning.github.io) and PR! 
> - PS: In each title's Content Block, the top part is newer, below is older.


[TOC]

# ZKP building blocks

## Cryptography

《公钥密码学研究方法论》[https://documents.uow.edu.au/~fuchun/methodology.html]
- 这是 slides，这是最新的本体： [https://eprint.iacr.org/2023/715


- 密码学纯理论进展速度也同样相当的惊人，
 - 清华毕业的Yanyi Liu跟着Rafael Pass 揪着OWF不放疯狂输出，试图彻底搞清楚密码学的复杂性理论基础。
 - Yilei Chen回国后负责清华姚班的Cryptography课 http://www.chenyilei.net/cryptography-s2023.html， 下一代姚班选手的ZKP就都是他教的了，他在FS要不要密码学哈希，iO/lattice方面也有很多工作。
 - Lijie Chen就更不用说了，华人TCS之光，博士论文写了551页https://www.mit.edu/~lijieche/Lijie-Chen-thesis.pdf，已经是average-case hardness 和 derandomization领域世界级的专家了，可以去预测哪年得图灵奖了。

**Curves**

- [The Pasta Curves for Halo 2 and Beyond](https://electriccoin.co/blog/the-pasta-curves-for-halo-2-and-beyond/)
- [Mina Book: Pasta Curves](https://o1-labs.github.io/proof-systems/specs/pasta.html)
- [Pallas and Elliptic Curves](https://medium.com/asecuritysite-when-bob-met-alice/pallas-and-elliptic-curves-f34115904b02)
- [A survey of elliptic curves for proof systems](https://eprint.iacr.org/2022/586.pdf)
- [Revisiting cycles of pairing-friendly elliptic curves](https://eprint.iacr.org/2022/1662.pdf)
- [Pairings in Rank-1 Constraint Systems](https://eprint.iacr.org/2022/1162.pdf)
- https://github.com/zcash/pasta_curves
- https://hackage.haskell.org/package/pasta-curves-0.0.1.0/docs/PastaCurves.html
- https://github.com/nccgroup/pasta-curves
- https://docs.rs/ark-pallas/0.3.0/ark_pallas/

## Math.

Probability and Measure https://www.youtube.com/playlist?list=PL0vEWJI_pj7RZ51zecINlzWxpFv83r8RE
还有这个，我快看完了，超喜欢

- Abstract Algebra I (full course) https://www.youtube.com/playlist?list=PL1r259iKOz-lJ0TjURlrUVASXQimpWAS5

- [The-Art-of-Linear-Algebra](https://github.com/kenjihiranabe/The-Art-of-Linear-Algebra)
- [Animation vs. Math](https://www.youtube.com/watch?v=B1J6Ou4q8vE)

[Succinct Proofs and Linear Algebra](https://angeris.github.io/papers/zk-linalg.pdf)


数学 : 
- 参考学习内容:多项式乘法和除法、拉格朗日插值、Schwartz-Zip-pel 引理、快速傅立叶变换(FFT) 、NTT、MSM 以及 Field extension等。
- An Introduction to Mathematical Cryptography
- https://explained-from-first-principles.com/number-theory 这个介绍数论的也不错

初等数论和群论 : 
- 推荐了 MIT’s 6.875 (Foundations of Cryptography) ，对于国内的同学来讲，建议结合潘承洞版的看，会更好！ 教材链接：[https://mit6875.github.io/HANDOUTS/numbertheory.pdf](https://t.co/kfIc1fO7gV)

## Rust 

https://github.com/evcxr/evcxr/tree/main/evcxr_jupyter yeah, a Jupyter kernel for Rust

```bash
cargo install evcxr_jupyter
evcxr_jupyter --install
jupyter lab
```


# ZKP Basics Tutorials

这个课也值得跟上：
PSE Lectures - A full course on Elliptic Curve Cryptography
https://www.youtube.com/playlist?list=PLV91V4b0yVqQ_inAjuIB5SwBNyYmA9S6M

新火公开课 https://space.bilibili.com/3493266041342842/channel/series
钟博的课

- https://www.rareskills.io/zk-bootcamp
- https://zkhack.dev/whiteboard/ 挺适合作为基础资料的
- https://www.rareskills.io/zk-book

I try not to recommend too many sources, since everyone learns differently. But here are a few:  
- Least Authority’s [Moonmath manual](https://leastauthority.com/community-matters/moonmath-manual/) for learning Snarks. Starts with the basics.  
- For learning about Starks, StarkWare’s [Stark 101 series](https://starkware.co/stark-101/).  
- For writing some ZK code and seeing what actually happens: [Noir from Aztec](https://docs.aztec.network/noir)

- a16z 整理的比较全的 ZKP 的资料库  https://a16zcrypto.com/zero-knowledge-canon/
- ZK 零知识: https://buidlerdao.notion.site/ZK-5963083942a744bbb60a0328008868e2
- 零基础学习 ZK:  https://mirror.xyz/searchblock.eth/y11EKtXAtK3aXRVMV1yYqw7FibKHxI0fK10vlVRDaD4
- 万字长文捕获 ZK Rollup 时代价值｜ZONFF Research: https://mp.weixin.qq.com/s/5zKdS-GL8w_z4XIDOjv7FA
-  [zksync开源](https://github.com/matter-labs/zksync-era)

怎么零基础学习零知识证明:
- https://mirror.xyz/searchblock.eth/y11EKtXAtK3aXRVMV1yYqw7FibKHxI0fK10vlVRDaD4

## [Awesome ZKP](https://github.com/matter-labs/awesome-zero-knowledge-proofs) 

- [Awesome Noir](https://github.com/noir-lang/awesome-noir)
- [Awesome Circom](https://github.com/arnaucube/awesome-circom)
- [Awesome Aleo](https://github.com/howardwu/awesome-aleo)
- [Awesome Cairo](https://github.com/auditless/awesome-cairo)
- [Awesome-Folding](https://github.com/lurk-lab/awesome-folding)
- [Awesome HALO2](https://github.com/adria0/awesome-halo2)
- [Awesome PLONK](https://github.com/fluidex/awesome-plonk)
- [Awesome ZKML](https://github.com/worldcoin/awesome-zkml)
- [Awesome zkEVM](https://github.com/LuozhuZhang/awesome-zkevm)
- [Awesome Zero Knowledge](https://github.com/ventali/awesome-zk)

# Nova / Folding schemes

nova 写的五子棋: https://www.zkconnect4.dev/

Nova 项目: https://github.com/microsoft/Nova/network/dependents

A Brief History of Folding Schemes https://arc.net/e/2831196C-9575-47A6-966E-B34DB840168E 
- Bootleproof-type IPA是「folding 前史」的一章

https://nova-browser-ecdsa-web.vercel.app/

Folding with ProtoGalaxy - Liam Eagen https://www.youtube.com/watch?v=SpkTvRia1EA

 - [Nova: A New Chapter in Zero-Knowledge Proofs](https://huobi-ventures.medium.com/nova-a-new-chapter-in-zero-knowledge-proofs-108110cd816e)  (medium这糟糕的排版)

Benchmark on recursion Plonky vs Nova https://github.com/nikkolasg/recursive-bench

白菜: 有对NOVA-ML 感兴趣的不，这个课题可能比较大，可以考虑组个支队了 😂  
- https://github.com/socathie/nova-ml
- awesome-zkml : https://github.com/worldcoin/awesome-zkml
- Team Novi (Nova):  https://0xparc.notion.site/Team-Novi-2d81bc06b0aa4c99b61a9ee06166b3b6
- risc0-nova
	- https://github.com/hero78119/risc0-nova
- [Parallelizing Nova](https://hackmd.io/@9MPvD9czTQS18zEjAERVAg/Hy-vKK04h)
 -  [Nova: A New Chapter in Zero-Knowledge Proofs](https://huobi-ventures.medium.com/nova-a-new-chapter-in-zero-knowledge-proofs-108110cd816e)  (medium这糟糕的排版)


# Halo2 

https://github.com/CPerezz/halo2_sumcheck 👀

https://github.com/axiom-crypto/halo2-wasm

https://github.com/zkCert/halo2-zkcert

[0xPARC Course Recordings, Slides, and Notes](https://0xparc.notion.site/Course-Recordings-Slides-and-Notes-90d0ddb5f9ee49f7a244dbbe60d563ff#549d7b6081e1412a96ac8951dddf9e75)

Taiko: https://github.com/taikoxyz/circuit-tools/tree/main
 - 我们从 PSE 的 zkevm 里抽象出了一个 sdk，大家如果想用 halo2 开发可以试下 [Grin] 欢迎给我提 issue

solidity verifier: https://github.com/privacy-scaling-explorations/halo2-solidity-verifier/pulls

halo2 tinyram  https://github.com/Orbis-Tertius/tiny-ram-halo2

- Poseidon: https://github.com/zcash/halo2/blob/main/halo2_gadgets/src/poseidon/pow5.rs
- proof of validator: https://github.com/asn-d6/halo2-merkle-tree-ipa-bench
- social recovery: https://hackmd.io/@Nerolation/H1BvRWg02

axiom 的 open source program

----

使用了 halo2 的 Applications: 

- ZK Email https://github.com/zkemail  halo2   
- ZK Wordle: https://zordle.xyz/  halo2
- Hammster： https://github.com/ytham/hammster  halo2
 - zk-draw : Verifiable random draw with zero-knowledge of the random seed https://github.com/jae-cuz/zk-draw   halo2
- ZK Microphone: https://github.com/Miyamura80/ZKMicrophone 
- [Building a Zero Knowledge web app with Halo 2 and Wasm (part 1)](https://medium.com/@yujiangtham/building-a-zero-knowledge-web-app-with-halo-2-and-wasm-part-1-80858c8d16ee)
- zk-img: Fighting Deepfakes with Zero-Knowledge Proofs   https://medium.com/@danieldkang/zk-img-fighting-deepfakes-with-zero-knowledge-proofs-9b76c23e3789  尚未开源

大部分由 @Kurt Pan 博士整理


# STARK 

深入探索STARK的安全性和可靠性——STARKs全面安全分析 https://blog.csdn.net/mutourend/article/details/133821797

- [A Walk-Through of a Simple zk-STARK Proof](imgs/A%20Walk-Through%20of%20a%20Simple%20zk-STARK%20Proof.pdf)

- Boojum：zkSync的高性能去中心化STARK证明系统 https://blog.csdn.net/mutourend/article/details/131770996

- 一个非常简洁的stark介绍，比vitalik版本更加适合初学者:
	- Zero Knowledge Virtual Machine step by step https://eprint.iacr.org/2023/1032.pdf

- https://github.com/aszepieniec/stark-brainfuck/blob/master/docs/engine.md
	- 老外写个一个简单版本的zkvm，不过是基于stark的
	- 并且也配有文档，适合研究基于stark zkvm的原理理解。

@0xhhh: 总结了下 Stark的一些资料，很多来自于@Kurt Pan ，感谢潘老师: 
- https://www.notion.so/Stark-Cannon-0801f24ea9e543449e015bf4063bb71d?pvs=4
- https://literate-wolfsbane-bf0.notion.site/Stark-Cannon-0801f24ea9e543449e015bf4063bb71d?pvs=4

Kurt Pan : 
https://github.com/erhant/zkbrainfuck A Brainfuck zkVM with Circom.

https://starkware.co/stark-101/ @Frank Jz Liu 推荐，亲测很不错

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

----


@Maxlion🦁 提供：

Cairo 环境配置➕ERC 实现（Cairo1）
https://starknetastro.xlog.app/Starknet_Shanghai_Workshop_DAY1

Cairo 语言中文文档
https://book.cairo-lang.org/zh-cn/index.html

开发工具版本号推荐
湘_Xiang 版本
https://w3hitchhiker.notion.site/Starknet-cc07119ed57648faa92f7630219225b3?pvs=4
鸦_Cryptonerdcn 版本
https://docs.google.com/document/d/1ehBfE2CBeVL9rDhSc_JiUtxZJKWVKl8A6o1f1Ee0X4M/mobilebasic

[Cairo合约示例]
Nethermind 版
https://starknet-by-example.voyager.online/
LambdaClass 版
https://cairo-by-example.com/
OpenZeppelin 版
https://github.com/OpenZeppelin/cairo-contracts/tree/cairo-2

Starknet 文档
https://book.starknet.io/

Starknet 基金会给 Astro 社区开发者的答疑文档
https://docs.google.com/document/d/1ku_y4M9PUe_GcwiBlvq6Kz2LkagyXD2Za1XCZj-IHVo/mobilebasic

Starknet 中文知识库(生态向)
https://starknet-astro.super.site/


# ZKML

[Zator: Verified inference of a 512-layer neural network using recursive SNARKs](https://github.com/lyronctk/zator)v

TensorPlonk: A “GPU” for ZKML, Delivering 1,000x Speedups https://medium.com/@danieldkang/tensorplonk-a-gpu-for-zkml-delivering-1-000x-speedups-d1ab0ad27e1c


 - https://github.com/worldcoin/awesome-zkml
 - https://github.com/socathie/circomlib-ml
 - https://github.com/socathie/nova-ml
 - https://github.com/lyronctk/zator

ZKML slides： https://www.canva.com/design/DAFi3o7FiR4/d9LMeacr6QQwYy9C0BQHgA/view

这篇 ZKML 文章很顶:  https://mirror.xyz/sevenxventures.eth/3USbrj7kcK7lyq_7upA4iyWV5pWMII7KrM40z5zpEXo


# zkVM

https://github.com/vacp2p/zk-explorations
 - 对不同 lib 和 algo 实现的 VM 做的性能测试 benchmark

发现了asz有好多好东西啊，stark很好的学习资料 https://github.com/aszepieniec/stark-anatomy

下一期共学STARK+zkVM的资料

https://neptune.cash/
[STARK 算法解析（第 6 部分: 加速整个流程）](https://mp.weixin.qq.com/s/nBfL0MzqzOxGlSZgw6I85Q)
https://asz.ink/alan-szepieniec/ he is an advisor to Nervos Foundation.
https://neptune.cash/
https://neptune.cash/learn/brainfuck-tutorial/

- snarkVM: Aleo 已经把 Marlin 更新成 Varuna 了 [varuna](https://github.com/AleoHQ/snarkVM/tree/testnet3/algorithms/src/snark/varuna)

[徒手写 zkVM  - https://eprint.iacr.org/2023/1032.pdf](https://eprint.iacr.org/2023/1032.pdf)
 - ZERO KNOWLEDGE VIRTUAL MACHINE STEP BY STEP
 - 稍微比较偏理论

- https://github.com/aszepieniec/stark-brainfuck/blob/master/docs/engine.md
	- 老外写个一个简单版本的zkvm，不过是基于stark的
	- 并且也配有文档，适合研究基于stark zkvm的原理理解。

另外他去年一篇zkvm的概览，对理解zkvm的设计还是很棒的。
他是指 https://aszepieniec.github.io/stark-brainfuck/ 的作者？
 - 基本的输入输出，内存等。我一直认为那个教程就是最好的zkvm入门的。

https://github.com/cryptape/ckb-bf-zkvm
秘猿 A BrainFuck zkVM implementation on CKB, using Halo2.
一个只有几条指令的 vm 机器


- snarkVM有啊，Aleo家的就是 https://github.com/AleoHQ/snarkVM。没记错底层proof sys用的Marlin。
	- 可能program execution是面向冯诺伊曼架构RAM模型，而AIR相比于电路抽象层次更高，设计STARK VM更直接，所以大多数项目会这么选吧。
	- 不过zkEVM都用SNARK堆出来了，同样的工程师去堆一个自定义ISA的VM对他们来说应该是个simpler task吧

Nova VM 来了, 看不过来了。。。。
- [Towards a Nova-based ZK VM](https://zkresear.ch/t/towards-a-nova-based-zk-vm/105)


我在找一个write a vm from scratch的课程
 - 要是只是vm不要求zkvm那就太多了，比如5天前的这篇 https://andreabergia.com/blog/2023/07/i-have-written-a-jvm-in-rust/

![](../未命名/ZK-Materials_image_1.jpg)
上周测试了一下 PSE evm circuit，生成的 raw proof 用 evm 验证需要大约 4000万+ gas。（本地调高了 block gas limit）
也测试了 scroll-prover 的 chunk proof，evm 验证大概需要 40万+ gas。感觉 gas fee 这块至少有 100 倍的以上的提升

chunk proof 里面包涵了 2次压缩，还是做了不少的工作

![](../未命名/ZK-Materials_image_2.jpg)

不太确定上面的代码是不是具体的电路，看了下感觉没多少行。[Facepalm]

zkevm super circuit 的 column 太多了，应该超过了 1000 个。
chunk 和 aggregation 的 column 少了很多，但是应该像 Frank 所说，电路 “平铺”在聚合电路里。不知道他们怎么优化到 40万 gas 的，这个水平已经和 circom 差不多了





# cutting-edge
## Lattice

Lattice-based cryptography: The tricky math of dots https://www.youtube.com/watch?v=QDdOoYdb748&list=PLMItfTVgwEAvTX4-sZkcF5s3-l1JDocY0&index=4

## Quantum

[Quantum Resistance and the Signal Protocol](https://signal.org/blog/pqxdh/)
 - Signal 已经集成 Kyber了


# ZK Applications

## ZK Email

最近有进展，确实值得研究：ZK Email https://github.com/zkemail

ZK Proof of Email：通往decentralized identity之路 https://blog.csdn.net/mutourend/article/details/129004763?ops_request_misc=&request_id=2425a5a6d21c4b6bbe440828ba478ccf&biz_id=


## zkWASM

[Exploring alternatives to WASM for smart contracts](https://forum.polkadot.network/t/exploring-alternatives-to-wasm-for-smart-contracts/2434/21)

一个华人团队搞的 zkwasm
 - https://github.com/DelphinusLab/zkWasm


 - 另一个zkwasm - supernova, 估计9-10月发布
	 - https://twitter.com/powdr_labs/status/1679822931340173313
	 - https://twitter.com/HoumanShadab/status/1679155719805755392


```
【zkwasm 分享总结】： 
程序泡在 webassembly 的 bytecode 里面。是一个 webassenmbly 的小程序。
不分链游的问题：游戏运行的结果有可能作弊，不知道你是怎么运行这个程序的，有可能你上传了一些裸数据。 所以仍依靠中心化的服务器

会在跑 webassembly 的时候跑一个，监听用户事件，产生输入输出的 sequence，把他编译成一个 webassembly 的文件。每个操作带来的 consequence 会被管控的，把 proof 上链。

how to generate proof：
- Prover 实例
- 不用每次都和链上交互（gas 太高），可以把本次所有的行动都压成一个 proof。

https://github.com/DelphinusLab/zkWasm

如果想 onchain Verify，需要 Deploy 一个测试合约

合约生成，prove 加速：
- https://github.com/DelphinusLab/continuation-batcher

https://hackmd.io/@sinka/BJUIyufEc

后续：模块化公开课
```



## **PSE**


From CEX to CCEX with Summa
https://mirror.xyz/privacy-scaling-explorations.eth/_1Y6ExFD_Rs3oDxwx5_kWAj_Tl_L9c0Hm7E6SVJei0A

https://mirror.xyz/privacy-scaling-explorations.eth/f2ZfkPXZpvc6DUmG5-SyLjjYf78bcOcFeiJX2tb2hS0

PSE Lectures - Ep 6 https://www.youtube.com/watch?v=l7bEN1V7qRM

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





# Tricks & Tools

https://github.com/joelparkerhenderson/sha256-sentence
- The SHA256 for this sentence begins with seven, seven, f, zero, a, b, b and five.
- The SHA256 hash of this message begins with 534d765
- 我去，厉害了,主要是里面没有什么随机数

https://zkbench.dev/
trade-offs and performance of different frameworks.

[A survey of ZK Languages](https://zkvalidator.com/a-survey-of-zk-languages/)

# books & websites & Podcasts..🎙📚

https://blog.lambdaclass.com/ 要经常看

https://blog.csdn.net/mutourend -- 简中唯一zkp-complete博客[Grin]
- 订阅下 mutourend 邹老师的csdn，这技术热点追踪翻译速度才叫快，“区块链媒体”们望尘莫及😂

- 去年出版的《哥德尔传》 <https://book.douban.com/subject/36073022/> 是我近些年读到写得最好的传记，
- 仅次于我十年前读到的《维特根斯坦传》 <https://book.douban.com/subject/6152040/> （这是不可能被撼动的第一传记神作）

- [The arithmetic of pairing-based proof systems](imgs/ZK-Materials_pdf_1.pdf)

三本叙事书籍 : pnp / tns / cw

 - [p ?= np](./imgs/pnp.pdf)
 - [Crypto Wars](./imgs/cw.pdf)
 - [The network state](./imgs/The-network-state.pdf)

https://cryptography.rs/ 
这个页面很全也很漂亮，我cryptography.land也是受这里启发

刚才CJ还提到了这个网站 https://asecuritysite.com/ ，我也经常看这个，可以感到这个教授对密码学是真爱，做得非常全面且扎实

----

🎙
老牌密码学播客 ： Security Cryptography Whatever https://securitycryptographywhatever.com/
[Episode 288: Quantum Cryptography with Or Sattath](https://zeroknowledge.fm/288-2/)


## zkp info flow

https://twitter.com/i/spaces/1ynJOaMnBlOKR
 - [zkp twitter space 202309201217 ](./imgs/202309201217-zkp-twitter-space.pdf)
https://www.proofoftrack.xyz/
还有Suning的newsletter，小伙子精力旺盛，新link整理得很快

郭老师推： https://twitter.com/1dot2
邹老师博： https://blog.csdn.net/mutourend
论文看我的这个： https://zk.cryptography.land/ 
zkmesh: https://zkmesh.substack.com/
自定义Twitter Threads：比如我这个 https://twitter.com/i/lists/1522499398210531329
项目方的博客（这个很多，看几篇看对不对胃口，调整）
Youtube：PSE/a16z/Eth Global etc
podcast: https://zeroknowledge.fm/

以上所有内容（包括Twitter List）均可RSS订阅，且建议使用RSS订阅

项目程序一手信息几乎唯一的选择：Github
论文的话 https://eprint.iacr.org/ 
又想起来这个重要的标准化组织（很少被提起但里面有好东西）： https://zkproof.org/
比如  https://docs.zkproof.org/presentations
沙漏老师的feed : https://github.com/PrimitivesLane/PrimitivesFeed
一个中科院数学所的学生做的密码学链接聚合网  https://link.fffmath.com/
泛web3终极聚合器RAW： https://twitter.com/nake13/status/1526852356402745346

forum:
https://ethresear.ch/
https://zkresear.ch/


# Interdisciplinarity(e.g. AI)

AI:
- [A hacker's guide to Language Models(1)](https://github.com/fastai/lm-hackers/blob/main/lm-hackers.ipynb)
- [A hacker's guide to Language Models(2)](https://www.youtube.com/watch?v=jkrNMKz9pWU)


# Others (can't archive)

当时逐字听译的一个 Brendan 的 talk: Plonky2 简介 https://mp.weixin.qq.com/s/qSWFLQPQJvWHclAvlEXEaQ

Q: 我想知道 这个 recursive 实际能跑起来的例子哪里有[Lol]
A:  Plonky2-based的吗？olavm肯定能实际跑起来 https://github.com/Sin7Y/olavm


[Eos: Efficient Private Delegation of zkSNARK Provers](https://www.usenix.org/conference/usenixsecurity23/presentation/chiesa)

Lurk Beta 估计还得半年以上 https://github.com/lurk-lab/lurk-rs/issues/657

ABCDE ZK Hacker Camp |【Session 8: Efficient Zero-KnowledgeProofs: Theory and Practice】 https://www.youtube.com/watch?v=j2_9nwgfhEw

 - https://anoma.github.io/VampIR-Book/
 - zk 摩尔定律: [zkintro](https://zkintro.com/)  
 - @Qi Zhou : 话说，我们在做zkWASM的Cannon，也计划用各种foding + aggregate的方案，感兴趣的小伙伴可以报名😄
- risc0-nova
	- https://github.com/hero78119/risc0-nova
- zkGeth, 看起来是把 geth 转化成risv64指令集，然后做一个zkrisc出来。
	- https://hackmd.io/@HqESr6_rQbmdCj2v03vrcQ/HyMA2pkmh 
- Towards a Nova-based ZK VM 的作者开始写的新书 : 
	- [zkintro](https://zkintro.com/) 

网上有比较好的用bellperson或者bellman写 zkp的例子吗？
- 具体一点 我觉得lurk的Poseidon 实现里有不错可以借鉴的 https://github.com/lurk-lab/neptune/blob/main/src/sponge/circuit.rs
- 因为Nova用的是relaxed r1cs所以你也可以直接参考nova的examples比如 https://github.com/microsoft/Nova/blob/main/examples/minroot.rs , Constraint 部分都是一样的
- 想找一些bellman 入门介绍快速上手，目前看到的最详细的就是 https://electriccoin.co/blog/bellman-zksnarks-in-rust/ 了
- 仍然记得sinka（高老师）说学习框架最好写一个排序，当时他用的是halo2


- [Foundations of Data Availability Sampling](https://eprint.iacr.org/2023/1079.pdf)
- [Boojum Upgrade: zkSync Era’s New High-performance Proof System for Radical Decentralization](https://zksync.mirror.xyz/HJ2Pj45EJkRdt5Pau-ZXwkV2ctPx8qFL19STM5jdYhc)


https://slush.dev/   tendermint + Cairo vm  定制L3 as service
https://0xparc.org/blog/parc-squad 这个有点意思


关于 20 年以后 zk 的综述, pan 老师有推荐吗 : 
- 20年以后形势就是社区为王，不少好东西先发hackmd了。综述当然也有不少，但都是更细分领域的了，比如 https://eprint.iacr.org/2023/671 ， https://eprint.iacr.org/2023/857  等等

（发现和另一个研究群人员不overlap再发一遍）过了一遍HyperNova和Customizable constraint systems，感觉这个工作非常让人兴奋。CCS用来统一R1CS AIR PLONKish，然后用Nova IVC的方式multi-folding生成SNARK。有一种ZK界LLVM的即视感。各位老师怎么看。

# zkp QA 

Question Lists: 
- zkp 的前端？后端？算数化？ 
- 什么是 MSM 加速？ 
- P!=NP ?
- SumCheck 没有办法转成非交互式的zk 证明吧？

----

zkp 的前端？后端？算数化？ 我感觉应该是后端 需要@Kurt Pan 给出一个 前端 后端的定义

- https://a16zcrypto.com/posts/article/measuring-snark-performance-frontends-backends-and-the-future/

有两种解释：
1. 前端=算术化/后端=证明系统
2. 前端=密码学编译器（比如多项式承诺）/后端=信息论证明系统（比如IOP）

-----

什么是 MSM 加速？ 

以groth16为例，涉及7次NTT：4个INTT ，5次msm：其中1次G2的MSM 。
整体Prover消耗上，MSM应该占据了70%多吧. NTT应该占据10-20%。

----

P!=NP

```
刚郭老师说有人想听关于P vs NP问题的内容，这里刚好有一篇很新超棒的科普文章。我明天10:30可以给大家过一下这篇文章：
https://www.quantamagazine.org/complexity-theorys-50-year-journey-to-the-limits-of-knowledge-20230817/

“If you believe in hardness, then you should believe that it’s hard to prove hardness,” “why it had been so hard to prove that this seemingly hard problem about computational hardness was actually hard”

“which intuitively seem harder and thus are perhaps easier to prove hard.”

“complexity theory is itself complex” 看完了，这篇过于震撼

研究PRG，研究succinctness，研究NIZK，研究secret sharing，这些密码学研究全都反过来都跟证明P不等于NP密切相关。再反过来又可以做到直接基于P不等于NP假设（而不是存在单向函数）的密码学。

- https://www.quantamagazine.org/the-cryptographer-who-ensures-we-can-trust-our-computers-20230727/
- https://www.quantamagazine.org/a-short-guide-to-hard-problems-20180716/
- [P vs. NP and the Computational Complexity Zoo](https://www.youtube.com/watch?v=YX40hbAHx3s)
- https://www.quantamagazine.org/complexity-theorys-50-year-journey-to-the-limits-of-knowledge-20230817/


单向函数存在（P不等于NP），则存在对NP的ZKP；单向函数不存在（且P=NP），则也存在对NP的ZKP。

前半句是GMW86原话。后半句：如果P=NP，又因为对P永远存在无条件的ZKP system，所以也存在无条件的对NP的证明系统。

所以ZKP恰恰是少数无论P是否等于NP都存在的密码学协议。OTP也是。还有所有的「信息论安全密码学」。而大多数需要单向函数的密码协议，都活在Minicrypt之后。

这个才是需要展开说说的。但上次随便一讲都讲了两个半小时，还只讲了一条线，耽误大家吃饭都[Facepalm]

所以是不是可以理解为，只要有P问题的存在，就存在密码学协议
我不知道密码学协议的定义，所以没法讨论

对P问题的零知识证明系统trivial的无条件存在，这个证明系统就一个特点：完全无视prover

赶紧学lattice吧
```


----

请教大家一个问题，SumCheck 没有办法转成非交互式的zk 证明吧？

如果不强调非交互可以看这篇 https://arxiv.org/pdf/1704.02086.pdf 
如果强调非交互，就要小心绕过一个结论：一个零知识的public coin的交互式协议是不能应用Fiat-Shair转换为非交互的，soundness不能保证。
- 这句话难懂，pan老师这个有文章说明么

例子应该是 Barak01 的 non-blackbox simulation里的协议。上次Deng Yi老师讲的
就是说过早达到 zk 性质对 fs 并非好事

我的理解在 sumcheck 中，每一轮需 verifier 提供随机数给 prover，prover 计算 $g_i(x)$ 并发给verifier，这个计算 $g_i(x)$ 的过程 verifier 做不了，所以必须要交互
 - 没有让verifier做prover工作的。转非交互的思路是把verifier的工作（发随机数）让prover做（用哈希）。
- 非交互这个词也不好，非交互不是没有交互，而是单向一轮交互
- 在 sumcheck 的 step3 Verifier 
 checks that the partial sum and total sum agree when the partial sum is evaluated at 0 and 1 and its outputs added。
 - 按照 https://semiotic.ai/articles/sumcheck-tutorial/ 中的描述只有verifier 验证partial sum 和total sum 的结果才能发送随机数



[^1] 巨量资料 FYI，大部分出自网络及群聊整理，时间有限出处 没有做细致排版，请恕无法极尽详实覆盖，如需加上出处请联系作者 ~
