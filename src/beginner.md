> 💡 [@Demian](https://github.com/Demian101?tab=repositories): 作为 zkp 新人，走了很多弯路，也整理下自己的学习路径和一些参考资料供大家入门。 希望本教程可以帮助减少一些盲目的打击和莫名的痛苦，节省一点点时间

[TOC]

### 1. 建立对 zkp 的直观理解

① 在纵身潜入 ZKP 的海洋之前，可以先建立对它**最直观的理解**：

- [1. 初识「零知识」与「证明」](https://learn.z2o-k7e.world/zkp-intro/1/zkp-back.html)
- [2. 理解「模拟」](https://learn.z2o-k7e.world/zkp-intro/2/zkp-simu.html)
- [3. 寻找「知识」](https://learn.z2o-k7e.world/zkp-intro/3/zkp-pok.html)

> - 安比实验室（郭宇老师）所写的 zkp-intro 是公认目前全网最简洁易懂的 zkp 入门系列（而且还是中文的！！）  
> - 前 3 篇需要看懂，不了解的概念就 Google + chatgpt + 社群询问 ...
> - Chap 4-5 主要是非交互的 Schnorr 和 CRS、哈密尔顿环路等，看不懂没关系，可以先放一放

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

其他优秀的 Courses （随便看看）: 

- [Zero Knowledge Canon, part 1 & 2 - a16z crypto](https://a16zcrypto.com/posts/article/zero-knowledge-canon/)
- https://twitter.com/taikoxyz/status/1679468185291218944 ：

```
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
```


### 4. PLONK 协议の奥义

PLONK 无疑是目前最值得学习，需要彻底掌握的协议



#### 《理解 Plonk》

- [1-Plonkish Arithmetization](https://learn.z2o-k7e.world/plonk-intro-cn/plonk-arithmetization.html)
- [2-多项式编码](https://learn.z2o-k7e.world/plonk-intro-cn/plonk-lagrange-basis.html)
- [3-置换证明](https://learn.z2o-k7e.world/plonk-intro-cn/plonk-permutation.html)
- [4-算术约束与拷贝约束](https://learn.z2o-k7e.world/plonk-intro-cn/plonk-constraints.html)
- [5-多项式承诺](https://learn.z2o-k7e.world/plonk-intro-cn/plonk-polycom.html)
- [6-实现 Zero Knowledge](https://learn.z2o-k7e.world/plonk-intro-cn/plonk-randomizing.html)
- [7-Lookup Gate](https://learn.z2o-k7e.world/plonk-intro-cn/plonk-lookup.html)

> 出品：Secbit @郭宇 老师 , https://secbit.io/
> 
> 是大家公认的全网（包括外网）最好的 PLONK Tutorial。
> 
> 学习 PLONK，这一套就够了！（论文可以随便看一下）


PS: 如果文档看晕了，那么推荐郭老师的配套白板视频：
 - [【理解 PLONK - 1】 by 郭宇老师@安比实验室 05/02](https://github.com/zkp-co-learning/zkp-co-learn/discussions/86)
 - [【理解 PLONK - 2】 by 郭宇老师@安比实验室 05/03](https://github.com/zkp-co-learning/zkp-co-learn/discussions/89)
 - [【理解 PLONK - 3 - lookup】 by 郭宇老师@安比实验室 05/09](https://github.com/zkp-co-learning/zkp-co-learn/discussions/95)

-----

#### PLONK 代码实践
 - [plonkathon](https://github.com/0xPARC/plonkathon)
- [Rust 协议实现](https://github.com/adria0/plonk-by-fingers)
- [Harry 整理的精简版本 plonkathon 代码实现](https://github.com/zkp-co-learning/ZKP/blob/main/5-plonk-intro.md) 
- [Dr. Qi zhou 分享](https://706community.notion.site/Python-finite-field-poly-over-Fq-FFT-1968732f468c4c3fa3886a5658c58773?pvs=4)

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

【out of date】，请移步： zk Materials

### Tools
 - [Youtube 字幕神器](https://github.com/zkp-co-learning/zkp-co-learn/discussions/11)
 - [常用的希腊字母及其发音](https://github.com/zkp-co-learning/zkp-co-learn/discussions/91)
 - [github - Readme Latex](https://docs.github.com/zh/get-started/writing-on-github/working-with-advanced-formatting/writing-mathematical-expressions)
	 - 建议使用 `$` `$`，我自己测试过是有效的
