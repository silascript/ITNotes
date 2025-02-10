<%* 

let file_title= tp.file.title;
// 默认文件名
let defaultFileName = "Defualt_Book_Info";

// 检测是否已命名 
file_title = ( file_title.includes("未命名") || file_title.toLowerCase().includes("untitled") ) ? (file_title=await tp.system.prompt("请输入要创建的文件名")) : file_title;
// 如果没输入任何文件名则赋个默认文件名
file_title == ""?(file_title=defaultFileName) : (await tp.file.rename(file_title));

///////////////////////////////////////

// 默认书籍目录路径
let defaultDir="/Books/";

// vault库的根目录
const vaultRoot = app.vault.getRoot()

// 所有目录
// 即vault库根目录的所有子目录
// const folders = vaultRoot.children.filter(child => child.children)
// const folders = app.vault.getAllFolders(true)
const folders = app.vault.getAllFolders(true).map(f=>f.path)
// const folders = app.vault.getFolders()

// 列出所有目录
// 获取目录对象
// let selectedFolder = await tp.system.suggester( (e) => e.name,folders,false,"选择目录")
// let selectedFolder = await tp.system.suggester( (e) => e.path,folders,false,"选择存放的目录")
let selectedFolder = await tp.system.suggester( (e) => e,folders,false,"选择存放的目录")

console.log("目录路径："+selectedFolder)


let tempDir = defaultDir
if(selectedFolder != ""){
	tempDir="/"+selectedFolder
}

// 获取文件存在的目录
let inputDir = await tp.system.prompt("请输入目录地址",tempDir);
// 如果没有输入任何目录则赋个默认目录路径
if(inputDir == ""){
	inputDir = defaultDir+"/Books_Temp";
}

// 判断是否以 / 结束
inputDir = inputDir.endsWith("/")?inputDir:inputDir+"/";

// 重新组装文件路径 
// let fileFullPath = tp.file.exists((inputDir+inputFileName)+".md")?  : (inputDir+inputFileName)
// 移动文件到指定目录
await tp.file.move(inputDir+file_title)

-%>
---
booksdb: <% tp.file.cursor(1) %>
cover: 
category: 

---

# <% file_title %>

---

[书名:: <% tp.file.cursor(2) %>] 

[原版书名:: <% tp.file.cursor(3) %>]

[版本:: <% tp.file.cursor(4) %>]

[作者:: <% tp.file.cursor(5) %>]

[译者:: <% tp.file.cursor(6) %>]
  
[出版日期:: <% tp.file.cursor(7) %>]

[ISBN:: <% tp.file.cursor(8) %>]

 ![|520x600](<% tp.file.cursor(9) %>)

