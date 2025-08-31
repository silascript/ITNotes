---
aliases: []
tags:
  - vim
  - color
  - theme
  - colorscheme
  - list
created: 2023-08-18 19:44:52
modified: 2024-04-10 20:27:22
---
# vim 配色笔记

----

## <span id="vim_colorscheme_recommend">一些 colorscheme</span>

### <span id="vim_colorscheme_recommend_tender">Tender</span>

[tender](https://github.com/jacoborus/tender.vim) 这是一个 24 bit 的真彩色 colorshceme。

![tender preview 1](https://cloud.githubusercontent.com/assets/829859/18417365/7780885a-782e-11e6-8e88-150cfc70e35b.png)

使用前，vim 或 neovim 得把 真彩色选项打开：
```vimscript
if (has("termguicolors"))
 set termguicolors
endif
```

这配色也能与 [lightline](https://github.com/itchyny/lightline.vim) 或 [airline](https://github.com/vim-airline/vim-airline) 结合：

![lightline or airline ](https://cloud.githubusercontent.com/assets/829859/18418261/0e0f54f4-7843-11e6-9825-bff197a7f76a.png)

lightline：
```vimscript
let g:lightline = { 'colorscheme': 'tender' }
```

airline：
```vimscript
let g:airline_theme = 'tender'
```

### <span id="vim_colorscheme_recommend_deus">deus</span>

[deus](https://github.com/ajmwagar/vim-deus) 这是从 [Gruvbox]() 衍化而来的一款配色，整体而言，没有 Gruvbox 那么偏黄。

![deus preview 1](https://github.com/ajmwagar/vim-deus/raw/master/screencaps/vim-deus.jpg)

deus 的简单设置：
```vimscript
colorscheme deus
let g:deus_termcolors=256
```

---

###  <span id="vim_colorscheme_recommend_vimone">vim-one</span>

[vim-one](https://github.com/rakr/vim-one) 这配色灵感是从 atom 那个里来的。

![](https://github.com/rakr/vim-one/raw/master/screenshots/new-logo.png)

简单配置：
```vimscript

" 这配色有深色版有浅色版 根据需求自己选配
"set background=light
set background=dark

let g:one_allow_italics = 1 " I love italic for comments
colorscheme one
```

---

### <span id="vim_colorscheme_recommend_onedark">onedark</span>

[onedark](joshdick/onedark.vim) 同样是款 「One」 系列的配色。

![onedark preview](https://raw.githubusercontent.com/joshdick/onedark.vim/main/img/readme_header.png)

---

### <span id="vim_colorscheme_recommend_sonokai">sonokai</span>

[sonokai](https://github.com/sainnhe/sonokai) 是基于 monokai pro 改的一款配色。

![sonokai preview](https://user-images.githubusercontent.com/37491630/87916859-a03dad80-caa6-11ea-9694-b34c4a980672.png)

![sonokai preview 2](https://user-images.githubusercontent.com/37491630/87898138-ee8b8600-ca7f-11ea-90ea-681955458c68.png)

这配色有多个选项可选配，连光标颜色都能配。

具体配置请参考：[sonokai doc](https://github.com/sainnhe/sonokai/blob/master/doc/sonokai.txt)

---

### <span id="vim_colorscheme_recommend_gruvbox">Gruvbox</span>

[gruvbox](https://github.com/morhetz/gruvbox) 著名的配色，各种编辑器都能见到这货。不是太黑的，颜色不算非常惊艳，至少没有第一次见到 monokai 那样，但配色恰到好处，这配色最大特点就是舒服。

![gruvbox preview 1](https://camo.githubusercontent.com/a05028ef4dae5865098c508fc9f686b211f510198f07e6a5636734dbac618b30/687474703a2f2f692e696d6775722e636f6d2f476b496c38466e2e706e67)

![gruvbox airline](https://camo.githubusercontent.com/a7287ec3f5259b4c03cb4bb2f70c7dfc777d76b7c72f4c161198191025a1d535/687474703a2f2f692e696d6775722e636f6d2f775251636555522e706e67)

具体使用参考：[gruvbox wiki](https://github.com/morhetz/gruvbox/wiki)

---

### <span id="vim_colorscheme_recommend_gruvboxmaterial">gruvbox-material</span>

[gruvbox-material](https://github.com/sainnhe/gruvbox-material) 是 gruvbox 的一个衍生品，杂交了 material 风格的「混血」配色。

gruvbox-material 详细用法参考：[gruvbox-material Doc](https://github.com/sainnhe/gruvbox-material/blob/master/doc/gruvbox-material.txt)

---

### <span id="vim_colorscheme_recommend_gruvboxbaby">gruvbox-baby</span>

[gruvbox-baby](https://github.com/luisiacc/gruvbox-baby) 又是一个 Gruvbox 的衍生品。

![gruvbob-baby preview](https://user-images.githubusercontent.com/31720261/147399558-bf00b60a-aea9-46f7-a823-fc760cda05be.png)

简单配置：

```vim
colorscheme gruvbox-baby

" 背景色有两个版本可选 默认为 medium
background_color = dark

" 还能设透明 默认 false
transparent_mode = true

" 注释的样式，默认为斜体
comment_style =italic

```

---

### <span id="vim_colorscheme_recommend_gruvbox8">vim-gruvbox8</span>

[vim-gruvbox8](https://github.com/lifepillar/vim-gruvbox8) 是一款简化和优化 Gruvbox 衍生配色。

![gruvbox8 preview](https://camo.githubusercontent.com/6df4058330e072dd1f6c87829da0b844bd719c90b072579a1551a55de9330fe3/68747470733a2f2f7261772e6769746875622e636f6d2f6c69666570696c6c61722f5265736f75726365732f6d61737465722f67727576626f78382f67727576626f78382d76617269616e74732e706e67)

---

### <span id="vim_colorscheme_recommend_material">material</span>

material 是 google 弄的一套设计规范。

vim  material 风格的 配色很多，大概列出一些 github 上 star 比较多的。

#### kaicataldo/material

[kaicataldo/material](https://github.com/kaicataldo/material.vim)

![kaicataldo material preview](https://raw.githubusercontent.com/kaicataldo/material.vim/main/screenshots/material-all-variants.png)

简单配置：

```vim
	colorscheme material
```

```vim
	" 允许斜体
	let g:material_terminal_italics = 1
```

```vim
	" 风格
	let g:material_theme_style = 'lighter'
```
material 这款配色可选的 style 有如下：

* default
* palenight
* ocean
* lighter
* darker
* default-community 
* palenight-community 
* ocean-community 
* lighter-community 
* darker-community

这款配色同样支持 [lightline](https://github.com/itchyny/lightline.vim) 和 [airline](https://github.com/vim-airline/vim-airline)：

```vimscript
let g:lightline = { 'colorscheme': 'material_vim' }
```

```vimscript
let g:airline_theme = 'material'
```

---

### <span id="vim_colorscheme_recommend_nord">nord</span>

[nord](https://github.com/arcticicestudio/nord-vim) 挺素的一款配色。

![nord preview](https://raw.githubusercontent.com/arcticicestudio/nord-docs/develop/assets/images/ports/vim/overview-go.png)

另一个库：[nord.nvim](https://github.com/shaunsingh/nord.nvim) 这个在 neovim 下才显示正常。

![nord.nvim preview](https://user-images.githubusercontent.com/71196912/128029391-ad55fd41-d5f9-43bd-a795-c11b562f9d6d.jpg)

---

### <span id="vim_colorscheme_recommend_everforest">everforest</span>

[everforest](https://github.com/sainnhe/everforest) 同样也是非常经典的配色。

![everforest screenshot 1](https://user-images.githubusercontent.com/37491630/206063921-58418bb0-7752-43f3-9f3b-f3752f8ee753.png)

可以进行一些设置：

```vim
# everforest
var everforest_result = commands_basic.ExistPlug('sainnhe/everforest')
if everforest_result == 1
  try
    # 检测当前 colorscheme  
    if g:colors_name ==? 'everforest'
      # Available values: 'hard', 'medium'(default), 'soft'
      # g:everforest_background = 'soft'
      g:everforest_background = 'medium'
      # For better performance
      # g:everforest_better_performance = 1
    endif
  catch
  endtry
endif
```

---

### <span id="vim_colorscheme_recommend_tokyonightnoir">tokyonightnoir</span>

[tokyonightnoir-vim](https://github.com/timmajani/tokyonightnoir-vim) 

![tokyonightnoir screenshot](https://raw.githubusercontent.com/ghifarit53/tokyonight-vim/master/pictures/screenshot.png)

---

### <span id="vim_colorscheme_recommend_papercolor">papercolor</span>

[papercolor-theme](https://github.com/NLKNguyen/papercolor-theme)

![papercolor screenshot](https://camo.githubusercontent.com/73cceafdb27e465cb991a047ffc34440bc8c4aa746a3c4239f5cd96d369135bc/68747470733a2f2f6e6c6b6e677579656e2e66696c65732e776f726470726573732e636f6d2f323031352f30352f632d6461726b2d73706c69742e706e67)

---

### <span id="vim_colorscheme_recommend_base16">base16</span>

[base16-vim](https://github.com/tinted-theming/base16-vim) 这是一个配色「小集合」，有**200**种配色样式可选。

![base16 screenshot 1](https://github.com/tinted-theming/base16-vim/raw/main/screenshots/base16-vim-screenshot-horizon-dark.png)

![base16 screenshot 1](https://github.com/tinted-theming/base16-vim/raw/main/screenshots/base16-vim-screenshot-onedark.png)

预览：[base16-gallery](https://tinted-theming.github.io/base16-gallery/)

---

### <span id="vim_colorscheme_recommend_awesome-vim-colorschemes">awesome-vim-colorschemes</span>

[awesome-vim-colorschemes](https://github.com/rafi/awesome-vim-colorschemes) 这个是配色集合，懒得找配色，直接用这个就好了！

---

## 相关链接

* [vimcolorschemes](https://vimcolorschemes.com/)
* [Vim colors | Generate your custom colorscheme](https://vimcolors.org/)
* [best vim colorscheme](https://vim.page/best-vim-themes-color-schemes)

---

## 相关笔记

* [Vim笔记](Vim_Note.md)
* [vim插件笔记](Vim_Plugin.md)
* [vim常用操作](vim常用操作.md)
* [Vim视频清单](Vim_Videos.md)
* [Vimscript笔记](Vimscript_Note.md)
* [Vimscript9笔记](Vimscript9_Note.md)