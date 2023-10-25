-----

### **第一周:**

1. 课程学习资料: 
   1. [理解 PLONK 系列](https://learn.z2o-k7e.world/plonk-intro-cn/plonk-intro.html)
   2. [1-basic concepts](https://learn.z2o-k7e.world/halo2/chap-0/index.html)
   3. [0xPARC halo2 Introduction](https://learn.0xparc.org/materials/halo2/learning-group-1/introduction) 

2. 课程学习目标: Understanding and building user-facing applications with `Halo2` and `PLONKish` proving systems



### **第二周:**

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

### **第三周**

- [editing...]



----
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
