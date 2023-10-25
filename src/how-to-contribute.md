-----

[TOC]

-----

## 贡献流程

1. Github 上 fork 本 [Repo](https://github.com/zkp-co-learning/zkp-co-learning.github.io)
2. 可以在 `./src/zk-everything` 下 `mkdir` 一个以自己名字命名的文件夹
3. `src/SUMMARY.md` 是前端网站显示的文件组织目录，可以修改该文件，找到一个合适的放置目录，将文章的本地 `.md` 文件位置链接过去
4. 正常的 PR 流程
5. 经老师们审核后领取 Bounty！

## 文章格式

## 内容模板

1. 文章 metadata ，如 「贡献者作者信息 (required）」，  「标签、联系方式 (optional） 」

```bash
> 作者: 如 @大壮 https://github.com/dazhuang      
> 标签: 如 halo2, Nova, STARK, Folding schema .... # mdbook 暂不支持 tag 功能
> 时间: 2023-09-10
```

比如：
> 作者：[大壮](https://github.com/Demian101)
> 
> Author: [大壮](https://github.com/Demian101)



2. 文章开始之前，可以添加 `[TOC] `来让 mdbook 自动生成该文章的 Table of contents（目录）

```bash
[TOC]
```

3. 可添加 admonition block，语法见[这里](https://tommilligan.github.io/mdbook-admonish/)

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


## 配套代码(optional)

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


## 关于 md 渲染

众所周知，Github  网站的 `Latex` 等渲染功能非常弱鸡，往往需要一些奇技淫巧才能让公式等正常渲染出来。而在本 MDBOOK 中，您完全不需要关注这种伤害身心的问题，不需要给 github markdown 做专门的适配和魔改。在 Obsidian（或者如 Typora 等主流 Markdown 编辑器）里的  `.md` 文件显示是什么样的，本网站中都可完美无痛渲染！

**本地 Dev 预览方法：**

```bash
$ [安装 Rust]
$ cargo install mdbook mkbook-latex  mdbook-toc
$ mkbook serve --open       # 本地预览

```

Tips :
 - `src/SUMMARY.md` 是会在前端组织显示的所有文件目录及其链接
 - 公式测试：可以在 [katex.org](katex.org) 测试，大家在 Obisidian notes 里怎么写公式，前端就会咋显示，

（contribution by PR process）
