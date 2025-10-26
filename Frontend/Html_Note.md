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
modified: 2025-10-26 21:09:51
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

### XHTML

 #xhtml

**XHTML** 是 「3 种 [HTML4](#HTML4) 文件根据 XML 1.0 标准重组」而成的。

HTML 语法比较的「自由奔放」，所以在不同的浏览器渲染出现不同的表现。W3C 想要把 HTML 标准纳入 XML 标准中，让「严格」的 XML 来 「规范」HTML，所以就有了 XHTML 这货。

XHTML 应用出现在 2005 年到 2012 年左右的「重构时代」。

### HTML5

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

## 网页标准

Web 标准的内容主要包括 [结构](#结构标准)（Structure）、[表现](#表现标准)（Presentation） 和 [行为](#行为标准)（Behavior）。

### 结构标准

结构用于对网页元素进行整理和分类，结构化标准语言包括 [XML](../XML/XML_Note.md)、HTML、[XHTML](#XHTML)。

### 表现标准

表现标准用于设置网页元素的版式、颜色、大小等外观样式，主要指的是 [CSS](CSS_Note.md)（层叠样式表 Cascading Style Sheet）标准。

### 行为标准

行为标准主要包括对象模型，如 [DOM](../JS/DOM_Note.md)、[ECMAScript](../JS/JS_Note.md) 等。

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

**属性** 是标签的额外信息，属性都是以「键值对」的形式出现，使用空格和其他属性分隔。

![attribute](https://mdn.mozillademos.org/files/16476/attribute.png)

#### 通用属性

##### id

`id` 属性，是一个 [标签](#html_basic_concept_tag) 的**唯一标识**，不可重复。

##### class

`class` 属性，是将一些有相同功能的标签归类的标识，可重复。

> [!info] 
> 
> 类名的第一个字符不能使用数字，并且严格区分大小写，一般采用小写的英文字母。

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

---

## <span id="html_tag_table">表格</span>

一个表格构成需要三个标签：

* `<table>`：一个表格
* `<tr>`：表的一行
* `<td>`：表的一个单元格

示例：

```html
<table>
	<tr>
		<td></td>
		<td></td>
		<td></td>
	</tr>
	<tr>
		<td></td>
		<td></td>
	</tr>
</table>
```

### <span id="html_tag_table_tr">行</span>

#tr

`<tr>`，用于定义表格的一行，必须嵌套在 `<table>` 标签中。

为了对表格行进行更细的语义化，还可以使用 [`<thead>`](#thead)、[`<tbody>`](#tbody) 和 [`<tfoot>`](#tfoot) 三个标签对 `<tr>` 标签进行分组。

示例：

```html

<table>
	<thead>
		<tr>
			<td></td>
			<td></td>
			<td></td>
		</tr>
		<tr>
			<td></td>
			<td></td>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td></td>
			<td></td>
			<td></td>
			<td></td>
		</tr>	
	</tbody>
	
	
</table>
```

#### thead

`<thead>` 用于定义表格的头部，必须位于 [`<table>`](#html_tag_table) 标签中。

一个表格只能定义一对 `<thead>`。

#### tbody

`<tbody>` 用于定义表格的头部，必须位于 [`<table>`](#html_tag_table) 标签中。

一个表格只能定义一对 `<tbody>`。

#### tfoot

`<tfoot>` 用于定义表格的头部，必须位于 [`<table>`](#html_tag_table) 标签中。

一个表格只能定义一对 `<tfoot>`。

### <span id="html_tag_table_td">单元格</span>

#td

`<td>` 用于定义表格中的单元格，必须嵌套在 [`<tr>`](#html_tag_table_tr) 标签中。一个 `<tr>` 中有几个 `<td>`，即表示该行有几列。

#### th

`<th>` 用于定义「表头单元格」,其属性、用法与普通的 `<td>` 完全相同，属性对单元格的**语义化**标签。

> [!tip] 
> 
> `<th>` 中的文本默认是加粗居中显示，这对于普通的单元格 `<td>` 来说，完全可以通过自身的相关属性或 [CSS](CSS_Note.md) 设置达到。所以 `<th>` 对于表格而言，并不是必须的，完全可以只使用普通的 `<td>`。

---

## <span id="html_tag_form">表单</span>

网页信息交互最重要的一块就是「**表单**」。表单作为客户与服务器交互的重要媒介，在网络应用中发挥的作用不可替代。

表单作为网页一部分可以采集客户端输入的信息，然后发送给服务器端特定的处理程序这样可以完成服务器端和客户端之间的交互，从而实现用户注册、用户登录、网络投票、在互惠考试等网络应用。

### form

### <span id="html_tag_form_attributes">form 标签属性</span>

`<form>` 标签有几个重要的属性：

* `action`：表单提交的目的地
* `method`：表单提交方式，一般有两种：`post` 和 `get`

### <span id="html_tag_form_input">input</span>

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

#### <span id="html_tag_form_input_newtype">HTML5 input 类型</span>

#### number

`number` 类型，是一个只能输入数字的输入域。

`number` 类型还能设定所接受数字的限定：
* `min`：最小值
* `max`：最大值

示例：

```html
<input type="number" name="activate_code" id="activate_code" min="1" max="20" />
```

#### email

`<input>` 的 `type` 如果设置为 `email`，那该输入框只能输入 emailk 地址：

```html
<input type="eamil" />
```

#### color

`<input>` 的 `type` 如果设置为 `color`，那它将是一个颜色拾取器：

```html
<input type="color" name="color1" />
```

##### datePickers

日期选择器。有多个可供选取日期和时间类型。

`type` 属性可以选：

* `date`：选取日、月、年，不包括时间
* `month`：选取月、年
* `week`：选取周和年
* `time`：选取时间（小时和分钟）
* `datetime-local`：选取时间、日、月、年（本地时间）

> [!tip] 
> 
> 本来还有个 `datetime` 的，不过后来的标准移除此类型了：[Remove input\[type=datetime\] · Issue #336 · whatwg/html](https://github.com/whatwg/html/issues/336)

```html
<input type="date" name="user_date" id="user_date" />
<input type="datetime-local" name="user_datetimel" id="user_datetimel" />
<input type="month" name="user_month" id="user_month" />
<input type="time" name="user_time" id="user_time" />
<input type="week" name="user_week" id="user_week" />
```

#### <span id="html_tag_form_input_new">HTML5 新 input 标签</span>

##### option

`<option>` 这个标签一般不会单独使用，它是作 [select](#select), [optgroup](#optgroup) 、[datelist](#datelist) 标签的子标签存在。

> [!tip] 
> 
> 其中 `<option>` 标签有个属性 `selected`，是设定该选项默认被选中的，如果未设置此属性，默认选中第一个选项。

##### optgroup

`<optgroup>` 同样也不会单独使用，是作为 [select](#select) 标签的子标签，[option](#option) 的父级标签存在。

它的功能是将多个 `<option>` 进行分组。

##### datelist

`<datalist>` 标签用于定义输入框的选项列表。

选项使用 [option](#option) 标签进行定义。

如果 [单行文本框](#单行文本框) 想要在输入时出现使用 `<datalist>` 定义的数据，可以通过 `list` 属性与 `<datalist>` 的 `id` 值「绑定」：

```html

<input type="text" list="address" />

<datalist id="address">
	<option>上海</option>
	<option>北京</option>
	<option>广州</option>
</datalist>
```

##### select

`<select>` 与 [datelist](#datelist) 类似的控件，区别是不能输入，只能选取。

同样的选项也是 [option](#option) 标签：

```html
<label for="user_province">省份：</label>

<select name="user_province" id="user_province">
	<option value="js">江苏</option>
	<option value="gd" selected="true">广东</option>
	<option value="hn">河南</option>
	<option value="hb">河北</option>
	<option value="sd">山东</option>
	<option value="sx">山西</option>
	<option value="shx">陕西</option>
	<option value="hn">湖南</option>
	<option value="hb">湖北</option>
	<option value="sc">四川</option>
</select>
```

还能对 `<option>` 使用 [optgroup](#optgroup) 标签进行「分组」：

```html
<select>
	<optgroup label="国内">
		<option value="gz">广州</option>
		<option value="sz">深圳</option>
		<option value="bj" selected>北京</option>
	</optgroup>
	<optgroup label="国外">
		<option value="ld">伦敦</option>
		<option value="ny">纽约</option>
		<option value="xn">悉尼</option>
	</optgroup>
</select>
```

---

## 相关链接

* [Mozilla HTML 教程](https://developer.mozilla.org/zh-CN/docs/Web/HTML)

### 检测

* [Can I use](https://caniuse.com)
* [HTML5test](https://html5test.com)

---

## 相关笔记

* [前端笔记](Front-end_Note.md)
* [前端资料清单](Front-end_Material.md)
* [前端教程视频清单](./Front-end_Videos.md)
* [Css 笔记](CSS_Note.md)
* [Javascript 笔记](../JS/JS_Note.md)

