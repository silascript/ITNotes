---
aliases: []
tags:
  - markdown
  - test

created: 2023-01-13 12:27:45
modified: 2023-02-02 4:14:05
---

# Markdown 测试

---

这个文档主要用来测试 Markdown 一些小细节用的。

这里主要测试 Markdown 链接在 Obsidian 中及 [github](https://github.com) 中的差异。

Obsidian 使用标题作为「锚点」时，链接中使用 `%20` 来替代空格。

而如果手动设置使用 `-` 来替代空格，则符合 [github](https://github.com) 的链接要求。

---
## 目录

Obsidian 式（引用「锚点」时，使用 `%20` 来替代空格）
* [TestTiltle1](#TestTiltle1)
* [Test Title2](#Test%20Title2)
* [testTitle3](#testTitle3)
* [test Title4](#test%20Title4)
* [Test标题5](#Test标题5)
* [Test 标题6](#Test%20标题6)
* [test标题7](#test标题7)
* [test 标题8](#test%20标题8)
* [Test 标题9](#Test%20标题9%20test-9)

Github 式（引用「锚点」时将空格使用 `-` 来替代）
* [Test Title2](#Test-Title2)
* [test Title4](#test-Title4)
* [Test 标题6](#Test-标题6)
* [test 标题8](#test-标题8)
* [Test 标题9](#test-9)

---

## TestTiltle1

大写无空格的标题

---

## Test Title2

大写有空格标题。

---

## testTitle3

小写无空格标题

---

## test Title4

小写有空格标题

---

##  Test 标题 5

大写中英混非无空格标题

---

## Test 标题 6

大写中英混排有空格标题

---

## test 标题 7

小写中英混排无空格标题


---

## test 标题 8

小写中英混排有空格标题

---


## Test 标题 9 {#test-9}


使用 `{#id}` 的方式指定标题 id


[github](https://github.com) 最终转换成 HTML 的源码是这样的：

```html
<a id="user-content-test-标题9-test-9" class="anchor" aria-hidden="true" href="#test-标题9-test-9">
```

---




## 测试结果

不出意外，[github](https://github.com) 上所有使用 `%20` 来代替空格的「锚点」全都跳转失效，原因就是在标题部分，[github](https://github.com) 的生成 html 策略是，将空格转成 `-` 来替代生成相应的 `a` 标签的 `href` 值，而在目录索引部分对以标题作为「锚点」的链接，[github](https://github.com) 「原封不动」,空格还是空格。

当然去查看 html 源码，并不是像 [Obsidian](https://obsidian.md/) 中那样显示为 `%20`，因为 `%20` 是跟 [ASII 码](https://baike.baidu.com/item/ASCII/309296) 相关的，**20** 这个数字，不是十进制，而是十六进制，其对应的键位就是「空格键」，在 Markdown 文档中写 `%20` ，被 [github](https://github.com) 转换后，会成为空格。


`%20` 在 Obsidian 中能够预览并跳转成功，原因是 Obsidian 阅读视图处理预览的机制跟 [github](https://github.com) 不同。



