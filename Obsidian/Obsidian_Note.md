
# Obsidian 笔记

---

## 目录
* [Obsidian 配置目录](#obn_config_dir)
* [Snippet](#obn_snippet)
*  [插件](#obn_plugin)
	* [第三方插件](#obn_plugin_commp)


---

##  <span id="obn_config_dir">Obsidian 的配置目录</span>

在 Obsidian的 **vault**，就是所谓的「库」，其根目录下，Obsidian 会生成一个配置目录 **.obsidian**。

### .obsidian 目录结构

* Obsidian 是使用 json 文件来配置的。
* app.json：主配置文件
* appearance.json：外观配置文件
* core-plugins.json：核心插件配置文件
* community-plugins.json：第三方插件配置文件
* graph.json：图表相关的配置文件（像什么链接、关系图谱等）
* hotkeys.json：快捷键配置文件

![obsidian_configdir_1](./Obsidian_Note.assets/obsidian_configdir_1.png)

* plugins： 目录是存放插件的目录。每个插件都以独立的目录存放。
* themes：主题样式。存的是一些 css 文件。

![obsidian_configdir_2](./Obsidian_Note.assets/obsidian_configdir_2.png)

---

## <span id="obn_snippet">Snippet</span>


1. 将相应的 css 文件放到当前库下.obsidian 目录中的 **snippets** 目录下。
2. 在 obsidian 的 「外观」（Appearance）设置选项中，「CSS 代码片段」（CSS Snippets）选项，点下刷新，就能加载出可用的 Css
 Snippets ，根据需要启用相应的 CSS Snippet，该 Snippet 的功能就能生效了。


### 常用的 Snippet：

#### Clutter-Free Headings

[clutter-free-headings](https://github.com/deathau/obsidian-snippets#clutter-free-headings)可以让标题那些「#」符号以「H1」~「H6」方式显示。


#### Bigger Link Popup Preview

[Bigger Link Popup Preview](https://github.com/kmaasrud/awesome-obsidian/blob/master/code/css-snippets/bigger-link-popup-preview.css) 这个 Snippet 可以加大预览窗口大小。


---
## <span id="obn_plugin">插件</span>

Obsidian 的插件分为 「核心插件」和「第三方插件」。


### <span id="obn_plugin_commp">第三方插件</span>

要安装和使用第三方插件前，先得把「设置」中的「安全模式」开关关闭，才能浏览和安装第三方插件。


#### 常用插件

##### Quick Explorer
这个插件，是在界面标题栏中显示，当前路径，并且可以快速浏览文件。

![obsidian_plugin_quickexplorer](./Obsidian_Note.assets/obsidian_plugin_quickexplorer.png)




##### Show Current File Path
此插件是在底部状态栏上显示当前文件名，点击能够复制文件的路径名。
> 默认情况，点击复制的是相对路径，只有在这个插件的设置中，打开了「Copy absolute path」选项才会复制绝对路径。

![obsidian_plugin_show_current_file_path](./Obsidian_Note.assets/obsidian_plugin_show_current_file_path.png)


---


##### Obsidian tabs
这插件能让多个面板变成单面板多标签的形态。
[Obsidian_Note](Obsidian_Note.md)
使用 **Obsidian tabs** 前：
![obsidian_plugin_tabs_before](./Obsidian_Note.assets/obsidian_plugin_tabs_before.png)

使用 **Obsidian tabs** 后：
![obsidian_plugin_tabs_after](./Obsidian_Note.assets/obsidian_plugin_tabs_after.png)


---

##### <span id="ob_plugins_cp_btn_codeblocks">Copy Button for code blocks</span>

[Copy Button for code blocks](https://github.com/jdbrice/obsidian-code-block-copy) 是一个在代码区添加一个复制按钮的插件。这插件异常的实用，非常推荐安装。

![copy button for code blocks screenshot](https://github.com/jdbrice/obsidian-code-block-copy/raw/main/screenshot.png)


---

##### <span id="ob_plugins_cmenu">cMenu</span>


[cMenu](https://github.com/chetachiezikeuzor/cMenu-Plugin) 这个插件是在编辑区添加一些快捷功能按钮。

![cMenu](https://raw.githubusercontent.com/chetachiezikeuzor/cMenu-Plugin/master/assets/cMenu.gif)


这插件能使编辑文档时，提高其编辑的流畅度。

这插件除了预设的功能按钮，还能根据自己的需求，自定义配置自己的按钮。




---

##### <span id="ob_plugins_obsidian-commander">obsidian-commander</span>

[obsidian-commander](https://github.com/phibr0/obsidian-commander) 是一个自定义命令插件。

![ob_commander_1](https://user-images.githubusercontent.com/46250921/177593938-2c3aae81-1bf6-45df-b06a-e51a8b4e4a0e.svg)


这插件的自由度非常高，可以在很多位置自定义命令。


可以在标题栏、状态栏、右键菜单、页眉及侧边栏添加自定义命令。




---

##### <span id="ob_plugins_relativelinenumbers">Relative line Numbers</span>

[obsidian-relative-line-numbers](https://github.com/nadavspi/obsidian-relative-line-numbers) 是一个显示相对行号的插件

![relative line numbers demo](https://github.com/nadavspi/obsidian-relative-line-numbers/raw/main/demo.gif)


---

##### <span id="ob_plugins_vimrcsupport">Vimrc Support</span>


[vimrc support](https://github.com/esm7/obsidian-vimrc-support) 是一个增加了内置的 vim 功能的插件。


不过这插件有点鸡肋，完全比不上在 vscode 上使用 vim的插件的体验，而且配置麻烦。




---


##### Theme Picker

快速切换已安装的主题。


---

##### Admonition

添加提示块。

语法：ad-类型

```ad-note
note 类型
```

---

##### Folder Note Plugin

这是一个目录插件。可以在点击SideBar 中的目录时，在面板上展现目录下的所有内容。




---

##  相关资料
[awesome-obsidian](https://github.com/kmaasrud/awesome-obsidian)
[obsidian-snippets](https://github.com/deathau/obsidian-snippets)


