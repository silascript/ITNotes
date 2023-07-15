<%*

// 文件创建时间
let createTime = tp.file.creation_date("YYYY-MM-DD HH:mm:ss")

-%>
---
aliases:
  - 
tags: generalnote
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


// vault库根
const vaultRoot = app.vault.getRoot()

// 所有目录
const folders = vaultRoot.children.filter(child => child.children)

// 指定目标目录名
// 默认值: General_Note
// 如果用户未指定目标目录，则使用默认目录名
// let inputDir = await tp.system.prompt("请输入目录路径",defaultDirName)
// let inputDir = tp.system.prompt("请输入目录路径",defaultDirName)

// 检测当前文件目录
// console.log("tp.file.folder()： "+tp.file.folder())
// console.log("tp.file.folder(false)： "+tp.file.folder(false))
// console.log("tp.file.folder(true)： "+tp.file.folder(true))

// 列出所有目录
// 获取目录对象
let selectedFolder = await tp.system.suggester( (e) => e.name,folders,false,"选择目录")

let dirName = ""

if(null== selectedFolder){
	dirName = await tp.system.prompt("请输入目录路径", defaultDirName )
}else{
	dirName = selectedFolder.name
}

// let selectedFolder = await tp.system.suggester( (e) => e.name,folders,false,"选择目录")

// console.log(selectedFolder)
// name为目录的名称
// console.log("selectedFolder.name： "+selectedFolder.name)

// 检测当前文件目录
// console.log("tp.file.folder()： "+tp.file.folder())
// console.log("tp.file.folder(false)： "+tp.file.folder(false))
// console.log("tp.file.folder(true)： "+tp.file.folder(true))

// console.log(tp.file.exists(selectedFolder.name+"/"+tp.file.title+".md"))


// 指定文件名
// 如果用户没给文件名，则使用默认文件名
let inputFileName = ( tp.file.title.includes("未命名") || tp.file.title.toLowerCase().includes("untitled") ) ? (await tp.system.prompt("请输入要创建的文件名",defaultFileName)) : tp.file.title 

// 重新组装文件路径
let fileFullPath = dirName+"/"+inputFileName

// console.log("-------------------")

// console.log(tp.file.exists(selectedFolder.name+"/"+inputFileName+".md"))
// console.log(tp.file.exists(selectedFolder.name+"/"+inputFileName))

// 检测文件是否已经存在目标目录
// 并移动文件到目标目录 
// 如果同名，便在文件名后加的"时间戳"
await tp.file.move(await tp.file.exists(fileFullPath+".md") ? (fileFullPath+"_"+tp.date.now("YYYYMMDDHHmmss")) : fileFullPath)

%>