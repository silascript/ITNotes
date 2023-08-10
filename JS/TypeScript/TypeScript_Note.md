---
aliases:
  - 
tags:
  - typerscript
  - js
  - javascript
created: 2023-08-02 11:41:26 
modified: 2023-08-4 21:54:20

---

# TypeScript 笔记

---

## 目录

---

## 安装

### 安装 NodeJS

[安装nodejs](NodeJS_Note.md#node_install)

### 安装 tsc

```shell
npm i tsc -g
```

#### 常用 tsc 命令

* `tsc`：编译当前目录下所有 ts 文件
* `tsc xxx.ts`：编译当头目录下指定 ts 文件
* `tsc -w xxx.ts `：监听指定 ts 文件，只要文件保存就对其进行编译

### 安装 ts-node

`ts-node` 这个工具是用于编译运行 ts 文件，节省每次都敲下 `tsc` 编译 ts 及 `node` 运行编译后的 js。

```shell
npm i ts-node -g
```

### 初始化项目

在项目根目录下，执行以下命令，可以初始化该项目：

```shell
tsc --init
```

> [!info] 关于 init
> 
> 会在初始项目，并在项目中生成一个 `tsconfig.json` 的配置文件。

---

## TS 项目

### 编译上下文

使用 `tsc --init` 命令初始化项目后，会在项目根目录中生成一个 `tsconfig.json` 文件，这个就是 TypeScript 的编译上下文配置文件。

#### tsconfig 部分属性解析

`target`：指定 ECMAScript 目标版本。`ES3`、`ES5`、`ES2015`、`ES2016` 等。

---

## 类型

### 基本类型

#### number

ts 中的 number 类型包括：整型、浮点、二进制、八进制、十六进制。

```typescript
let num1 :number= 1 //整型
let num2 :number= 0.1 // 浮点型
let num3 :number= 0b001 // 二进制以 「0b」或「0B」开头
let num4 :number= 0o12 // 八进制以「0o」或「0O」开头
let num5 :number= 0x22 // 十六进制以「0X」或「0x」开头
```

如果变量值为 `NaN`，则表示此变量不是一个数字。
> [!tip] NaN
> 
> Not a Number

#### 字符串

```typescript
let s1 :string = "hello"
```

#### 空类型

空类型有三种：
* null
* undefined
* void

> [!info] null 与 undefined 区别
> 
> `null` 指曾赋过值，但目前没有值。`null` 是关键字，不是标识符，所以不能将其作为变量来使用和赋值。
> 
> `undefined` 指从未赋值。`undefined` 是标识符，可以当作变量来使用和赋值。
> 
> **严格模式**下，`null` 和 `undefined` 两种类型的变量不能互相赋值。
> [!tip]
> 
> `tsconfig.json` 文件中启用了 `strictNullChecks`

#### 特殊类型

##### any

使用 any 类型时，实质是关闭 ts 的类型检查。

##### unknown

unknown 类型跟 any 类型非常相似，但更安全。

##### never

---

### 联合类型

```typescript
let n1: string | number = "hello"
```

---

### 数组类型

可以限定数组中存放数据的类型，可以是多种：
```typescript
let a1: (string | number)[] = ["shell", 123]
```
> [!info] 数组类型
> 
> 原生 JS 数组本来就是可以存在任何类型的数据，而且可以混着放，但对于真正开发而言，这种「高自由度」是不利于开发规范性的，所以 TS 就在编译期对这个「自由度」作了适当的限制。

---

