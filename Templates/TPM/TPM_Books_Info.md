<%* 

let file_title= tp.file.title;
// 默认文件名
let defaultFileName = "Defualt_Book_Info";
// 检测是否已命名 
 file_title = ( file_title.includes("未命名") || file_title.toLowerCase().includes("untitled") ) ? (file_title=await tp.system.prompt("请输入要创建的文件名")) : file_title;
// 如果没输入任何文件名则赋个默认文件名
file_title == ""?(file_title=defaultFileName) : (await tp.file.rename(file_title));

// 获取文件存在的目录
let inputDir = await tp.system.prompt(" 目录地址 ",defaultFileName);
// 判断是否以 / 结束
inputDir = inputDir.endsWith("/")?inputDir:inputDir+"/";
// 默认书籍目录路径
let defaultDir="/Books/";
// 如果没有输入任何目录则赋个默认目录路径
if(inputDir == ""){
	inputDir = defaultDir;
}

// 重新组装文件路径 
// let fileFullPath = tp.file.exists((inputDir+inputFileName)+".md")?  : (inputDir+inputFileName)
// 移动文件到指定目录
await tp.file.move(inputDir+file_title)

-%>

---
booksdb: 
cover: 
category: 

---

# <% file_title %>

---

[书名:: ] 

[原版书名:: ]

[版本:: ]

[作者:: ]

[译者:: ]
  
[出版日期:: ]

[ISBN:: ]

 ![|520x600]()

