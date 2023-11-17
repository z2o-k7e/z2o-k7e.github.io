-----

### **第一周 (10.15~10.21):**

1. 课程学习资料: 
   1. [理解 PLONK 系列](https://learn.z2o-k7e.world/plonk-intro-cn/plonk-intro.html)
   2. [1-basic concepts](https://learn.z2o-k7e.world/halo2/chap-0/index.html)
   3. [0xPARC halo2 Introduction](https://learn.0xparc.org/materials/halo2/learning-group-1/introduction) 

2. 课程学习目标: Understanding and building user-facing applications with `Halo2` and `PLONKish` proving systems



### **第二周 (10.22~10.28):**

1. 课程学习资料:
	- [2-simple circuit](https://learn.z2o-k7e.world/halo2/chap-1/index.html)
	- [3-custom gate](https://learn.z2o-k7e.world/halo2/chap-2/index.html)
	- [4-fibonacci](https://learn.z2o-k7e.world/halo2/chap-3/index.html)
	- 0xPARC halo2 Lecture:
	  - [Halo2 API & Building a Basic Fibonacci Circuit (Part 1)](https://learn.0xparc.org/materials/halo2/learning-group-1/halo2-api)
	  - [Halo2 API & Building a Basic Fibonacci Circuit (Part 2)](https://learn.0xparc.org/materials/halo2/learning-group-1/halo2-api-continued)
2. 课程学习目标:
	- 理解 halo2 中 Circuit、region、Layouter、custom gate、constraints 等概念及其 API 代码 represent.
	- 能够理解 Fibonacci 示例和 isZero 示例的电路构建流程

### **第三周 (10.29~11.04)**

1. 课程学习资料: 
	- [lookup table](https://learn.z2o-k7e.world/halo2/chap-4/index.html)

2. 课程学习目标:
	- 掌握 halo2 中 lookup / lookup_any API 的使用，及其常见应用场景（如 Range-check）

### **第四周 (11.06~11.12)**

1. 课程学习资料:
    - [0xPARC decompose](https://learn.0xparc.org/materials/halo2/learning-group-1/exercise-3)
    - 代码 repo: <https://github.com/zkp-co-learning/halo2-step-by-step>
    - 推荐参考笔记： <https://learn.z2o-k7e.world/halo2/tmp/9-decomposed.html>
2. 课程学习目标:
	- decompose 是一个综合性的代码实践，要求我们在阅读源码的同时掌握 halo2 的核心 API。

### **第五周 (11.13~11.19)**

#### ① zkEmail 

本周以 zkEmail 为例的 zk 应用实例的 halo2 真实应用场景代码实践。

zkEmail 学习方法论：

首先要明确学习目标，学习 zkEmail 能学到什么？我觉得有以下几点：

1. zk email 是一个解决“实际业务”的中型电路系统；
2. zk email 是强烈依赖 [halo2-base](https://github.com/axiom-crypto/halo2-lib/tree/community-edition/halo2-base) 的，而 halo2-base 对 halo2 电路有着一定的优化，主要体现在接口优化和灵活的行列设计；
3. zk email 中会涉及到 RSA，biguint，sha256，regex，base64 等电路；代码模块比较清晰，每个模块也有例子，一周时间学习全部的内容有点多，大家可以根据需求学习；

如果有对以上有兴趣的同学可以学习 zk email 的学习路线建议：
1. 我觉得可以先从 [halo2-base](https://github.com/axiom-crypto/halo2-lib/tree/community-edition/halo2-base) 入手，学习 [axiom](https://github.com/axiom-crypto) 对 halo2 做的优化；
2. 选择一个感兴趣的模块分析代码切入，逐步深入，比如 从 biguint 计算 => RSA , 逐步把 RSA，Regex，Sha256 这几个电路理解，再理解 zk email 是如何像堆积木一样把整个大电路堆积起来的；

halo2-zk-email 中可以学习到的更多是在电路前端设计上，规模属于中型应用，可能还是花费一些时间的。

> - 由 @DK(零与一)@secbit 帮助整理 ~


top-down 的 approach: 
- 直接打开 <https://emailwallet.org/> 去玩他们最新的出来的 emailwallet 
- 看 slides: <https://docs.google.com/presentation/d/1nHW57t8SQ-NCqK366_xpkB7WuC3lFX-9/>
- docs: <https://docs.sendeth.org/>
- 再打开 github: <https://github.com/zkemail>，就接起来了。

> - 由 @Kurt Pan 帮助整理 ~

#### ② zkEVM

- [zkEVM Word encoding](https://github.com/zkp-co-learning/halo2-step-by-step/discussions/58)

下次 oh 会介绍一下 zkevm。以 [PSE/Scroll zkevm-circuit](https://github.com/scroll-tech/zkevm-circuits) 代码为例子。

可以先通过 <https://www.evm.codes> 了解一下 EVM 指令集，在 playground 玩一下感兴趣的指令。

然后，通过下面的资料了解下 evm bytecode 的组成格式，执行过程。
- <https://www.youtube.com/watch?v=_tcyI_lNvo0>
- <https://medium.com/@_ajsantander>

最后，推荐 PSE 的 [zkevm-specs](https://github.com/privacy-scaling-explorations/zkevm-specs)，文档中详细定义了 zkevm 电路的约束，并且提供了对应的 python 代码。
- <https://github.com/privacy-scaling-explorations/zkevm-specs>

> - 由 @[Yang Zhou](https://github.com/yz89) 帮助整理 ~

<br />
<br />
<br />


----

<br />
<br />
<br />
**前置学习 (PLONK & Rust)：**

 - [x] [PLONK Tutorials](https://learn.z2o-k7e.world/plonk-intro-cn/plonk-arithmetization.html) &  [Lookup Gates @secbit](https://learn.z2o-k7e.world/plonk-intro-cn/plonk-lookup.html)
 - [x] [Rust 圣经](https://course.rs/about-book.html)

**必备学习材料：**

1. [0xPARC Halo2](https://learn.0xparc.org/halo2)  是主要参考的视频课程
2. [参考代码](https://github.com/zkp-co-learning/halo2-step-by-step/tree/main)   是对课程 Reference 代码必要的优化和版本升级
3. [参考文档: z2o-k7e-Halo2](https://learn.z2o-k7e.world/halo2/chap-1/index.html)   是部分电路图解 & 对代码更细致的阐释(仍更新中...) [^1]
4. 其他必读参考：
	1. [zcash halo2 book](https://zcash.github.io/halo2/) / [github](https://github.com/zcash/halo2/blob/main/book/)  理论内容偏多，可以当做字典反复查看
	2. [zcash halo2 book 中文翻译](https://trapdoor-tech.github.io/halo2-book-chinese/) / [github](https://trapdoor-tech.github.io/halo2-book-chinese/)




**其他参考资源:**
- [halo2.club](https://halo2.club)
- [Awesome halo2](https://github.com/adria0/awesome-halo2)
- [halo2-learning-materials](https://learn.z2o-k7e.world/halo2/halo2.html)
- [QA](https://github.com/zkp-co-learning/halo2-step-by-step/discussions)  |  [开营视频回放](https://www.youtube.com/watch?v=0BVaXaRpgww&t=10s)



----



for 纯新手:

1. 新手学习 ZKP 可以参考新手村入门攻略 <https://learn.z2o-k7e.world/zk-everything/zk-roadmap.html> (must)
2. 学习 PLONK 郭老师的系列没跟上非常正常，一般正常人都大概需要反复读 5 遍，各种概念不断交叉记忆，相互关联，然后才能建立一些直观的印象
3. 有了一定的印象后，可以参考 [@Harry L 为文档写的 python 代码](https://github.com/Antalpha-Labs/baby-plonk/blob/main/tutorials/understanding-plonk-cn/3-plonk-permutation.ipynb)，手敲印证，加深印象 (optional)
4. 学习 Rust 编程 (must)
5. halo2 学习方法论： <https://learn.z2o-k7e.world/halo2/halo2.html>  (must)



> !有任何看不懂的 Part 都可以提 [issue](https://github.com/zkp-co-learning/halo2-step-by-step/issues) 或者 [Q&A](https://github.com/zkp-co-learning/halo2-step-by-step/discussions/categories/q-a) ~
> 
> 备注：因为现在还没有关于 halo2 特别好的一站式课程，所以很多参考资源需要反复研究 & 交叉印证



[^1]: 目前我们正在对部分教程内容进行重新的整理&修改，会随着课程逐步更新和优化，欢迎 👏🏻 PR 和 issue ！
