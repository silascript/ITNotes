---
aliases: []
tags:
  - latex
  - markdown
  - maths
created: 2026-06-07 20:52:15
modified: 2026-06-07 21:03:45
---

# LaTex 笔记

---

## 简介

LaTeX 是一种基于 TeX 的排版系统，由图灵奖得主 Lamport 开发，而 Tex 则是由 Knuth 最初开发，这两位都是计算机界的巨擘。当然开发者强并不是我们学习 LaTeX 的理由，LaTeX 和常见的所见即所得的 Word 文档最大的区别就是用户只需要关注写作的内容，而排版则完全交给软件自动完成。这让没有任何排版经验的普通人得以写出排版非常专业的论文或文章。

---

## 数字公式

### 基础语法

* 命令：以反斜杠 `\` 开头，如 `\alpha`、`\sum`
* 参数：用花括号 `{}` 包围，如 `\frac{a}{b}`
* 下标：使用 `_`，如 `x_1`
* 上标：使用 `^`，如 `x^2`
* 分组：用花括号将多个字符组合，如 `x_{i+1}`

### 常用命令

* `\alpha`, `\beta`, `\gamma` 希腊字母
* `\sum`, `\prod`, `\int` 求和、乘积、积分
* `\frac{分子}{分母}` 分数
* `\sqrt{表达式}` 平方根
* `\sqrt[n]{表达式}`       n 次根

---

## 相关笔记

* [LaTex 资料清单](LaTex_Material.md)
* [Markdown 笔记](Markdown_Note.md)