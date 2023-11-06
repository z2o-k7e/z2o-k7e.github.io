11.04/2023 Debug..
### before = ["gettext"]

```toml
[preprocessor.katex]
before = ["gettext"]
after = ["links"]
```

在 
```bash
MDBOOK_BOOK__LANGUAGE=en mdbook serve -d book/en
```
状态下，每次 en.po 改变，页面都会相应改变。

注意，此时 message.pot 文件是空的。
```bash
#: src/SUMMARY.md:5
msgid "1"
msgstr ""
```

暂时推测：en.po 文件才是决定 en 目录渲染的关键。

en.po:
```bash
#: src/plonk-arithmetization.md:22
msgid ""
"电路中有4个变量，其中三个变量为输入变量 $(x_1, x_2, x_3)$ ，一个输出变量 "
"$out$，其中还有一个输入为常数，其值为 $2$。"
msgstr ""
"There are 4 variables in the circuit, with three variables being input "
"variables $(x_1, x_2, x_3)$, one output variable $out$, and one constant "
"input with a value of $2$."

#: src/plonk-arithmetization.md:24
msgid ""
"一个电路有两种状态：「空白态」和「运算态」。当输入变量没有具体值的时候，电路"
"处于「空白态」，这时我们只能描述电路引线之间的关系，即电路的结构拓扑。"
msgstr ""
"A circuit has two states: \"blank state\" and \"operational state\". When "
"the input variables do not have specific values, the circuit is in the "
"\"blank state\", and we can only describe the relationship between the "
"circuit wires, or the structural topology of the circuit."
```

![](imgs/mdbook%20debug_image_1.png)

![](imgs/mdbook%20debug_image_2.png)

合理推测：文本带 `$ xx $` 的部分，导致整个段落都没有被渲染。

这说明 `before = ["gettext"]` 在 gettext 之前就对文件做了某些变更，导致后续 gettext 无法定位到
```
"电路中有4个变量，其中三个变量为输入变量 $(x_1, x_2, x_3)$ ，一个输出变量 "
"$out$，其中还有一个输入为常数，其值为 $2$。"
```
这句话，从而导致无法渲染英文内容，然后显示的是中文内容

解决方案：**暂无**

### no before = ["gettext"]

```toml
[preprocessor.katex]
# before = ["gettext"]
after = ["links"]
```

![](imgs/mdbook%20debug_image_3.png)

文本带 `$ xx $` 的部分，可以正常渲染了！这说明 gettext 正确索引到内容了。

但是，复杂公式部分失败了。 推测还是 gettext 做了点什么。。。

![](imgs/mdbook%20debug_image_4.png)

这部分内容，考虑到目前 message.pot 和 en.po 里面没有 `\begin ..` 内容的相关信息，也就是说我们为了避免 message.pot 和 en.po 的干扰，故意在 2 个文件中都删除了这个复杂公式相关的内容。

但是他还是渲染失败了，这说明就是：
1. gettext 先启动，生成 en 目录的时候发生了一些什么故事
2. katex 启动，发现无法渲染。

最后挣扎：查看 /en 目录



### 其他解决方法：
1. 修改 gettext（或许） cmd ，让其忽略 `$$` 文本。
2. 
