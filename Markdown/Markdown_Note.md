---
aliases: []
tags:
  - markdown
  - editor
  - obsidian
  - typora
  - mermaid
created: 2023-01-13 12:27:45
modified: 2024-05-24 11:22:29
---

# Markdown 笔记

轻量级标记语言。比 html 简单多了，非常适合不会 html 的人写文档。

Markdown 比 doc 等文档更开放，更易于发布。

---

## 目录

* [基础](#md_basic)
  * [关于锚点](#关于锚点)
	  * [相关测试](#相关测试)
* [高级](#md_advance)
	* [折叠](#md_advance_collapse) 
	* [画图](#md_advance_draw)
	* [Mermaid图形种类](#Mermaid图形种类)
	* [Mermaid 常用语法](#Mermaid-常用语法)
		* [mermaid 中连接线的类型](#mermaid-中连接线的类型)
	* [Markdown相关教程](#Markdown相关教程)
* [扩展](#md_extra)
	* [使用 Font Awesome](#md_extra_fontawesome)
	* [徽章相关](#md_extra_badge)
* [相关工具](#md_tools)
	* [Markdown 编辑器](#md_tools_mdeditors)
		* [Obsidian](#md_tools_mdeditors_obsidian)
		* [Joplin](#md_tools_mdeditors_joplin)
---

## <span id="md_basic">基础</span>

### <span id="md_basic_title">标题</span>

使用 `#` 号来表示标题。标题共有 6 个级别，分别以对于数量的 `#` 来表示。

```markdown
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题

```

### <span id="md_basic_anchor">关于锚点</span>

> [!example] 示例 1
> 
> ```markdown
>	* [基础设置](#基础设置)
>		* [文件与链接](#文件与链接)
>			* [设置内部链接类型](#设置内部链接类型)
>		* [外观](#外观)
>	* [Obsidian 配置目录](#obn_config_dir)
>	```
>	
> ```markdown
>	## <span id="obn_config_settings_basice">基础设置</span>
>		### <span id="obn_config_settings_basice_filelinks">文件与链接</span>
>			#### 设置内部链接类型
>			### <span id="obn_config_settings_appearance">外观</span>
>		##  <span id="obn_config_dir">Obsidian 的配置目录</span>
> ```
> 
> 
> 这个例子使用了两种方式对「锚点」的定义与引用。
> 1. 使用 `<span>` 这个行内标签，并且使用 `id` 属性为其定义「锚点」值。
> 2. 什么都没加，直接就是原生的 Markdown 语法 `#` 符号定义标题。 
> 
> 而引用「锚点」时，同样也使用了两种方式：
> 1. 对于部分使用了 `id` 指定「锚点」值的标题，使用 `#标题` 方式来引用「锚点」。
> 2. 对于标准 Markdown 语法，即使用 `#标题` 方式定义标题来说，只能使用 `#标题` 方式来引用「锚点」。
>
> 来看下在 [Obsidian](../NoteSoft/Obsidian/Obsidian_Note.md) 及 [github](https://github.com) 上的效果：
> 1. Obsidian 中使用 `<span id>` 这种方式定义的锚点，并在引用时使用 `#id` 的方式引用，Obsidian 找不到锚点。
> 2. github 上无论哪种方式引用「锚点」都能实现链接跳转。
>   
> 顺便看下，[github](https://github.com) 渲染成 html 后，html 源码成了什么：
> 
> ```html
>	<li><a href="#%E5%9F%BA%E7%A1%80%E8%AE%BE%E7%BD%AE">基础设置</a></li>
>	<li><a href="#%E6%96%87%E4%BB%B6%E4%B8%8E%E9%93%BE%E6%8E%A5">文件与链接</a></li>
>	<li><a href="#%E8%AE%BE%E7%BD%AE%E5%86%85%E9%83%A8%E9%93%BE%E6%8E%A5%E7%B1%BB%E5%9E%8B">设置内部链接类型</a></li>
>	<li><a href="#%E5%A4%96%E8%A7%82">外观</a></li>
>	<li><a href="#obn_config_dir">Obsidian 配置目录</a></li>
> ```  
>
> 可以发现，`<span>` 在经过 [github](https://github.com) 渲染转换之后，生成的是 `<a>` 标签。
> 
>  使用 `id` 属性定义的「锚点」并使用 `#id` 方式引用的「锚点」，在经过渲染转换后，是非常标准的 html 「锚点」引用语法。
>  
>  但使用 `#标题` 方式引用「锚点」，[github](https://github.com) 就将中文标题「encoding」 了，可读性变差，但功能还是能实现的，这就是为什么 [github](https://github.com) 下两种方式都能实现跳转功能。
>
>  如果想要在 [Obsidian](../NoteSoft/Obsidian/Obsidian_Note.md) 中使用指定锚点 `id` 值来引用「锚点」，可以使用 `^` 符号来引用：
> 
>> [!example] 示例 1.1
>> ![md_anchor_1](./Markdown_Note.assets/md_anchor_1.png) 
>> 而出来是 ` [外观](Obsidian_Note.md#^f53495)` 这种效果，[Obsidian](../NoteSoft/Obsidian/Obsidian_Note.md) 生成一个值来引用这个「锚点」,虽然不像之前使用 `#id` 方式引用出现 `unable to find section` 问题，同样也是没在预览窗口实现「锚点」内容的「完全」预览（说预览不完全是因为，它只预览出标题，但没有预览出标题「周围」的内容，这也体现了 Obsidian `#` 与 `^` 两种引用链接方式的不同），但是能够实现「锚点」跳转。
>>> [!bug] 问题
>>> `#^` 这种方式的引用在 [Obsidian_Note](../NoteSoft/Obsidian/Obsidian_Note.md) 中跳转没问题，但在 [github](https://github.com/) 中就不能实现「锚点」跳转功能了，因为生成的链接的那个「值」是 Obsidian 自己特有的。 

> [!bug] 空格问题
> 如果标题中存在空格，在使用 `#标题` 方式引用时，Obsidian 中预览及跳转都没问题，但在 [github](https://github.com/) 是未能跳转成功的。
>

> [!summary] 总结
> 现阶段，既要能在 Obsidian 中跳转成功，又能在 [github](https://github.com/) 中跳转成功，只能使用 `#标题` 方式。
> 
> 另外，中英混排的标题不能有空格，不然文档在 [github](https://github.com/) 下是不能跳转 -- 其实在 [VSCode](https://code.visualstudio.com/) 使用 [Markdown预览插件](../Editors/Editors_Note.md#editors_vscode_extensions_markdown-preview-enhanced) 预览文档，这种有空格的中英混排标题，同样是不能完成跳转功能的。
>
>> [!help] 解决方案
>> 1. 为了能使 [Obsidian](../NoteSoft/Obsidian/Obsidian_Note.md) 的链接能够实现预览及跳转，而且更能体现 Markdown 的简单原则，在引用「锚点」时，尽量使用 `#标题` 这种更具备「语义性」的引用方式。
>> 2. 在定义「标题」时，尽量不要使用中英混排的标题。
>> 3. 在定义「标题」时，如果非要用到中英混排，尽量不要在其中使用 **空格**。
>> 4. 在定义「标题」时，如果一定要中英混排而且还包含 **空格**，那有两种处理方案：
>> 	* 在引用这个「锚点」时，使用 `-` 来代替 **空格**，如 `[mermain 中连接线的类型](#mermain-中连接线的类型)`，这种方式只适合用于中英混排中英文是小写字母，如果英文出现大写字母，那这种方式也 ***可能*** 没用。
>> 	>>> [!tip] 各种预览对于大小写字母处理
>> 	>>>	因为不同的渲染器转换器，出来的效果是有差异的。 
>> 	>>>  * [github](https://github.com) 正常跳转。
>> 	>>>  * [Typora](https://www.typoraio.cn/) 正常跳转。
>> 	>>>  * [VSCode](../Editors/Editors_Note.md#editors_vscode) 的 [Markdown Preview Enhanced](../Editors/Editors_Note.md#Markdown-Preview-Enhanced) 插件中的预览效果，跳转不成功。
>> 	>>>  * 使用浏览器 Markdown 插件进行 Markdown 文件预览时，跳转不成功。
>> 	>>>   
>> 	>>>  从根上分析，HTML 文档中的 **标签名** 和 **属性名** 应该都是大小写 **不敏感** 的，按道理是不应该存在什么大小写而出现「锚点」跳转不了的问题，但事实就是有些预览方式存在这个问题。
>> 	>>>
>> 	>>>>[!help] 原理
>> 	 >>>>  
>> 	 >>>> 诸如 [github](https://github.com) 在对 Markdown 文件进行渲染转换成 html 时，会将 **空格** 转换成 `-`，这才是引用「锚点」时，可以使用 `-` 实现锚点跳转的原因所在。
>> 	>>>> 
>> 	>>>> 至于大写字母不生效的原因也很奇怪，因为 [github](https://github.com) 会将所有「锚点」定义的地方的 html `<a>` 标签的 `href` 值中英文都从大写字母都转成小写字母，但引用「锚点」处的 `<a>` 标签的 `href` 属性值它没有转，这就是跳转出问题的可能原因，说「可能」，上面已经分析过了，HTML 标签中的属性值压根就不存在大小写敏感，而实际在 [github](https://github.com) 测试，大写字母做标题，引用这「锚点」时，是能成功跳转的，这也侧面证明了 HTML 标签属性值大小写不敏感这个事实。
>> 	>>>> 
>> 	>>>> ```html
>> 	>>>> <!-- 这个引用锚点 href  值就没有转成小写 #Mermaid-->
>> 	>>>> <a href="#Mermaid-%E5%B8%B8%E7%94%A8%E8%AF%AD%E6%B3%95">Mermaid 常用语法</a>
>> 	>>>> <!-- 看到了吧，这是锚点定义的地方，即标题定义部分，它的 href 属性值 已经从大写转成小写 #mermaid -->
>> 	>>>> <a id="user-content-mermaid-常用语法" class="anchor" aria-hidden="true" href="#mermaid-常用语法">Mermaid 常用语法</a>
>> 	>>>> ```
>> 	>>>> 
>>  
>> 	 * 如果不确定标题存在空格及英文大小写，造成引用「锚点」时跳转是否正常，那就直接使用 `#id` 这种对 [Obsidian](../NoteSoft/Obsidian/Obsidian_Note.md) 不太「友好」的方式来引用「锚点」。
> 

#### 相关测试

关于链接及「锚点」，这个 [测试页面](Markdown_Test.md) 能说明很多东西。

---

### <span id="md_basic_esc">转义字符</span>

Markdown 使用 `\` （反斜杠）来对一些特殊字符进行转义。当然如果觉得麻烦，还能使用 ASII 码直接显示。

比如要显示**\`**时，可以写成 **\\\`**，也可以写成 `&#96;`。

> [!tip] 使用 ASII 码的小问题
> 如使用 ASII 码显示特殊字符时，最后不要省略分号 `;`，虽然在 [Obsidian](../NoteSoft/Obsidian/Obsidian_Note.md) 是能显示，但像 [Typora](https://typoraio.cn/) 等编辑器中是不能正常显示的，所以为了兼容性，最好把分号加上。

### <span id="md_basic_qoate">引用</span>

引用文本块使用 `>` 来标识。

```markdown

> 这一个段文本引用块
```

#### <span id="md_basic_qoate_extension">引用块扩展</span>

引用块最常用的扩展就是在引用块上加上一些「美化」标识，这种扩展语法不是 Markdown 标准语法，而是扩展语法。就如 [Obsidiane](../NoteSoft/Obsidian/Obsidian_Note.md) 中的 [Callout Blocks](../NoteSoft/Obsidian/Obsidian_Note.md#obn_syntax_calloutblocks) 类似。而现今使用最多的是 [GFM](https://github.github.com/gfm/) （GitHub Flavored Markdown）已经支持了类似功能。不过 Github 没 Obsidian 的样式这么丰富。

GFW 支持 5 种引用扩展样式：

* NOTE
* TIP
* IMPORTANT
* WARNING 
* CAUTION

> [!tip] 相关链接
>
> * [An option to highlight a "Note" and "Warning" using blockquote (Beta) #16925](https://github.com/orgs/community/discussions/16925)

---

## <span id="md_advance">高级</span>

### <span id="md_advance_collapse">折叠</span>

想要使部分内容「折叠」起来的效果，可以使用 `<details>` 标签。

```markdown
<details>

折叠测试

</details>
```

其实 `<details>` 标签不是 markdown 语法，而是 [Html](../Frontend/Html_Note.md) 的东西。

而一般与 `<details>` 标签一起使用的，还有其子标签 `<summary>` 标签，作为 `<details>` 标签的「标题」。

```markdown
<details>
<summary>折叠标题</summary>

折叠测试

</details>

```

---

## <span id="md_tutorial_links">Markdown 相关教程</span>

* [obsidian 论坛  成雙醬 的Markdown 教程](https://forum-zh.obsidian.md/t/topic/435)
* [https://www.markdown.cn](https://www.markdown.cn)
* [Github Markdown 规范](https://gfm.docschina.org/zh-hans/)

---

### <span id="md_advance_draw">画图</span>

在 Markdown 中绘图，使用的 [Mermaid](https://github.com/mermaid-js/mermaid) 这个工具。 

最重要的是，[github](https://github.com) 官方已原生支持！

Mermaid 是一种基于 Javascript 的通过代码创建图表的工具，其使用类似于 Markdown 的语法。

在 Markdown 中使用 Mermaid 是通过代码块来实现的，只需在代码块的语言指定中指定为 `mermaid` 就能在 Markdown 中使用 Mermaid 来展示各类图表。

#### Mermaid 图形种类
* 流程图：使用 `flowchart` 或 `graph` 关键字
* 序列图：使用 `sequenceDiagram` 关键字
* 甘特图：使用 `gantt` 关键字
* 类图：使用 `classDiagram` 关键字
* 饼状图：使用 `pie` 关键字
* 状态图：使用 `stateDiagram` 关键字
* 用户旅程图：使用 `journey` 关键字

#### Mermaid 常用语法

##### mermaid 中连接线的类型

| 类型 | 描述 |
| :---: | :---: |
| -> | 无箭头实线 |
| --> | 无箭头虚线 |
| ->> | 带箭头实线 |
| -->> | 带箭头虚线 |
|  -x | 十字箭头的实线 |
| --x | 十字箭头的虚线 |
| -) | 开放箭头的实线 |
| --) | 开放箭头的虚线 |

##### 流程图
mermaid 有两种流程图：flowchart 流程图和 graph 流程图。

###### flowchart 流程图

flowchart 流程图的风格偏传统流程图。

优点：显示效果比较朴素并可以较方便的调节引导线方向和流程图布局。

缺点：语法过分复杂

使用 `flowchart` 声明这个图是 flowchart 类型的图。

然后跟着这个图的方向。
* T：Top
* B：Buttom
* D：Down
* L：Left
* R：Right

所以两两组合就指定了这个图的方向：
* TB 或 TD：从上到下
* BT：从下到上（没有 “DT”，只能是 `BT`）
* LR：从左到右
* RL：从右到左

语句可以加 `;`，也可以省略。
```mermaid
flowchart LR;

Start --> Stop
```

###### graph 流程图
graph 流程图相较 flowchart 最大优点 **语法简单**。

##### 时序图

时序图也称为序列图。

mermaid 中 时序图使用 `sequenceDiagram` 来声明。

节点有两种：
* `participant`：默认样式
* `actor`：显示一个小人

使用 `as` 关键字来为节点起「别名」。

简单示例：
```mermaid
sequenceDiagram;
participant A1 as a1;
actor B1 as b1; 
A1->B1: Hello
B1->A1: Hi


```

##### 甘特图

##### 饼状图

简单示例：

```mermaid
pie
"猫": 20
"狗": 30
"仓鼠": 5
```

> 更多更详细的 Mermaid 的 [文档](https://mermaid-js.github.io/mermaid/#/) 。

---

## <span id="md_extra">扩展</span> 

### <span id="md_extra_fontawesome">使用 Font Awesome 来加图标</span>
Font Awesome 使用步骤：

1. 引入 css
在 markdown 文件最后引入 Font Awesome 文件。
```html
<head>
<link rel="stylesheet" href="https://cdn.bootcdn.net/ajax/libs/font-awesome/6.1.0/css/all.min.css">

</head>
```
> 可以找下国内的 CDN。  
> 常用 CDN：  
> [bootcdn](https://www.bootcdn.cn)

2. 插入符号 

示例：

> 各版本的写法有区别

5.x

<i class="fab fa-weibo"></i> `<i class="fab fa-weibo"></i>`

6.x

<i class="fa-brands fa-weibo"></i> `<i class="fa-brands fa-weibo"></i>`

<i class="fa-brands fa-weixin"></i> `<i class="fa-brands fa-weixin"></i>`

##### 调节符号尺寸

示例：

把微信的符号调大

<!-- <i class="fa-brands fa-weixin fa-2x"></i> `<i class="fa-brands fa-weixin fa-2x"></i>` -->
<i class="fab fa-weixin fa-2x"></i> `<i class="fab fa-weixin fa-2x"></i>`

尺寸参数有下列这些：

* fa-2xs <i class="fab fa-weixin fa-2xs"></i>
* fa-xs <i class="fab fa-weixin fa-xs"></i>
* fa-sm <i class="fab fa-weixin fa-sm"></i>
* fa-lg <i class="fab fa-weixin fa-lg"></i>
* fa-xl <i class="fab fa-weixin fa-xl"></i>
* fa-2xl <i class="fab fa-weixin fa-2xl"></i>
* fa-2x 至 fa-10x
	* fa-2x <i class="fab fa-weixin fa-2x"></i> 
	* fa-3x <i class="fab fa-weixin fa-3x"></i> 
	* fa-4x <i class="fab fa-weixin fa-4x"></i> 
	* fa-5x <i class="fab fa-weixin fa-5x"></i> 
	* fa-6x <i class="fab fa-weixin fa-6x"></i> 
	* fa-7x <i class="fab fa-weixin fa-7x"></i> 
	* fa-8x <i class="fab fa-weixin fa-8x"></i> 
	* fa-9x <i class="fab fa-weixin fa-9x"></i> 
	* fa-10x <i class="fab fa-weixin fa-10x"></i> 

| 相对尺寸 | 字体尺寸 | 同等像素值 |
|:--------:|:--------:|:----------:|
|  fa-2xs  | 0.625em  |    10px    |
|  fa-xs   |  0.75em  |    12px    |
|  fa-sm   | 0.875em  |    14px    |
|  fa-lg   |  1.25em  |    20px    |
|  fa-xl   |  1.5em   |    24px    |
|  fa-2xl  |   2em    |    32px    |

符号默认大小是 16px。

##### 让符号转动 
只要加入 `fa-span` 就可以让符号转起来。

示例：

<i class="fab fa-weixin fa-spin"></i> `<i class="fab fa-weixin fa-spin"></i>`

具体的符号列表及写法请参数：[Font Awesome 符号列表](https://fontawesome.com/icons)

---

### <span id="md_extra_badge">徽章相关</span>

进入 [shields.io](http://www.shields.io/) ，在输入框中输入 「lable」、「messge」和颜色，点击「Make Badge」，网站就能帮你生成一个徽章。

![md_badge_shields](./Markdown_Note.assets/md_badge_shields.png)

新生成的徽章：
![md_badge_new](./Markdown_Note.assets/md_badge_new.png)

地址栏中的地址就是就个徽章的网址。你可以嵌入你的页面中。就如像下面这样：

![silascript-hello](https://img.shields.io/badge/silascript-hello-blue) `![silascript-hello](https://img.shields.io/badge/silascript-hello-blue)`
> 跟图片写法一致。

以上演示制作的徽章是是静态的。[shields.io](http://www.shields.io/) 还能生成动态的徽章。具体玩法，网站有介绍。

除了 [shields.io](http://www.shields.io/) 外，还有其他类似的网站也能制作徽章，使用方式大同小异。 

徽章制作网站列表：
* [shields.io](http://www.shields.io/)
* [badgen](https://badgen.net)
* [forthebadge](https://forthebadge.com)
* [badge.fury](https://badge.fury.io)

---

## <span id="md_tools">相关工具</span>

### <span id="md_tools_preview">预览工具</span>

#### Python-Markdown

[markdown](https://github.com/Python-Markdown/markdown) 是 python 写的一个 markdown 转换工具。

通过 [pip](../Python/Python_Note.md#python_pip) 来安装：

```shell
pip install markdown
```

当然这个工具是有可执行程序的，所以是可以使用 [pipx](../Python/Python_Note.md#python_pipx) 来安装：`pipx install markdown`。

#### markdown2

[python-markdown2](https://github.com/trentm/python-markdown2) 跟上面那个类似。

同样也是可以通过 [pip](../Python/Python_Note.md#python_pip) 来安装：

```shell
pip install markdown2
pip install markdown2[all]  # to install all optional dependencies (eg: Pygments for code syntax highlighting)
pypm install markdown2      # if you use ActivePython (activestate.com/activepython)
easy_install markdown2      # if this is the best you have
python setup.py install
```

同理，这工具也能使用来安装 [pipx](../Python/Python_Note.md#python_pipx)：`pipx install markdown2`。

---

### <span>格式化器</span>

#### mdformat

[mdformat](https://github.com/executablebooks/mdformat) 是一个 [Python](../Python/Python_Note.md) 写的 markdown 格式化工具。

可以使用 [pip](../Python/Python_Note.md#python_pip) 或 [pipx](../Python/Python_Note.md#python_pipx) 来安装。

```shell
pipx install mdformat
```

mdformat 默认如果只装这个，安格式化的风格是标准的 Markdown 风格。这个格式化器，有其他风格的库可以安装，如 [GFM](https://github.github.com/gfm/) 风格的 `mdformat-gfm`，可以通过 `pip` 安装：

```shell
pip install mdformat-gfm
```

官方是建议你把几个支持库都装了：

```shell
pip install mdformat-gfm mdformat-frontmatter mdformat-footnote
```

##### 配置

在用户根目录下，新建 `.mdformat.toml` 配置文件。这是 mdformat 的默认配置文件。

```toml

number = true # 有序列表按数字排序
```

相关配置参考 [Configuration file - mdformat documentation](https://mdformat.readthedocs.io/en/stable/users/configuration_file.html)

##### 相关资料

* [mdformat | VitePress](https://shellraining.github.io/tools/mdformat)

---

### <span id="md_tools_mdeditors">Markdown 编辑器</span>

#### <span id="md_tools_mdeditors_typora">Typora</span>

收费了！不介绍了！

#### <span id="md_tools_mdeditors_obsidian">Obsidian</span>

[Obsidian](https://obsidian.md) 这款编辑器，个人觉得完全可以替代 Typora。从功能上更强于 Typora 。

Obsidian 设置更丰富，而且更自由，更换配色主题比 Typora 方便多了！

Obsidian 是 windows、macos 和 Linux 三平台都支持的编辑器。Linux 下它依赖 electron，有 Snap 版和 Appimage 版本。

Obsidian 具体使用请参考：[Obsidian 笔记](../NoteSoft/Obsidian/Obsidian_Note.md)

#### <span id="md_tools_mdeditors_joplin">Joplin</span>

[Joplin](https://joplinapp.org) 这款编辑器，全平台支持，有桌面版、手机版，甚至命令行版 -- 这有点过分了。但个人感觉没有 Obsidian 这么惊艳！

<head>
	<!-- <script defer src="https://cdn.bootcdn.net/ajax/libs/font-awesome/5.15.4/js/all.min.js"></script> -->
	<!-- <script defer src="https://cdn.bootcdn.net/ajax/libs/font-awesome/5.15.4/js/v4-shims.min.js"></script> -->
	<link rel="stylesheet" href="https://cdn.bootcdn.net/ajax/libs/font-awesome/5.15.4/css/all.css" >
</head>

---

## 相关资料

* [互联网那些事儿 | 扒一扒互联网Markdown的那些事儿](https://cloud.tencent.com/developer/article/2355142)
* [unified - 一个用于处理markdown的解析器 - 掘金](https://juejin.cn/post/7193215092316438589)

