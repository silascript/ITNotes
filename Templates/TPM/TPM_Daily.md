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

// 默认文件名
let defaultDailyFileName = today

// 检测是否已命名
let inputFileName = ( tp.file.title.includes("未命名") || tp.file.title.toLowerCase().includes("untitled") ) ? (await tp.system.prompt("请输入要创建的文件名",defaultDailyFileName)) : tp.file.title 

// 重新组装文件路径
let fileFullPath = "Daily/"+inputFileName

// 并移动文件到 Daily 目录
// 如果遇到同名将加时间戳以示区分
// 重名规则：
// 1. 如果文件名是使用默认日期，即"YYYY-MM-DD"形式，则在后面添加时分秒的时间戳
// 2. 如果文件名非默认的日期名，则在后面加完整时间戳，即"YYYYMMDDHHmmss"形式
await tp.file.move( await tp.file.exists(fileFullPath+".md")? (inputFileName == today?fileFullPath+"_"+tp.date.now("HHmmss"): fileFullPath+"_"+tp.date.now("YYYYMMDDHHmmss")) : fileFullPath)

%>

