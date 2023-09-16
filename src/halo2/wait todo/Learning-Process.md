
 - [x] [安比实验室 Lookup Gates](https://github.com/sec-bit/learning-zkp/blob/develop/plonk-intro-cn/plonk-lookup.md)
 - [ ] [论文: Recursive Proof Composition without a Trusted Setup](https://eprint.iacr.org/2019/1021.pdf)
 - [ ] [Awesome halo2!](https://github.com/adria0/awesome-halo2)

#### The Halo2 Book:
  - https://zcash.github.io/halo2/
  - https://trapdoor-tech.github.io/halo2-book-chinese/  中文翻译
  - https://github.com/zcash/halo2/blob/main/book/   书籍 Github
 - [x] [Concepts](https://zcash.github.io/halo2/concepts.html)
 - [x] [User Documentation](https://zcash.github.io/halo2/user.html)
 - [x] [Proving system](https://zcash.github.io/halo2/design/proving-system.html)
 - [ ] [Protocol Description](https://zcash.github.io/halo2/design/protocol.html)
 - [ ] [Implementation](https://zcash.github.io/halo2/design/implementation.html)
 - [ ] [Gadgets](https://zcash.github.io/halo2/design/gadgets.html)
	 - [ ] SHA256
	 - [ ] ECDSA

#### 0xPARC: Halo2 Learning Group
 - Official : https://learn.0xparc.org/halo2/
 - https://github.com/icemelon/halo2-tutorial/
 - [x] 1. Introduction 8.21 
 - [x] [2. Halo2 API & Building a Basic Fibonacci Circuit (Part 1)]()  
	 - [x] https://github.com/icemelon/halo2-examples  **(Haicheng's Code)**
	 - [x]  https://github.com/enricobottazzi/halo2-fibonacci-ex  整理了 0XPARC 课上的 QA !  and Example2 Example 3 ...
	 - [x] https://github.com/therealyingtong/halo2-hope  code reference
	 - [x] https://github.com/jasonmorton/halo2-examples/blob/master/src/range_check/example1b.rs   Jason Morton 超详细注释 line by line
	 - [ ] https://github.com/enricobottazzi/halo2-intro
 - [x] 3. [Fibonacci Circuits cont. + Basic Halo2 gadgets](https://www.youtube.com/watch?v=YclxoJ069zg&t=1695s)
	 - [ ] Jason Morton Code missed Haicheng's `print` part.
 - [x] 4. [Halo2 Learning Group: Circuit Exercises 1](https://www.youtube.com/watch?v=DThChBlQQ6Y)
 - [x] 5. [Halo2 Learning Group: Circuit Exercises 2](https://www.youtube.com/watch?v=PVA4kryrmAg&t=6s)
 - [ ] Fibonacci & Lookup table ? in Polygon ZKEVM Course
 - [ ] [Halo2 API & Building a Basic Fibonacci Circuit (Part 2)](https://www.youtube.com/watch?v=YclxoJ069zg)

#### StarLi 星想法
 - [x] [01 - Halo2入门基础介绍 StarLI-Trapdoor](https://www.bilibili.com/video/BV1ML4y1M7iV)
 - [x] [02 - Halo2 深入理解Permutation & Lookup算法](https://www.bilibili.com/video/BV1C34y1t7pN)
 - [x] [03 - Halo2协议基础及介绍](https://www.bilibili.com/video/BV19L4y1T7ai)    25' 左右对整体协议的介绍
 - [x] [04 - Halo2电路进阶 (sha256 优化实现)](https://www.bilibili.com/video/BV1LL411P7ba)
	 - [ ] 如何实现 (mod  $2^{32}$ ) 加法 ? 
	 - [ ] 如何实现 `XOR`  (有些 代码很 make sense ) 值得再看
 - [x] [05 - Halo2源代码导读](https://www.bilibili.com/video/BV1HS4y1D7tX)
	 - [x] https://mp.weixin.qq.com/s/VerLN8-tqetKs1Hv6m4KLg
 - [ ] [Halo2 开发常见错误 (Custom Gate)](https://mp.weixin.qq.com/s?__biz=MzU5MzMxNTk2Nw==&mid=2247488089&idx=1&sn=0eb6b7e6ae1363522029fd8b592c5e57&chksm=fe130349c9648a5f1f376766c08b08fe6319949b14c6130909484cbe0cf8936edd7890063de9&cur_album_id=1458661849167511555&scene=190#rd)

#### 知乎小白专栏: 
 - [零知识证明原理及区块链应用分享](https://www.zhihu.com/column/c_1170346974367916032)
 - [x] [Plookup 原理详解](https://zhuanlan.zhihu.com/p/366693663)     **扩展到multiple tables** 部分 make sense, 但是公式太不清晰了, 可以重新回头看
 - [x] [Halo2: Lookup argument](https://zhuanlan.zhihu.com/p/447736247)  主要是 halo2 book 的 Proving System 部分的讲解,   zero knowledge 部分很 make sense, 值得再读
 - [x] [Halo2：原理剖析](https://zhuanlan.zhihu.com/p/385134321)    简易大纲
 - [ ] [V神如何巧妙讲解](https://zhuanlan.zhihu.com/p/429253463)

#### DoraHacks

 - [x] [零知识证明：Halo2解读](https://www.bilibili.com/video/BV1qk4y1j71i)    乏善可陈...

#### Code

 - [x] https://github.com/Divide-By-0/halo2-learning-group
 - [ ] [Halo2 Tutorial Intro](https://erroldrummond.gitbook.io/halo2-tutorial)
	 - [ ] 提供了 ECDSA / 等 gadgets , 值得再读 ...
	 - [ ] 提供了  Range check 等 gadgets , 值得再读 ...
 - [x] [Develop Circuits Using Halo 2](https://medium.com/@ola_zkzkvm/sin7y-tech-review-develop-circuits-using-halo-2-829e2f26856)    对 ZCash $a^2 + b^2 = c$  的 Tutorial , 图解非常好, 搭配 ZCash 的翻译一起看
 - [x] [Building a Zero Knowledge web app with Halo 2 and Wasm (part 1)](https://medium.com/@yujiangtham/building-a-zero-knowledge-web-app-with-halo-2-and-wasm-part-1-80858c8d16ee)
	 - [ ] Very Goods！
 - [ ] [zordle](https://github.com/nalinbhardwaj/zordle)  讲解清晰: 
 - [ ] [Awesome HALO2](https://github.com/adria0/awesome-halo2)
 - [ ] [halo2 tutorial From HaiCheng Shen](https://github.com/icemelon/halo2-tutorial)  沈海诚 code


Others :
 - [ ] [ZKSwap 团队详细解读——Halo2 原理](https://www.geekmeta.com/article/3704425.html)
 - [ ] [Zero knowledge 2 Zero knowledge](https://mirror.xyz/searchblock.eth/y11EKtXAtK3aXRVMV1yYqw7FibKHxI0fK10vlVRDaD4)
 - [ ] [EF grants halo2 Out of date.](https://github.com/EDGDrummond/EF-grant)
 - [ ] YouTube Series : 
	 - [ ] [zkEVM Audit Education Sessions 1/4 -Circuit Arithmetization for ZKP](https://www.youtube.com/watch?v=ofDcWqZAGK4)   workflow of halo2
	 - [ ] [zkEVM Audit Education Sessions 2/4 -Fibonacci examples for PIL & Halo2](https://www.youtube.com/watch?v=luMnF1fHeIk)
	 - [ ] [zkEVM Audit Education Sessions 3/4 - Architecture and Workflow](https://www.youtube.com/watch?v=qbR5pwmRxKY)
	 - [ ] [zkEVM Audit Education Sessions 4/4 - zkEVM Auditing & Lessons Learned](https://www.youtube.com/watch?v=gCKh6ySXYTU)
 - [ ] [Sin7Y Tech Review: Develop Circuits Using Halo 2](https://medium.com/@ola_zkzkvm/sin7y-tech-review-develop-circuits-using-halo-2-829e2f26856#)


论文: BCH19 Recursive proof composition without a trusted Setup


现在我们有一个质押对赌的程序。他的逻辑大概这样：
- 比如用户 a 为 flag_1 下注100美元。比如他下注是 flag_1 - Done 
- 然后用户 b 和 c 下注的是分别是 1 美元和 9 美元，他们下注的是 flag_1 - Failed

用户身份：
- 用户 a 是 flag 的承诺人，通常只有 1 个，他自己质押 flag_1 的状态为 Done 来激励自己，克服拖延
- 用户 b、c、d ... 是 flag 的对赌人，通常可以是 0 ~ n  (n 无上限)，对赌人赌 flag 的状态是 Failed

这个游戏是这么玩的，每个 flag 都有 2 种状态：Done 和 Failed：
1. 假如 flag_1 最后的状态是 Done。那么用户 a 拿走所有对赌用户的钱，比如在上面这个例子中，他就把 b 和 c 的 1 美元和 9 美元都拿走了
2. 假如 flag_1 最后的状态是 Failed ，那么用户 a 的赌注就会被所有的投注用户瓜分，这里引入一个杠杆，假设杠杆率设置为 5，那么用户 b 就能拿走 1×5=5 美元，用户 c 就能拿走 9×5 = 45 美元。剩下的100 - 5 - 45 = 50 美元进入到国库。
3. 在 状态是 Failed 场景中，即使对赌用户投注的钱再多，他最多只能拿走不超过 100 美元的奖励。

我的问题是，这里的杠杆率设置似乎不太合理，因为用户 a 即使下注 Done 且赌对，他也最多只能拿走十美元，但是用户 b 和 c 的杠杆率都是 5 倍。

你能否帮我优化一下这个杠杆场景，使得不论对flag_1 的状态押注是怎样的，能够保证两方都愿意为其下注。