---
aliases: []
tags:
  - format
  - formatter
  - prettier
created: 2024-05-24 09:59:11
modified: 2025-09-23 05:04:57
---

# Prettier 笔记

---

## 简介

[Prettier](https://prettier.io) 是一个格式化工具。

## 基本

### 全局配置

用户目录下， `.prettierrc` ，是 prettier 的默认全局配置文件。
> [!tip] 
> 
> 支持 `.yaml`、`.yml`、`.json` 及 `。js` 后缀。
> 
> 也就是说你可以将全局配置文件命名为 `.prettierrc.json` 或 `.prettierrc.yaml` 等。
> 

如果配置文件在项目下，那就是项目级别的配置。

#### 常用配置项

```json
printWidth: 80, // 单行长度
tabWidth: 2, // 缩进长度
useTabs: false, // 使用空格代替tab缩进
semi: true, //句末使用分号
singleQuote: false, // 使用单引号
quoteProps: 'as-needed', // 仅在必需时为对象的key添加引号
jsxSingleQuote: true, // jsx中使用单引号
trailingComma: 'all', // 多行时尽可能打印尾随逗号
bracketSpacing: true, // 在对象前后添加空格-eg: { foo: bar }
jsxBracketSameLine: true, // 多属性html标签的‘>’折行放置
arrowParens: 'always', // 单参数箭头函数参数周围使用圆括号-eg: (x) => x
requirePragma: false, // 无需顶部注释即可格式化
insertPragma: false, // 在已被preitter格式化的文件顶部加上标注
htmlWhitespaceSensitivity: 'ignore', // 对HTML全局空白不敏感
vueIndentScriptAndStyle: false, // 不对vue中的script及style标签缩进
endOfLine: 'lf', // 结束行形式
embeddedLanguageFormatting: 'auto', // 对引用代码进行格式化
```

### 插件

对于 Prittier 不支持的语言，可以通过 Plugin 来支持其格式化：[Prittier Plugins](https://prettier.io/docs/plugins#community-plugins)

---

## 常用文件配置

### markdown

#### prettier-plugin

[prettier-plugin](https://github.com/lint-md/prettier-plugin) 这个插件是让 [Markdown_Note](../Markdown/Markdown_Note.md) 排版更符合中文习惯。

##### 安装

它是基于 [ lint-md](https://github.com/lint-md/lint-md)，所以先安装 lint-md：`npm install -g @lint-md/cli`

再安装 prettier 及插件：`npm i prettier-plugin-lint-md prettier -D`

##### 配置

在 `prettier.mjs` 配置文件中添加 `plugins` 内容：

```js
const config = {
	//....其他配置
	// 添加prettier 基于lint-md的插件 用于markdown格式化 
	plugins: [`prettier-plugin-lint-md`],
};
```

---

## 文档

* [Prettier.io docs options](https://prettier.io/docs/en/options.html)

---

## 相关笔记

* [格式化器 笔记](Formatter_Note.md)
* [格式化器 资料清单](Formatter_Material.md)

