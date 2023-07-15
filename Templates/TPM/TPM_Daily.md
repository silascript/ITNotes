<%* 
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

今天：<% today %>

<%*


// 检测是否已命名
let inputFileName = ( tp.file.title.includes("未命名") || tp.file.title.toLowerCase().includes("untitled") ) ? (await tp.system.prompt("请输入要创建的文件名")) : tp.file.title 

// 重新组装文件路径
let fileFullPath = "Daily/"+inputFileName

// 并移动文件到 Daily 目录
await tp.file.move( await tp.file.exists(fileFullPath+".md") ? (fileFullPath+"_"+tp.date.now("YYYYMMDDHHmmss")) : fileFullPath)

%>

