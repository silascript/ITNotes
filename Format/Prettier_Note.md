---
aliases: []
tags:
  - formatter
  - prettier
created: 2024-05-24 09:59:11
modified: 2024-08-02 03:23:59
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

---

## 文档

* [Prettier.io docs options](https://prettier.io/docs/en/options.html)

---

## 相关资料

* [代码美化神器——Prettier使用详解 - 掘金](https://juejin.cn/post/6970267363845341220)
* [史上最全Preitter指南(含option中文解释) - 知乎](https://zhuanlan.zhihu.com/p/356654434)
* [VSCode的全局配置与项目单独配置 - 掘金](https://juejin.cn/post/7290740395913691177)
* [详解prettier使用以及与主流IDE的配合 - w4ngzhen - 博客园](https://www.cnblogs.com/w4ngzhen/p/17559689.html)
* [prettier configuration doc](https://prettier.nodejs.cn/docs/en/configuration.html)

