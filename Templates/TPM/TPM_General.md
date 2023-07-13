<%*



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



# 笔记

---



<%*

// 默认目录名
let defaultDirName = "General_Note"
// 默认文件名
let defaultFileName = "G_Note"

// 指定目标目录名
// 默认值: General_Note
// 如果用户未指定目标目录，则使用默认目录名
let inputDir = await tp.system.prompt("请输入目录路径",defaultDirName)

// console.log(inputDir)

// 指定文件名
// 如果用户没给文件名，则使用默认文件名
let inputFileName = tp.file.title.includes("未命名") ? (tp.file.title.toLowerCase().includes("untitled") ? (await tp.system.prompt("请输入要创建的文件名",defaultFileName)) : tp.file.title) : (tp.file.title.toLowerCase().includes("untitled") ? (await tp.system.prompt("请输入要创建的文件名",defaultFileName)) : tp.file.title)

// 文件完整路径(相对路径，相对于库)
let fileFullPath = inputDir+"/"+inputFileName

// 检测是否目标目录下是否已有同名文件
// 如果同名，便在文件名后加的"时间戳"
inputFileName = await tp.file.exists("/"+fileFullPath+".md") ? (inputFileName+"_"+tp.date.now("YYYYMMDDHHmmss")) : inputFileName

// 重新组装文件路径
fileFullPath = inputDir+"/"+inputFileName

// 并移动文件到 Daily 目录 
await tp.file.move(fileFullPath)

%>