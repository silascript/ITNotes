---
aliases: []
tags:
  - front-end
  - html
  - html5
  - h5
  - xhtml
  - dom
  - w3c
  - whatwg
created: 2023-08-18 19:44:52
modified: 2025-10-14 23:07:50
---

# Html 笔记

---

## 目录

* [HTML 简介及历史](#html_introducton_history)
	* [XHTML](#html_introducton_xhtml)
	* [HTML5](#html_introducton_html5)
* [HTML 编辑器](#html_editors)
* [HTML 基本概念](#html_basic_concept)
* [基本标签](#html_tag_basic)

---

## <span id="html_introducton_history">HTML 简介及历史</span>

**HTML** 全名叫 「超文本标记语言」（HperText Markup Language）。

1989 年由欧洲核子研究中心的物理学家**蒂姆.伯纳斯.李**（Time Berners-Lee）开发出世界上第一个 Web 服务器和客户机。

1989 年 12 月，蒂姆为他的发明正式定名为 「World Wide Web」。

1991 年 5 月 WWW 在 Internet 上首次露面，并成为了标准。

### HTML 版本时间线

* 1995 年 11 月 24 日 HTML2
* 1997 年 1 月 14 日 HTML3
* 1997 年 12 月 18 日 HTML4
* 1998 年 4 月 24 日 HTML 4.0 微调，不增加版本号
* 1999 年 12 月 24 日 HTML 4.01 作为 [W3C](https://www.w3.org) 推荐标准发布。
* 2014 年 10 月 28 日 HTML 5 作为 [W3C](https://www.w3.org) 推荐标准发布。

### HTML4

### <span id="html_introducton_xhtml">XHTML</span>

 #xhtml

**XHTML** 是 「3 种 [HTML4](#HTML4) 文件根据 XML 1.0 标准重组」而成的。

HTML 语法比较的「自由奔放」，所以在不同的浏览器渲染出现不同的表现。W3C 想要把 HTML 标准纳入 XML 标准中，让「严格」的 XML 来 「规范」HTML，所以就有了 XHTML 这货。

XHTML 应用出现在 2005 年到 2012 年左右的「重构时代」。

### <span id="html_introducton_html5">HTML5</span>

 #html5

HTML 5 实质是 [WHATWG](#WHATWG) 的，[W3C](#W3C) 只是「发布者」。

从 HTML 5 始，W3C 已经失去对 包括 HTML 及 [DOM](../JS/DOM_Note.md) 在内的控制权。
> [!info] 
> 
> 2018 年 W3C 的 DOM 4.1 标准被苹果、Mozilla、Google 及微软四大浏览器厂商反对。  
> 2019 年 5 月 28 日，W3C 宣布 WHATWG 将是 HTML 和 DOM 标准的唯一发布者。

 [WHATWG](#WHATWG) 背后站着的是浏览器厂商。

### W3C

万维网联盟

### WHATWG

WHATWG（WHATWG 正确发音是：[what-wig]）， Web Hypertext Application Technology Working Group，在 2004 年，由 Apple 公司、Mozilla 基金会和 Opera 软件公司所组成。

后来随着 Chrome 浏览器大行其道，Google 也加入其中。再后来连微软了加入的。

WHATWG 是针对 [W3C](#W3C) 网页标准的发展缓慢，以及 [W3C](#W3C) 意图放弃 HTML 转而发展以 XML 为基础的技术而成立。

---

## <span id="html_editors">HTML 编辑器</span>

HTML 本质上是文本，所以对 HTML 文件编辑可以使用「文本编辑器」。  

大概介绍几种常用的文本编辑器：

### <span id="html_editors_notepad">记事本</span>

Windows 自带的，最朴素的文本编辑器。

### <span id="html_editors_sublime">SublimeText</span>

  #editor #sublime 

[SublimeText](../Editors/Sublime_Note.md) 是一款轻量级、顔值高的文本编辑器。最新版本是 [SublimeText4](https://www.sublimetext.com)。

### <span id="html_editors_vim">vim</span>

#editor #vim 

[Vim](../vim/Vim_Note.md) 是 [Linux](../Linux/Linux_Note.md) 系统下最强大的编辑器。 

### <span id="html_editors_vscode">VSCode</span>

 #editor #vscode 

对于有更大项目管理需要的用户，可以使用 [VSCode](../Editors/VSCode_Note.md)。  
虽然 [vim](#vim)，但如果是做一些有点规模的项目时，VSCode 更优秀。

## 其他

免费的编辑器：

还有什么 Adobe 的 [Brackets](https://brackets.io)、Github 的 [Atom](https://atom.io)、[Komodo edit](https://www.activestate.com/products/komodo-ide/downloads/edit/)。

IDE：

当然还有古老又笨重还收费的 Adobe Dreamweaver。  

另外还有个装 X 货 [Hbuilder X](https://www.dcloud.io/hbuilderx.html)。

> [!info] 
> 
> Hbuilder 没有 Linux 版本，如果系统是 Linux 的，就略过了。

当然，还有个重量级成员：[webstorm](https://www.jetbrains.com/webstorm/)。个人十分讨厌 [JetBrains](https://www.jetbrains.com) 全系的 IDE，因为我非常讨厌 Swing。

还有顺便提一句，**Notepad++** 这种**烂货**就不要用了！

---

##  <span id="html_basic_concept">HTML 基本概念</span>

**HTML** 不是一门编程语言，而是一种用于定义内容结构的标记语言。

> [!tip] 
> 
> html 是给页面搭「骨架」的。

### <span id="html_basic_concept_tag">标签</span>

HTML 代码由不同的标签构成。

### <span id="html_basic_concept_element">元素</span>

浏览器渲染网页时候，会把 HTML 源码解析成一个标签树，每个 [标签](#标签) 都是一个节点（node），被称为网页「**元素**」（element）。

> [!info] 
> 
>「标签」和「元素」使用的场合不一样：标签是网页源码的角度来看，而元素是从编程角度来看。

标签和元素的关系：

![tag_element](https://mdn.mozillademos.org/files/16475/element.png)

#### <span id="html_basic_concept_type">元素分类</span>

##### 行内元素

行内元素也称为「内联元素」，inline element。

行内元素不会独占一行，宽度随元素的内容变化而变化。

##### 块级元素

### <span id="html_basic_concept_attribute">属性</span>

**属性** 是标签的额外信息，使用空格与棱名和其他属性分隔。

![attribute](https://mdn.mozillademos.org/files/16476/attribute.png)

---

## <span id="html_tag_basic">基本标签</span>

网页的基本标签有以下这些：

* `<!doctype>`
* `<html>`
* `<head>`
* `<meta>`
* `<title>`
* `<body>`

符合语法标准的网页，应满足以下基本结构：

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title></title>
</head>
<body>
</body>

</html>
```

* `<!DOCTYPE html>` 用来声明文档类型的。

在 xhtml 时代，这个声明是写成类似这样的：

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
```

当初 W3C 就是想把 Html 弄成 XML 的子集，所以使用了与 XML 一样的声明样式。

到了 HTML 5 时代，WHATWG 把 HTML 5 拉回了 html 4 那种比较自由的风格，所以对 XHTML 的风格作了调整，文档声明部分就简化成现在这个样子。

`<!DOCTYPE html>` 这货建议使用大写形式，因为这货本质上不是标签，而是一个处理指令。

* `<html>` 这是网页的根元素。该元素包含整个页面的内容。

该标签有一个 **lang** 属性，表示网页内容默认的语言。

* `<head>` 这个是一个窗口标签，这个标签或元素的内容对用户不可见，用于放置网页的「元信息」，是为了网页渲染做准备的。

`<head>` 有 7 个子元素：
* `<meta>` 设置网页元数据
	`<meta>` 常用属性：
	* `charset` 设置网页编码，如 `<meta charset="utf-8">`
	* `name` 属性，`content` 属性，这两属性以 键 - 值对的方式给文档提供元数据。
		`description` 网页内容的描述 

		`keywords` 网页内容的关键字 

		`author` 网页作者

		`referrer` 控制由当前文档发出的请求的 HTTP Referer 请求头。

		`generator` 生成此页面的软件的标识符

		`theme-color` 设定颜色，其 `content` 值必须是有效的 CSS `<color>` 值。

		示例：
		```html
		<meta name="description" content="HTML 语言入门">
		<meta name="keywords" content="HTML,教程">
		<meta name="author" content="张三">	
		```
* `<link>` 连接外部样式表
* `<title>` 网页标题
* `<style>` 放置内嵌的样式表
* `<script>` 引入脚本
* `<noscript>` 浏览器不支持脚本时，所要显示的内容
* `<base>` 设置网页内部相对 URL 的计算基准

## <span id="html_tag_table">表格</span>

---

## <span id="html_tag_form">表单</span>

网页信息交互最重要的一块就是「**表单**」。表单作为客户与服务器交互的重要媒介，在网络应用中发挥的作用不可替代。

表单作为网页一部分可以采集客户端输入的信息，然后发送给服务器端特定的处理程序这样可以完成服务器端和客户端之间的交互，从而实现用户注册、用户登录、网络投票、在互惠考试等网络应用。

### form 标签

### input 标签

`<input>` 标签用于收集用户输入的数据。

#### 重要属性

##### type

`<input>` 标签有一个重要的属性：`<type>`，这个属性的值决定的 `<input>` 标签的类型，而不同的类型的 `<input>` 标签是有其不同用途。

##### name

`<input>` 标签的 `name` 属性值，是在服务器端处理相应程序中获利该表单的控件值时使用。

#### 单行文本框

单行文本框的 `type` 属性值为 `text`：

```html
<input type="text"  name="" />
```

#### 密码框

密码框的 `type` 属性值为 `password`，当用户在输入内容时，「`*`」来代替显示每个输入的字符，以保证密码的安全性。

示例：

```html

<input type="password"  name="" />
```

#### <span id="html_tag_form_new">HTML5 新 input 类型</span>

#### number

`number` 类型，是一个只能输入数字的输入域。

`number` 类型还能设定所接受数字的限定：
* `min`：最小值
* `max`：最大值

示例：

```html
<input type="number" name="activate_code" id="activate_code" min="1" max="20" />
```

---

## 相关链接

* [Mozilla HTML 教程](https://developer.mozilla.org/zh-CN/docs/Web/HTML)

---

## 相关笔记

* [前端笔记](Front-end_Note.md)
* [前端资料清单](Front-end_Material.md)
* [前端教程视频清单](./Front-end_Videos.md)
* [Css 笔记](CSS_Note.md)
* [Javascript 笔记](../JS/JS_Note.md)

