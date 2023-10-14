[TOC]
#### Prerequisite
 - [x] [PLONK Tutorials](https://learn.z2o-k7e.world/plonk-intro-cn/plonk-arithmetization.html) &  [Lookup Gates @secbit](https://github.com/sec-bit/learning-zkp/blob/develop/plonk-intro-cn/plonk-lookup.md)
 - [x] [Rust 圣经](https://course.rs/about-book.html)

#### The Halo2 Book:
  - [zcash halo2 book](https://zcash.github.io/halo2/)
	  - [github](https://github.com/zcash/halo2/blob/main/book/)
	  - [Gadgets](https://zcash.github.io/halo2/design/gadgets.html)
  - [halo2 book 中文翻译](https://trapdoor-tech.github.io/halo2-book-chinese/)
	  - [github](https://trapdoor-tech.github.io/halo2-book-chinese/)

#### 0xPARC: Halo2 Learning Group
 - Official : https://learn.0xparc.org/halo2/
 - https://github.com/icemelon/halo2-tutorial/
 - [x] 1. Introduction 8.21 
 - [x] [2. Halo2 API & Building a Basic Fibonacci Circuit (Part 1)]()  
	 - [x] https://github.com/icemelon/halo2-examples  **(Haicheng's Code)**
	 - [x]  https://github.com/enricobottazzi/halo2-fibonacci-ex  整理了 0XPARC 课上的 QA !  and Example2 Example 3 ...
	 - [x] https://github.com/therealyingtong/halo2-hope  code reference
	 - [x] https://github.com/jasonmorton/halo2-examples/blob/master/src/range_check/example1b.rs   Jason Morton 超详细注释 line by line
	 - [x] https://github.com/enricobottazzi/halo2-intro
 - [x] [Fibonacci Circuits cont. + Basic Halo2 gadgets](https://www.youtube.com/watch?v=YclxoJ069zg&t=1695s)
	 - [x] Jason Morton Code missed Haicheng's `print` part.

#### StarLi 星想法
 - [x] [01 - Halo2入门基础介绍 StarLI-Trapdoor](https://www.bilibili.com/video/BV1ML4y1M7iV)
 - [x] [02 - Halo2 深入理解Permutation & Lookup算法](https://www.bilibili.com/video/BV1C34y1t7pN)
 - [x] [03 - Halo2协议基础及介绍](https://www.bilibili.com/video/BV19L4y1T7ai)    25' 左右对整体协议的介绍
 - [x] [04 - Halo2电路进阶 (sha256 优化实现)](https://www.bilibili.com/video/BV1LL411P7ba)
	 - [x] 如何实现 (mod  $2^{32}$ ) 加法 ? 
	 - [x] 如何实现 `XOR`  (有些 代码很 make sense ) 值得再看
 - [x] [05 - Halo2源代码导读](https://www.bilibili.com/video/BV1HS4y1D7tX)
	 - [x] https://mp.weixin.qq.com/s/VerLN8-tqetKs1Hv6m4KLg
 - [x] [Halo2 开发常见错误 (Custom Gate)](https://mp.weixin.qq.com/s?__biz=MzU5MzMxNTk2Nw==&mid=2247488089&idx=1&sn=0eb6b7e6ae1363522029fd8b592c5e57&chksm=fe130349c9648a5f1f376766c08b08fe6319949b14c6130909484cbe0cf8936edd7890063de9&cur_album_id=1458661849167511555&scene=190#rd)

#### 知乎小白专栏: 
 - [零知识证明原理及区块链应用分享](https://www.zhihu.com/column/c_1170346974367916032)
 - [x] [Plookup 原理详解](https://zhuanlan.zhihu.com/p/366693663)     **扩展到multiple tables** 部分 make sense, 但是公式太不清晰了, 可以重新回头看
 - [x] [Halo2: Lookup argument](https://zhuanlan.zhihu.com/p/447736247)  主要是 halo2 book 的 Proving System 部分的讲解,   zero knowledge 部分很 make sense, 值得再读
 - [x] [Halo2：原理剖析](https://zhuanlan.zhihu.com/p/385134321)    简易大纲
 - [x] [V神如何巧妙讲解](https://zhuanlan.zhihu.com/p/429253463)

#### DoraHacks
 - [x] [零知识证明：Halo2解读](https://www.bilibili.com/video/BV1qk4y1j71i)    乏善可陈...

#### Code
 - [x] [Halo2 Tutorial Intro](https://erroldrummond.gitbook.io/halo2-tutorial)
	 - [x] 提供了 ECDSA / 等 gadgets , 值得再读 ...
	 - [x] 提供了  Range check 等 gadgets , 值得再读 ...
 - [x] [Develop Circuits Using Halo 2](https://medium.com/@ola_zkzkvm/sin7y-tech-review-develop-circuits-using-halo-2-829e2f26856)    对 ZCash $a^2 + b^2 = c$  的 Tutorial , 图解非常好, 搭配 ZCash 的翻译一起看
 - [x] [Building a Zero Knowledge web app with Halo 2 and Wasm (part 1)](https://medium.com/@yujiangtham/building-a-zero-knowledge-web-app-with-halo-2-and-wasm-part-1-80858c8d16ee)  Very Goods！
 - [x] [zordle](https://github.com/nalinbhardwaj/zordle)  讲解清晰!
 - [x] [Awesome HALO2](https://github.com/adria0/awesome-halo2)
 - [x] [halo2 tutorial From HaiCheng Shen](https://github.com/icemelon/halo2-tutorial)  沈海诚 code
 - [x] https://github.com/Divide-By-0/halo2-learning-group

#### libs:
 - [More Optimized ECC Operations in halo2 by Axiom](https://github.com/axiom-crypto/halo2-lib/tree/main/halo2-ecc/src)
- [ ] axiom libs.
	- [ ] SY: We just open sourced a library of base components including range check here: https://github.com/axiom-crypto/halo2-lib/blob/main/halo2-base/src/gates/range.rs
	- [ ] https://axiom-crypto.github.io/halo2-lib/halo2_base/gates/range/trait.RangeInstructions.html#tymethod.is_less_than
	- [ ] This uses halo2-lib: https://github.com/axiom-crypto/halo2-lib
	- [ ] YT: it looks like the axiom fork does variable-length Poseidon (https://github.com/axiom-crypto/halo2/tree/main/primitives/poseidon):
	- [ ] ECDSA: https://github.com/axiom-crypto/halo2-lib/blob/main/halo2-ecc/src/secp256k1/tests/ecdsa.rs

#### Others :
 - [ ] [Deep dive on Halo2](https://www.youtube.com/watch?v=YlTt12s7vGE&t=4770s)
 - [ ] [ZKSwap 团队详细解读——Halo2 原理](https://www.geekmeta.com/article/3704425.html)
 - [ ] [Zero knowledge 2 Zero knowledge](https://mirror.xyz/searchblock.eth/y11EKtXAtK3aXRVMV1yYqw7FibKHxI0fK10vlVRDaD4)
 - [ ] [EF grants halo2 Out of date.](https://github.com/EDGDrummond/EF-grant)
 - [ ] YouTube Series : 
	 - [ ] [zkEVM Audit Education Sessions 1/4 -Circuit Arithmetization for ZKP](https://www.youtube.com/watch?v=ofDcWqZAGK4)   workflow of halo2
	 - [ ] [zkEVM Audit Education Sessions 2/4 -Fibonacci examples for PIL & Halo2](https://www.youtube.com/watch?v=luMnF1fHeIk)
	 - [ ] [zkEVM Audit Education Sessions 3/4 - Architecture and Workflow](https://www.youtube.com/watch?v=qbR5pwmRxKY)
	 - [ ] [zkEVM Audit Education Sessions 4/4 - zkEVM Auditing & Lessons Learned](https://www.youtube.com/watch?v=gCKh6ySXYTU)
 - [ ] [Sin7Y Tech Review: Develop Circuits Using Halo 2](https://medium.com/@ola_zkzkvm/sin7y-tech-review-develop-circuits-using-halo-2-829e2f26856#)


Records:
 - [ ] [Little Things I’ve Learned in Developing Halo2 Circuits by Chih-Cheng Liang | Devcon Bogotá]()


论文: BCH19 Recursive proof composition without a trusted Setup

