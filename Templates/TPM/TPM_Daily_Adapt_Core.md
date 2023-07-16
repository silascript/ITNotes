<%* 

// 模板说明
// 这个模板专门针对适配 Obsidian 核心插件「日记」的。
// 因为核心「日记」插件开启后，会出现一个按钮「打开创建今天的日记」。这个按钮所具备的功能是「打开」和「创建」日记--意味着使用这个功能时，是默认创建或打开当天的日记。即便你想创建一个同名日记文件也是不行的，「今天的日记」文件对于「日记」插件来说，有且只有一个，不存在同名更名「创建」的需求，已经存在的当天日记，这按钮的功能就只有将其「打开」。所以像 [TPM_Daily](TPM_Daily.md) 这个模板中，使用的`move()` 函数，给同名日记文件「重命名」的功能需求及写法，就不适用于核心插件「日记」。

// 今天日期
let today = tp.date.now("YYYY-MM-DD")

// 文件创建时间
let createTime = tp.file.creation_date("YYYY-MM-DD HH:mm:ss")
-%>
---
aliases:
  - 
tags: daily
created: <% createTime %> 
modified: <% createTime %>

---

# <% today %> 日记

---

[[<% tp.date.now("YYYY-MM-DD",-1) %> | 昨天]] | [[<% tp.date.now("YYYY-MM-DD",1) %> | 明天]]


今天：<% today %>


