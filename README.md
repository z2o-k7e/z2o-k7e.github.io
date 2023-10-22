

<span style="color: grey;">**Welcome to the**</span>

![](./src/logo.png)



<span style="color: grey;">**World!**</span>



<br />



[github](https://github.com/zkp-co-learning) of  $`\mathcal{Z}_2 \mathbb{O} \ {\text -} \ \mathcal{K}_7 \mathbb{E}`$



<br />





### 「给贡献者的小帖士」

#### 贡献流程

1. Github 上 fork 本 [Repo](https://github.com/zkp-co-learning/zkp-co-learning.github.io)
2. 可以在 `./src/zk-everything` 下 `mkdir` 一个以自己名字命名的文件夹
3. `src/SUMMARY.md` 是前端网站显示的文件组织目录，可以修改该文件，找到一个合适的放置目录，将文章的本地 `.md` 文件位置链接过去
4. 正常的 PR 流程
5. 经老师们审核后领取 Bounty！

#### 文章格式

##### 内容模板: 

1. 文章 metadata ，如 「贡献者作者信息 (required）」，  「标签、联系方式 (optional） 」

```bash
> 作者: 如 @大壮 https://github.com/dazhuang      
> 标签: 如 halo2, Nova, STARK, Folding schema .... # mdbook 暂不支持 tag 功能
> 时间: 2023-09-10
```

> 作者：[大壮](https://github.com/Demian101)

or

> Author: [大壮](https://github.com/Demian101)



2. 文章开始之前，可以添加 `[TOC] `来生成该文章的 Table of contents（目录）

```bash
[TOC]
```

3. 可添加admonition block，语法见[这里](https://tommilligan.github.io/mdbook-admonish/)

```admonish success title=""
This will take a while, go and grab a drink of water.
```


4. 文章正文（Markdown 格式的正文内容，无需担心 github 糟糕的渲染）

5. 文章末尾可以列出 「致谢」 & 「参考文献 References」

```
# References
 - [trapdoor-tech halo2 book](https://trapdoor-tech.github.io/halo2-book-chinese/user/simple-example.html)
 - [icemelon/HaiCheng Shen](https://github.com/icemelon/halo2-examples/blob/master/src/fibonacci/example3.rs)
 - [0xPARC halo2](https://learn.0xparc.org/)
```



----



如何添加图片？

 - 推荐直接在 `.md` 文章同级目录 `mkdir ./imgs` 目录，mdbook 中直接引用该 imgs 目录相对路径
 - 如果您使用的是 OSS 云存储，则无需考虑图片存储，只需一个 `.md` 文件即可~



##### 配套代码(optional)

如果文章有对应的实战代码那就再好不过啦！

可以直接 PR 到另一个 [Repo](https://github.com/zkp-co-learning/zkp-co-learn)，新建一个目录即可。

```bash
├── Nova
├── README.md
├── halo2-doc
├── halo2-learn
├── [Your code repo here !!]  # mkdir your code repo here !!
└── zk-everything
```

> 这边还没想好怎么放，可能后面位置有改动，不过...反正先放就好了 ！


#### 关于 md 渲染

众所周知，Github  网站的 `Latex` 等渲染功能非常弱鸡，往往需要一些奇技淫巧才能让公式等正常渲染出来。而在本 MDBOOK 中，您完全不需要关注这种伤害身心的问题，不需要给 github markdown 做专门的适配和魔改。在 Obsidian（或者如 Typora 等主流 Markdown 编辑器）里的  `.md` 文件显示是什么样的，本网站中都可完美无痛渲染！

**本地 Dev 预览方法：**

```bash
$ [安装 Rust]
$ cargo install mdbook mdbook-latex  mdbook-toc
$ mdbook serve --open       # 本地预览

```

Tips :
 - `src/SUMMARY.md` 是会在前端组织显示的所有文件目录及其链接
 - 公式测试：可以在 [katex.org](https://katex.org/) 测试，大家在 Obisidian notes 里怎么写公式，前端就会咋显示，

（contribution by PR process）



<br />

<br />

<br />

<br />

<br />



[TOC]



# Playlists


[Overview of jolt and lasso CN](https://www.youtube.com/watch?v=vDvyzVsyJm4&list=PLbQFt1T_44DySCe3glmsa6DoCJNFftCo5&index=1&t=2s&pp=iAQB) 

[Simple example for lasso CN](https://www.youtube.com/watch?v=w4UcrbKKjEo&list=PLbQFt1T_44DySCe3glmsa6DoCJNFftCo5&index=2)

[ProtoStar Compressing verification CN](https://www.youtube.com/watch?v=aHjrkWUvJ8g&list=PLbQFt1T_44DySCe3glmsa6DoCJNFftCo5&index=3)

[Some details of lasso polynomial commitment CN](https://www.youtube.com/watch?v=VfJogiQ---A&list=PLbQFt1T_44DySCe3glmsa6DoCJNFftCo5&index=4)

[Differences Between Nova and HyperNova CN](https://www.youtube.com/watch?v=VKZMZuKZ7JI&list=PLbQFt1T_44DySCe3glmsa6DoCJNFftCo5&index=5)

[Cycle fold based NOVA CN](https://www.youtube.com/watch?v=LoXX6EAUTYc&list=PLbQFt1T_44DySCe3glmsa6DoCJNFftCo5&index=6)

[Nova relaxed R1CS and it’s implementation CN](https://www.youtube.com/watch?v=7Zs_Y-wVrP0&list=PLbQFt1T_44DySCe3glmsa6DoCJNFftCo5&index=7)

[Protostar Accumulation scheme for NARK’s verifier checks CN](https://www.youtube.com/watch?v=3ag5jZh5OHQ&list=PLbQFt1T_44DySCe3glmsa6DoCJNFftCo5&index=8)

[ProtoStar Overview CN](https://www.youtube.com/watch?v=MNZ-Sm7jQic&list=PLbQFt1T_44DySCe3glmsa6DoCJNFftCo5&index=9)


**Nova Study session**

[Nova讨论-Primary和Secondary电路 CN](https://www.youtube.com/watch?v=gopJn_QAdqU&list=PLbQFt1T_44DwtG7Qv_BEyCP_t37qT9yMV&index=1)

[Nova讨论-延展性攻击 CN](https://www.youtube.com/watch?v=z4aEW9hxEs8&list=PLbQFt1T_44DwtG7Qv_BEyCP_t37qT9yMV&index=2)

[Nova讨论-Spartan协议part1 CN](https://www.youtube.com/watch?v=at2U9iOvEBg&list=PLbQFt1T_44DwtG7Qv_BEyCP_t37qT9yMV&index=3)

[SuperNova原理及其ROM实现CN](https://www.youtube.com/watch?v=xmMF5qn1T0M&list=PLbQFt1T_44DwtG7Qv_BEyCP_t37qT9yMV&index=4)

**ZKML**

[Painless Zero-Knowledge Circuitry with Nior - Aztec EN](https://www.youtube.com/watch?v=jkUIR_aM9JU&list=PLbQFt1T_44DwLHk3TOWUoFMYmVLksr3i6&index=2&t=51s)

[Things we should build on ZKML NOW By Dr Cathie @PSE EN](https://www.youtube.com/watch?v=fWbKeJeh0fY&list=PLbQFt1T_44DwLHk3TOWUoFMYmVLksr3i6&index=4)

**ZK co-learning**

[halo2 (zkEVM gadgets) 分享者 Dream @Scroll CN](https://www.youtube.com/watch?v=bgYsKXo4his&list=PLbQFt1T_44Dzl6glAwuL9DoegcFFAeipt&index=5)

[PSE - Semaphore CN](https://www.youtube.com/watch?v=jXI4f7F8p94&list=PLbQFt1T_44Dzl6glAwuL9DoegcFFAeipt&index=7)

[代码分享：plonkathon、babyPLONK CN](https://www.youtube.com/watch?v=Qsqk_D_jabg&list=PLbQFt1T_44Dzl6glAwuL9DoegcFFAeipt&index=8)

[用 Python来写简单的加密算法 CN](https://www.youtube.com/watch?v=mnDkTYqU9nM&list=PLbQFt1T_44DwN1zWl-KWhkp3s0LAkF2a8&index=8)

[分享 PLONK 原理 一 CN](https://www.youtube.com/watch?v=HtKmRcSJUG4&list=PLbQFt1T_44DwN1zWl-KWhkp3s0LAkF2a8&index=5)

[分享 PLONK 原理 二 CN](https://www.youtube.com/watch?v=O5HGp3EHDI0&list=PLbQFt1T_44DwN1zWl-KWhkp3s0LAkF2a8&index=4)

[分享Plonk 原理 三 Lookup argument CN](https://www.youtube.com/watch?v=StvnHnC4Dk4&list=PLbQFt1T_44DwN1zWl-KWhkp3s0LAkF2a8&index=3)

[郭宇@安比：Nova - Recursive SNARKs without trusted setup CN](https://www.youtube.com/watch?v=l19roUItyUE&list=PLbQFt1T_44DwN1zWl-KWhkp3s0LAkF2a8&index=2)

[ZKSAFE wallet案例分享 CN](https://www.youtube.com/watch?v=7zPfVUSyrVI&list=PLbQFt1T_44Dx9oPmtQBZzhlPkxsGqjLm0&index=7)

[From +-/ to Elliptic Curve @Boyuan Feng CN](https://www.youtube.com/watch?v=XLgvaccfx64&list=PLbQFt1T_44Dx9oPmtQBZzhlPkxsGqjLm0&index=5)

[zkiap session 6 & 8 QA @Jason Mortan EN](https://www.youtube.com/watch?v=ZxpA0gyjEn4&list=PLbQFt1T_44Dx9oPmtQBZzhlPkxsGqjLm0&index=4)


[Zkstudy Session 01: Spartan sum-check part 1 EN](https://www.youtube.com/watch?v=qvLmM3AYmhs&list=PLbQFt1T_44DylOOLHiWCIpVGJsrXHhzbs&index=9)

[ZKstudy Session 02 - Spartan: Encoding of R1CS  EN](https://www.youtube.com/watch?v=BLh_o2lZAbw&list=PLbQFt1T_44DylOOLHiWCIpVGJsrXHhzbs&index=8)

[ZKstudy Session 03 - CCS: Represent R1CS and Plonkish EN](https://www.youtube.com/watch?v=LPuMWgvJWgc&list=PLbQFt1T_44DylOOLHiWCIpVGJsrXHhzbs&index=7)

[ZKstudy Session 04 - 1- Represent R1CS and Plonkish; SuperSpartan's polynomial IOP for CCS EN](https://www.youtube.com/watch?v=D4HmInmuaYI&list=PLbQFt1T_44DylOOLHiWCIpVGJsrXHhzbs&index=6)

[ZKstudy Session 04 - 2- Represent R1CS and Plonkish; SuperSpartan's polynomial IOP for CCS EN](https://www.youtube.com/watch?v=xojrRDeBW2A&list=PLbQFt1T_44DylOOLHiWCIpVGJsrXHhzbs&index=5)

[Nova p1: Recursive Snarks and IVC EN](https://www.youtube.com/watch?v=smOPs0bWen0&list=PLbQFt1T_44DylOOLHiWCIpVGJsrXHhzbs&index=4)

[Nova p2 - folding schemes for r1cs EN](https://www.youtube.com/watch?v=ZPH1dC54t0k&list=PLbQFt1T_44DylOOLHiWCIpVGJsrXHhzbs&index=3)

[Nova: IVC from folding scheme - ZKstudy Session 07 EN](https://www.youtube.com/watch?v=xpgDdTqPnSg&list=PLbQFt1T_44DylOOLHiWCIpVGJsrXHhzbs&index=2)
[Hypernova - ZKstudy Session 08 EN](https://www.youtube.com/watch?v=oRhA3pLvsV0&list=PLbQFt1T_44DylOOLHiWCIpVGJsrXHhzbs&index=1)


[Scroll主题分享 zkrollup 线下 CN](https://www.youtube.com/watch?v=aNvI-P9livQ&list=PLbQFt1T_44DyVlo_E6o2TaK_nGdmEr2n-&index=3)

[Scroll 分享 zkEVM  CN](https://www.youtube.com/watch?v=ZmEnhbIxKyE&list=PLbQFt1T_44DyVlo_E6o2TaK_nGdmEr2n-&index=2)

[Towards trustless cross-chain communication from MPC to Zero-knowledge EN](https://www.youtube.com/watch?v=3whHesGCNcM&list=PLbQFt1T_44DzJsnfxQDgO3PaO8-LowVgo&index=2)

[Schnorr sequencer & KZG RLN EN](https://www.youtube.com/watch?v=JvAHd3t_4yc)

[Autonomous world & quick intro to tornado cash's privacyEN](https://www.youtube.com/watch?v=CZixqJ7Pv84)

[Chiquito and infinite garden ZKP Workshop EN](https://www.youtube.com/watch?v=7opMtn-3gjQ&t=3955s)

[ZKML with EZKL sharing session EN](https://www.youtube.com/watch?v=QgAuzsSbcik)

---

# Project Description:

zkp-co-learning is a Zero-Knowledge Proofs (ZKP) collaborative study and creation project that has been running for three terms. Since February 2023, we have studied https://zkiap.com/, zk-learning.org, plonkathon codes and more with over 300 participants. We are currently preparing advanced content for zk-learning.org.(like Nova、STARK...)

zkp-co-learning offers bounties to incentivize learners for their technical sharing, actively becoming community maintainers (Maintainer), and for organizing knowledge in the ZKP field. Together, we aim to improve ZKP Public Goods.

## Problem Statement:

Currently, there are challenges related to high barriers to entry, difficulty in learning in the ZKP field, lack of learning communities, and the overwhelming and opaque nature of information. We intend to address these issues specifically through zkp-co-learning.

## Team Background:

- Qi Zhou: Founder of ETHStorage
- 郭宇@Secbit: Founder of Secbit, https://github.com/sec-bit, https://secbit.io/
- Kurt Pan: Ph.D. in Cryptography from Fudan University, https://github.com/kurtpan666 / https://cryptography.land/
- Harry L: co-Founder of Rebase Community Core Contributor @Antalpha_Labs.
- Shirlene 孝羽: director of Creators Co-learning Community
- Demian: zkp-co-learning community maintainer, former JD.com algorithm engineer, https://github.com/Demian101
- Our Maintainers: dream@Scroll, Po@EthStorage, 0xhhh@EthStorage, Frank Jz Liu, miles, 白菜, KEEP, CJ, 笃行, 阳小雪, 啊咪咪小熊, 饭卡里还有不少钱呢...
- sponsoring agency: [Antalpha Labs]

## Vision and Mission:

1. Continue to recruit Maintainers to collaboratively manage the community and answer questions.
2. Operate technical media and Twitter to expand influence.
3. Utilize zk technology to support a broader range of web3 applications. 

## Roadmap:

We plan to accomplish the following tasks within 6 months:
1. Launch the zkp-co-learning.xyz website and aim for:
  1.1 Comprehensive ZKP learning path sorting and a clear, seamless learning guide. 
  1.2. zk Hackthon information gathering, allowing everyone to showcase and discuss ideas, and team up for Hackthon. 
  1.3. Distribute bounties, allowing anyone interested to participate and contribute knowledge and content.
2. Complete research on at least 5 open-source projects for EF PSE. (https://www.appliedzkp.org/projects)
3. Host a Hackthon or Hackerhouse to put theory into project practice.
4. Develop at least one zk application project with specific needs and use cases.
5. Organize and open-source the vast amount of zk materials that have accumulated and scattered within the current co-learning GROUP.
6. Attract at least 200 more people to participate in zkp technology learning and engage at least 20 developers in the development of zkp projects.


----

## Project Description：

zkp-co-learning 是一个已经持续了 3 期的 ZKP 共学共创项目，从 2023 年 2 月至今，我们与 300+ 学员共同研究了 https://zkiap.com/ ，zk-learning.org ，plonkathon 代码等，目前正在筹备 zk-learning.org 的进阶内容

zkp-co-learning 针对学员的技术分享，积极成为社群维护者（Maintainer），ZKP 领域的知识整理，都会提供对应的 bounty 来激励他们，共同完善 ZKP Public Goods


## Problem Statement

目前，针对 ZKP 领域学习门槛高、难度大，学习社群缺乏，信息庞杂不透明的情况，我们意图通过 zkp-co-learning 来针对性地解决这些问题


## Team Background

as above.


## 愿景

1. 继续招募 Maintainer 来共同维护社区，解答大家的问题
3. 运营技术媒体与 Twitter，扩大影响力
4. 利用 zkp 技术支持更广泛场景的 web3 应用


## Roadmap

我们计划使用 5 个月的时间完成以下事务：
1. 上线 zkp-co-learning.xyz 网站，完成：1. ZKP 完整学习路径梳理，清晰无痛的学习指南  2. zkp Hackthon 信息收集，让大家展示交流 ideas，组队 Hackthon  3. bounty 放送，让每个感兴趣的人都能参与其中，进行学识和内容的贡献
2. 完成至少 5 个 EF PSE 的开源项目 Research
3. 举办一次 Hackthon 或 Hackerhouse 来将理论付诸项目实践
4. 完成至少 1 个具有需求和使用场景的 zkp 应用项目的研发
5. 整理与开源目前共学 GROUP 里面沉淀散落的大量 zkp 资料
6. 再吸引至少 200 人参与到 zkp 技术的学习中，让至少 20 名开发者参与到 zkp 项目的研发中


----



## 投稿
欢迎大家把自己在写的内容放在这里， [zk-everything] 我们会同时排版到Antalpha Labs的公众号下.(最好留下联系方式，方便后续跟进）

投稿流程：
1. fork this repo and clone it to local machine (choose `for your own use`)
2. copy directy `.md` file into `src` folder
3. In `src/SUMMARY.md` (i.e the structure file of mdbook), add link to your newlly added posts
4. commit and push
4. to make a PR and wait to merge





## Themes ！
- 自由选题，完成学习并分享可得 **Bounty** ，推荐 PSE 开源项目 \~
  -  [Meeting PSE Share]

- 年底[土耳其 ZKP HackerHouse](https://labs.antalpha.com/hackerhouse/istanbul/)等你来 ！！

