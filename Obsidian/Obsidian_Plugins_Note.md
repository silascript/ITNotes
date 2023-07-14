---
aliases:
  - 
tags:
  - obsidian
  - plugin
created: 2023-06-28 17:02:25
modified: 2023-07-15 01:38:50
---
# Obsidian 部分插件笔记

---

## 目录

* [Dataview](Obsidian_Plugins_Note.md#obp_dataview)
* [相关链接](Obsidian_Plugins_Note.md#obp_aboutlinks)

---

## <span id="obp_dataview">Dataview</span>

[Dataview](https://github.com/blacksmithgu/obsidian-dataview) 是一个数据查询插件。用它来整理数据资料非常方便。

它使用代码块来实现相应的功能：

dataview 语法：
```text
	```dataview
	
	<QUERY-TYPE> <WITHOUT ID> <字段>
	FROM <来源>
	<WHERE> <条件表达式>
	<SORT> <排序依据 排序方式>
	<GROUP BY> <分组依据>
	<LIMIT> <限定显示记录数>
	<FLATTEN> <拆分表达式>
	
	```
```

## <span id="obp_templater">Templater</span>

### 设置

#### 不同目录指定特定的模板

General Settings 中有个设置「Template folder location」，这个是设置模板文件默认或全局目录位置。

但有一种需求是，我想在某个目录下新建笔记时，不使用右键选「Create Note from Template」后再选择相应模板这两个操作，而直接使用 Obsidian 新建笔记动作，而笔记自动套用相应的模板文件生成新笔记，那就得使用到 Templater 中的我翻译为「新建文件触发器」的功能。

要使用这个「触发器」功能得做以下设置：
1. 打开「Trigger Templater on new file creation」选项
2.  当「Trigger Templater on new file creation」选项被打开后，就会出现「Add New」设置栏。它可以新增目录与模板对应关系
3.  在「Add New」设置栏中，点击「+」，就能新建一个对应项：前面的是要生成新笔记的目录，后面是要使用到的模板文件

> [!tip] 触发条件
> 只能在右键「新建笔记」时，套用模板文件的触发器才能生效。如果是点击「文件列表」顶部那个「新建笔记」按钮，是不会触发的。
> 
> 原因，大概是「文件列表」**顶部**那个「新建笔记」按钮是「全局」的，新键笔记前「焦点」可能未落在指定目录，而使得触发条件不明，因为 Obsidian 的「新建笔记」有三个选项：「根目录」、「当前文件所在目录」和「指定附件目录」，所以使用 `Ctrl+N` 、`Ctrl+Shift+N` 或顶部「新建笔记」按钮新建笔记时，焦点并未落在指定的目录，所以 Templater 的新建笔记触发器未能触发成功。
> > [!tip]
> > 这极有可能是因为 Obsidian 的设计哲学造成的，因为 Obsidian 最亮点功能是「双链」，这个功能的出现，使得原来使用目录方式来分类笔记的方式被「平面化」了，至少目录层级会减少。所以焦点这块，Obsidian 做得非常烂，即便当前文件焦点都已经失去，使用快捷键将操作转到「文件列表」上，文件列表焦点都不存在，如果这里使用 `Ctrl+N` 或按顶部「新建笔记」按钮，只会设置里设定的新建笔记的三个地方新建笔记，如设置为「当前文件所在目录」，那只会在编辑区正显示的文件所在的目录中新建笔记，即便这个「当前文件」的焦点已经失去了。
> 
> 右键「新键笔记」是针对指定目录的，所以触发目标更明确，满足了触发条件，所以这个方式是能生效。

### 编写

要调用 [Javascript](../JS/JS_Note.md) 函数时，使用 `<%* %>`。

如果以 `%>` 结束代码块，会在生成文件时，多加一个空行。如果不想多个空行，就使用 `-%>` 来结束代码块。

#### 内置函数

##### 日期

##### 文件

###### 查询模板

```javascript
tp.file.find_tfile(模板名称)
```

###### 新建文件

```javascript
tp.file.create_new(template: TFile ⎮ string, filename?: string, open_new: boolean = false, folder?: TFolder)
```

> [!info] 参数详解
> `template`：模板。可以是 TFile 类型，也可以是字符串
> 如果使用 `TFile` 模型，就得使用 [查询模板](#查询模板) 函数。
> 
> `open_new`：创建完新文件是否打开新文件，默认是不打开
> 
> `folder`：新文件存放的目录，`TFile` 类型，使用 [查询模板](#查询模板) 函数获取。

##### 系统

### 相关链接

* [Obsidian API](https://github.com/obsidianmd/obsidian-api)
* [Templater Snippets - shabeblog](https://shbgm.ca/blog/obsidian/Templater+Snippets)

---

## <span id="obp_aboutlinks">相关链接</span>

* [Obsidian 笔记](Obsidian_Note.md)
* [Obsidian常用插件清单](Obsidian_Plugins_List.md)
* [Obsidian 相关视频](Obsidian_Videos.md)