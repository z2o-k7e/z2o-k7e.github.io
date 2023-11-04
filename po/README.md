为什么 

```
# gen message.po
MDBOOK_OUTPUT='{"xgettext": {"pot-file": "messages.pot"}}' \
  mdbook build -d po
```

这个命令生成的结果是不确定的？

- 健康的 message.pot
- 不健康的 message.pot

```rust
#: src/plonk-intro-cn/plonk-arithmetization.md:20
#: src/plonk-intro-cn/plonk-arithmetization.md:20
#: src/plonk-intro-cn/plonk-arithmetization.md:22
#: src/plonk-intro-cn/plonk-arithmetization.md:37
#: src/plonk-intro-cn/plonk-arithmetization.md:43
msgid "1"
msgstr ""
```
目前的情况是，只要是碰到 $$ 开头的公式就基本完蛋了。

	




<del>目前所有文章的 en.po 但是感觉太多了，翻译不工作，先挑 PLONK 翻吧

受限于翻译规模，目前 en.po 仅包含 PLONK 部分内容

- messages.pot.plonk :  单独把 plonk 部分内容 cut 到另一个 folder 做出来的
- en.po.all  之前 hack 的一版本翻译，所有的文章都翻译了，但是质量很差</del>
