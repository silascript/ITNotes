---
aliases: []
tags:
  - notesoft
  - obsidian
  - plugin
created: 2023-06-28 17:02:25
modified: 2025-02-25 02:05:08
---

# Obsidian 部分插件笔记

---

## 目录

* [Dataview](#obp_dataview)
* [Templater](#obp_templater)
* [相关链接](#obp_aboutnotes)

---

## <span id="obp_dataview">Dataview</span>

[Dataview](https://github.com/blacksmithgu/obsidian-dataview) 是一个数据查询插件。用它来整理数据资料非常方便。

### 设置

将「Date format」和「Date+time format」就是日期和时间两个设置设置成习惯的样式好了。

个人喜欢将其设置成以下这样：

* `yyyy-MM-dd`
* `yyyy-MM-dd HH:mm:ss`

> [!important] 
> 
> 别把日期中 `yyyy` 和 `dd` 打成大写的，会造成显示错误的

> [!tip] 
> 
> 如果有使用 [Linter](Obsidian_Note.md#obn_plugins_commp_linter) 插件，建议将日期及时间格式保持两个插件相同的样式设置，免得如果要 dataview 在对 [front matter](Obsidian_Note.md#obn_advanced_frontmatter) 格式化后，Dataview 根据日期条件筛选到的数据不准确。

### 语法

它使用代码块来实现相应的功能：

dataview 语法：

```text
<QUERY-TYPE> <WITHOUT ID> <字段>
FROM <来源>
<WHERE> <条件表达式>
<SORT> <排序依据 排序方式>
<GROUP BY> <分组依据>
<LIMIT> <限定显示记录数>
<FLATTEN> <拆分表达式>
```

### front-matter 检索

#### 图片

如果 [front matter](Obsidian_Note.md#obn_advanced_frontmatter) 中使用到图片，语法如下：

`key: "![xxx](图片地址)"`

其实就是将 [Markdown_Note](../../Markdown/Markdown_Note.md) 图片的值用双引号 `"` 括起来，而这个图片值中是可以支持 [Obsidian 专用语法](Obsidian_Note.md#obn_syntax) 中对 [图片大小进行控制的语法](Obsidian_Note.md#obn_syntax_imgsize)。

示例：`cover: "![JavaScript 高级程序设计 4th cover|60x70](https://img9.doubanio.com/view/subject/l/public/s33703494.jpg)"`

更多细节参考： [Dataview document](https://blacksmithgu.github.io/obsidian-dataview/#/)

---

## <span id="obp_templater">Templater</span>

### <span id="obp_templater_commands">命令</span>

Templater 中提供了几个命令来快速操作：

* **Template**: Open Insert Template modal `Alt + E`
* **Template**: Jump to next cursor `Alt + Tab`
* **Template**: Create new note from template `Alt + N`
* **Template**: Replace templates in the active file `Alt + R`

> [!info] 
> 
> 个人用得最多的是 `Alt+N` 这个，根据模板新建一个笔记。

### <span id="obp_templater_settings">设置</span>

#### 快捷键

默认光标跳转快捷键为 `Alt + Tab`，这与系统中和系统切换窗口的快捷键冲突了，所以只能将其修改成其他快捷键，个人设成 `Alt + ;`

#### 不同目录指定特定的模板

General Settings 中有个设置「Template folder location」，这个是设置模板文件默认或全局目录位置。

但有一种需求是，我想在某个目录下新建笔记时，不使用右键选「Create Note from Template」后再选择相应模板这两个操作，而直接使用 Obsidian 新建笔记动作，而笔记自动套用相应的模板文件生成新笔记，那就得使用到 Templater 中的我翻译为「新建文件触发器」的功能。

要使用这个「触发器」功能得做以下设置：

1. 打开「Trigger Templater on new file creation」选项
2. 当「Trigger Templater on new file creation」选项被打开后，就会出现「Add New」设置栏。它可以新增目录与模板对应关系
3. 在「Add New」设置栏中，点击「+」，就能新建一个对应项：前面的是要生成新笔记的目录，后面是要使用到的模板文件

> [!tip] 触发条件
> 
> 只能在右键「新建笔记」时，套用模板文件的触发器才能生效。如果是点击「文件列表」顶部那个「新建笔记」按钮，是不会触发的。
> 
> 原因，大概是「文件列表」**顶部**那个「新建笔记」按钮是「全局」的，新键笔记前「焦点」可能未落在指定目录，而使得触发条件不明，因为 Obsidian 的「新建笔记」有三个选项：「根目录」、「当前文件所在目录」和「指定附件目录」，所以使用 `Ctrl+N` 、`Ctrl+Shift+N` 或顶部「新建笔记」按钮新建笔记时，焦点并未落在指定的目录，所以 Templater 的新建笔记触发器未能触发成功。
> 
> > [!tip]
> > 
> > 这极有可能是因为 Obsidian 的设计哲学造成的，因为 Obsidian 最亮点功能是「双链」，这个功能的出现，使得原来使用目录方式来分类笔记的方式被「平面化」了，至少目录层级会减少。所以焦点这块，Obsidian 做得非常烂，即便当前文件焦点都已经失去，使用快捷键将操作转到「文件列表」上，文件列表焦点都不存在，如果这里使用 `Ctrl+N` 或按顶部「新建笔记」按钮，只会设置里设定的新建笔记的三个地方新建笔记，如设置为「当前文件所在目录」，那只会在编辑区正显示的文件所在的目录中新建笔记，即便这个「当前文件」的焦点已经失去了。
> 
> 右键「新键笔记」是针对指定目录的，所以触发目标更明确，满足了触发条件，所以这个方式是能生效。

### <span id="obp_templater_coding">编写</span>

要调用 [Javascript](../../JS/JS_Note.md) 函数时，使用 `<%* %>`。

如果以 `%>` 结束代码块，会在生成文件时，多加一个空行。如果不想多个空行，就使用 `-%>` 来结束代码块。

### <span id="obp_templater_functions">函数</span>

#### <span id="obp_templater_functions_internal">内置函数</span>

##### <span id="obp_templater_functions_internal_date">日期</span>

###### 获取当前日期

```javascript
tp.date.now(format: string = "YYYY-MM-DD", offset?: number⎮string, reference?: string, reference_format?: string)

```
> [!info] 函数原型解析
> 
> 第一个参数是日期时间格式化字符串，如果不指定，默认是就是 `YYYY-MM-DD`
> 
> 第二个参数是日期偏移量，如果 `-7` 就是上个星期，`7` 就是下个星期
> 
> 第三个是引用关联字符串
> 
> 第四个参数是引用辜联字符串格式化字符串

###### 获取明天日期

```javascript
tp.date.tomorrow(format: string = "YYYY-MM-DD")
```
> [!info] 函数解析
> 
> 这函数比 [获取当前日期](#获取当前日期) 函数还简单，只有一个格式化字符串的参数

###### 获取昨天日期

```javascript
tp.date.yesterday(format: string = "YYYY-MM-DD")
```
> [!info] 函数解析
> 
> 与 [获取明天日期](#获取明天日期) 函数类似。

---

##### <span id="obp_templater_functions_internal_file">文件</span>

###### 文件标题

```javascript
tp.file.title
```
> [!info]
> 
> 这是个属性，只获取当前文件的标题

###### 文件内容

```javascript
tp.file.content
```

> [!tip] 
> 
> `tp.file.content` 不是一个函数，而是一个属性，别乱加小括号。

> [!info] 源码
> 
> Templater [content 函数](https://github.com/SilentVoid13/Templater/blob/master/src/core/functions/internal_functions/file/InternalModuleFile.ts#L57)
> 
> Obsidian [read 函数](https://github.com/obsidianmd/obsidian-api/blob/master/obsidian.d.ts#L747)

```javascript
async generate_content():  Promise<string> {
	return await app.vault.read(this.config.target_file);
}
```

```javascript
read(normalizedPath: string): Promise<string>;
```

###### 查询模板

```javascript
tp.file.find_tfile(模板名称)
```

###### 新建文件

```javascript
tp.file.create_new(template: TFile ⎮ string, filename?: string, open_new: boolean = false, folder?: TFolder)
```

> [!info] 函数解析
> 
> `template`：模板。可以是 TFile 类型，也可以是字符串。
> 如果使用 `TFile` 模型，就得使用 [查询模板](#查询模板) 函数。
> 
> `open_new`：创建完新文件是否打开新文件，默认是不打开
> 
> `folder`：新文件存放的目录，`TFile` 类型，使用 [查询模板](#查询模板) 函数获取。

###### 获取目录

```javascript
tp.file.folder(relative: boolean = false)
```
> [!info] 函数解析
> 
> `relative` 这个参数是指定是否相对路径，默认是 `false`

###### 重命名

```javascript
tp.file.rename(new_title: string)
```
> [!tip] 重命名注意点
> 
> 这个函数不会改变后缀名

###### 移动文件

```javascript
tp.file.move(new_path: string, file_to_move?: TFile)
```

> [!info] move 函数解析
> 
> 第一个参数：目标路径，不需要带后缀名，如 `.md`
> 
> move 函数，如果目标路径中的文件名出现更改，就相当于拥有了 [重命名](#重命名) 的功能。
> 

> [!important] 
> 
> 还有一点提一下，就是这个函数是异步的得使用 `await` 关键字调用，而如果这个函数被自定义的函数「包装」使用时，自定义函数得使用 `async` 关键字声明，而调用这个自定义函数时，也得使用 `await` 关键字修饰。
> 
> 示例：
> 
> ```javascript
>	// 生成书籍信息笔记文件
>	// 参数 dirpth 将要存放笔记的目录路径
>	async function createNoteFile(dirpath){
>		// 判断是否以 / 结束
>		// 尾部没有目录分隔符/就补上
>		dirpath = dirpath.endsWith("/")?dirpath:dirpath+"/";
>		// 加上根路径，无论当前焦点位于什么目录，都从根定位选择或输入的目录路径
>		dirpath = dirpath.startsWith("/")?dirpath:"/"+dirpath;
>
>		// 文件完整路径
>		let fileFullPath = dirpath+file_title;
>		// 移动文件到指定目录
>		await tp.file.move(fileFullPath);
>	
>	}
> 
> 	// 生成笔记
>	await createNoteFile(inputDir);
> 
> ```
> 

###### 检测文件是否存在

```javascript
tp.file.exists(filename: string)
```
> [!info] exists 函数解析
> 
> 参数：文件名的相对路径。
> 
>> [!tip] 路径注意点
>> 
>> 这个路径中，文件名必须包括后缀名，如「**MyFolder/MyFile.md**」，`.md` 不能省。 
>> 
>> 另外，如果是链式操作最好不要把 `await` 关键字省略。
>> 
>>> [!example] 示例
>>> 
>>> ```javascript
>>> await tp.file.move( await tp.file.exists(fileFullPath+".md")? (inputFileName == today?fileFullPath+"_"+tp.date.now("HHmmss"): fileFullPath+"_"+tp.date.now("YYYYMMDDHHmmss")) : fileFullPath)
>>> ```

###### 光标跳转

```javascript
tp.file.cursor(order?: number)
```

插入模板后，将光标设置到此位置。

可以在 [Obsidian](Obsidian_Note.md) 配置 [快捷键](Obsidian_Plugins_Note.md#obp_templater_settings) 在不同的光标之间导航。

* `order`：不同光标的跳转顺序，例如从 1 到 2 到 3，依此类推。

---

##### <span id="obp_templater_functions_internal_system">系统</span>

###### 输入面板

```javascript
tp.system.prompt(prompt_text?: string, default_value?: string, throw_on_cancel: boolean = false, multiline?: boolean = false)
```

> [!info] 函数解析
> 
> 第一个参数：提法面板中标题文本
> 
> 第二个参数：默认值
> 
> 第三个参数：确定如果取消输入返回值。默认是 `false`，
> 
> 可以看下 [Templater 相关源码](https://github.com/SilentVoid13/Templater/blob/master/src/core/functions/internal_functions/system/InternalModuleSystem.ts)
> ```javascipt
>
>  try {
>                return await promise;
>            } catch (error) {
>                if (throw_on_cancel) {
>                   throw error;
>                }
>              return null;
>          }
> ```
> 可以看出，如果是 `false`，返回值为 `null`；如果是 `true`，则返回 `error`。
> 
> 第四参数：是否使用多行输入框，默认是 `false`，即单行输入框。

###### 输入提示选择面板

```javascript
tp.system.suggester(text_items: string[] ⎮ ((item: T) => string), items: T[], throw_on_cancel: boolean = false, placeholder: string = "", limit?: number = undefined)
```

> [!info] 函数解析
> 
> 第一个参数：提示项
> 
> 第二个参数：提示源，是个数组
> 
> 第三个参数：跟 [输入面板](#输入面板) 类似
> 
> 第四个参数：占位符字符串
> 
> 第五个参数：提示选项数量限制

> [!example] suggester 函数示例
> 
> ```javascript
> // vault库的根目录
> const vaultRoot = app.vault.getRoot()
> 
> // 所有目录
> // 即vault库根目录的所有子目录
> const folders = vaultRoot.children.filter(child => child.children)
> 
> // 列出所有目录
> // 获取目录对象
> let selectedFolder = await tp.system.suggester( (e) => e.name,folders,false," 选择目录 ")
> 
> ```

使用 DeepSeek 生成的 `suggester` 函数说明：

###### 函数定义

`tp.system.suggester(options, values, placeholder, throwOnCancel)`

参数说明：

1. **`options`** (数组，必需)  
    这是一个字符串数组，表示**显示给用户**的选择项（即用户看到的选项列表）。  
    例如：`["选项1", "选项2", "选项3"]`。
    
2. **`values`** (数组，必需)  
    这是一个与 `options` 对应的数组，表示每个选项的**实际返回值**。  
    例如：如果 `options` 是 `["A", "B", "C"]`，`values` 可以是 `["value1", "value2", "value3"]`。  
    用户选择 "A" 时，函数会返回 `"value1"`。
    
    * 如果 `values` 未提供，则默认使用 `options` 作为返回值。
        
    * 如果 `values` 是函数，则会对每个选项调用该函数，将其返回值作为实际值。
        
3. **`placeholder`** (字符串，可选)  
    这是选择器的占位符文本，显示在选择器的输入框中。  
    例如：`"请选择一个选项"`。
    
4. **`throwOnCancel`** (布尔值，可选)  
    如果设置为 `true`，当用户取消选择（例如点击外部或按 Esc 键）时，函数会抛出一个错误。  
    如果设置为 `false`（默认），取消选择时会返回 `null`。

###### 返回值

* 如果用户选择了一个选项，函数会返回对应的 `values` 中的值。
* 如果用户取消选择，函数返回 `null`（除非 `throwOnCancel` 设置为 `true`）。

###### 使用示例

示例 1：基本用法

```javascript
<%*
const options = ["选项A", "选项B", "选项C"];
const values = ["value1", "value2", "value3"];

const selectedValue = await tp.system.suggester(options, values, "请选择一个选项");
tR += `你选择的是: ${selectedValue}`;
%>

```

* 用户会看到一个下拉菜单，显示 `["选项A", "选项B", "选项C"]`。
* 如果用户选择 " 选项 A"，函数会返回 `"value1"`。

示例 2：简化用法

如果 `options` 和 `values` 相同，可以省略 `values` 参数：

```javascript
<%*
const options = ["文件夹1", "文件夹2", "文件夹3"];
const selectedFolder = await tp.system.suggester(options, null, "请选择一个文件夹");
tR += `你选择的文件夹是: ${selectedFolder}`;
%>
```

* 用户选择 「文件夹 1」 时，函数会直接返回 `"文件夹1"`。

---

示例 3：动态生成选项

```javascript
<%*
// 获取所有文件夹的名称
const folders = app.vault.getFolders().map(folder => folder.path);

// 让用户选择一个文件夹
const selectedFolder = await tp.system.suggester(folders, null, "请选择一个文件夹");
tR += `你选择的文件夹是: ${selectedFolder}`;
%>
```

动态列出 Obsidian 库中的所有文件夹，并让用户选择一个。

示例 4：处理取消选择

```javascript
<%*
const options = ["保存", "取消"];
const selectedAction = await tp.system.suggester(options, null, "请选择一个操作", false);

if (selectedAction === null) {
    tR += "操作已取消。";
} else {
    tR += `你选择了: ${selectedAction}`;
}
%>

```

* 如果用户取消选择，会输出 " 操作已取消 "。

注意事项

1. **异步函数**：`tp.system.suggester` 是一个异步函数，必须使用 `await` 调用。
2. **返回值**：返回值是 `values` 数组中对应的值，而不是 `options` 中的显示文本。
3. **用户交互**：该函数会阻塞脚本执行，直到用户做出选择或取消。

---

##### 其他

###### 取消生成笔记

在 Obsidian 的 Templater 插件中，如果你想在某些条件下放弃生成笔记（即停止模板的执行），可以通过以下方式实现：

* 方法 1：使用 `tp.frontmatter.cancel` 函数

`tp.frontmatter.cancel` 是 Templater 提供的一个函数，用于取消当前模板的执行。调用该函数后，模板会立即停止运行，并且不会生成新的笔记。

示例代码

```javascript
<%*
// 检查某个条件
const shouldCancel = true; // 这里可以根据实际条件设置

if (shouldCancel) {
    // 取消生成笔记
    tp.frontmatter.cancel("操作已取消，未生成笔记。");
} else {
    // 继续生成笔记
    tR += "正在生成笔记...";
}
%>
```

运行效果
* 如果 `shouldCancel` 为 `true`，模板会停止执行，并显示消息 `"操作已取消，未生成笔记。"`。
* 如果 `shouldCancel` 为 `false`，模板会继续执行，并输出 `"正在生成笔记..."`。

* 方法 2：使用 `return` 提前退出

如果你不想使用 `tp.frontmatter.cancel`，也可以通过 `return` 语句提前退出模板的执行。

示例代码

```javascript
<%*
// 检查某个条件
const shouldCancel = true; // 这里可以根据实际条件设置

if (shouldCancel) {
    // 提前退出
    return "操作已取消，未生成笔记。";
}

// 继续生成笔记
tR += "正在生成笔记...";
%>
```

运行效果

* 如果 `shouldCancel` 为 `true`，模板会提前退出，并返回消息 `"操作已取消，未生成笔记。"`。
* 如果 `shouldCancel` 为 `false`，模板会继续执行，并输出 `"正在生成笔记..."`。

* 方法 3：结合用户交互

你可以结合 `tp.system.prompt` 或 `tp.system.confirm` 实现动态判断是否放弃生成笔记。

示例代码

```javascript
<%*
// 询问用户是否继续
const shouldContinue = await tp.system.confirm("是否生成笔记？");

if (!shouldContinue) {
    // 取消生成笔记
    tp.frontmatter.cancel("用户取消了操作，未生成笔记。");
} else {
    // 继续生成笔记
    tR += "正在生成笔记...";
}
%>
```

运行效果

* 如果用户点击“否”，模板会停止执行，并显示消息 `"用户取消了操作，未生成笔记。"`。
* 如果用户点击“是”，模板会继续执行，并输出 `"正在生成笔记..."`。

* 方法 4：结合目录检测

如果你想在检测到某个目录不存在时放弃生成笔记，可以结合 `app.vault.getFolderByPath` 实现。

示例代码

```javascript
<%*
// 定义要检测的目录路径
const folderPath = "Notes/Projects"; // 替换为你的目录路径

// 检测目录是否存在
const folder = app.vault.getFolderByPath(folderPath);

if (!folder) {
    // 目录不存在，取消生成笔记
    tp.frontmatter.cancel(`目录 "${folderPath}" 不存在，未生成笔记。`);
} else {
    // 继续生成笔记
    tR += `目录 "${folderPath}" 存在，正在生成笔记...`;
}
%>
```

运行效果

* 如果目录不存在，模板会停止执行，并显示消息 `"目录 "Notes/Projects" 不存在，未生成笔记。"`。
* 如果目录存在，模板会继续执行，并输出 `"目录 "Notes/Projects" 存在，正在生成笔记..."`。

总结

在 Templater 插件中，放弃生成笔记可以通过以下方式实现：
1. 使用 `tp.frontmatter.cancel` 函数直接取消模板执行。
2. 使用 `return` 语句提前退出模板。
3. 结合用户交互（如 `tp.system.confirm`）动态判断是否放弃生成笔记。
4. 结合目录检测或其他条件判断是否放弃生成笔记。

根据你的需求选择合适的方法即可！

### <span id="obp_templater_docs">相关文档链接</span>

* [Obsidian API](https://github.com/obsidianmd/obsidian-api)
* [Templater Snippets - shabeblog](https://shbgm.ca/blog/obsidian/Templater+Snippets)

---

## <span id="obp_aboutnotes">相关笔记</span>

* [Obsidian 笔记](Obsidian_Note.md)
* [Obsidian常用插件清单](Obsidian_Plugins_List.md)
* [Obsidian 相关视频](Obsidian_Videos.md)
* [Obsidian 资料](Obsidian_Material.md)
* [Markdown笔记](../../Markdown/Markdown_Note.md)