## 引言
Halo2官方文档以及0xparc的系列视频教程已经很好了，但是对于初学者还是不够线性，刚开始就引入了chip等一些高级概念，同时整个课程虽注重实例但体系化不是很强，对于初学Halo2不是特别友好；同时Halo2最新的`0.3`版本与视频中采用的旧版Halo2有些API已经不太一致，如`FieldExt`等。
本文打算从电路的角度出发，针对电路开发中遇到的实际问题，依次从:
- [Halo2 API](#halo2-api概览)
- [chap-1:仅有单个门电路](./chap-1/readme.md)
- 包含自定义门电路
- Chip
- 具有动态Row的电路
- 电路优化
- 包含单列lookup的电路
- 包含多列lookup
等这些角度，从小白的视角,由浅入深学习和理解Halo2相关概念及API。

## Halo2 API概览
查看[Halo2代码](https://docs.rs/halo2_proofs/latest/halo2_proofs/)，会发现主要提供了5大类的API:
- plonk: 电路定义相关的API，比如定义电路的witness有哪些列以及电路约束等，这也是最重要使用的API；
- circuit: 为电路赋值witness会用到这些API，比如layouter来进行电路布局；
- poly: 为在多项式进行算数化提供了各种工具，包括采用Rotation选取相邻的row，计算commitment，对多项式打开等
- arithmetic: 主要提供与group, field和多项式相关的API，我们会在实例化电路的选用具体的曲线时候会用到；
- dev: 电路布局可视化等开发者工具
- transcript: 主要包括Fiat-Shamir相关API，这部分很少用到