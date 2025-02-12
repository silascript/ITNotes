<%* 

	// 此模板用于生成书籍信息笔记

	// 检测目录路径是否存在
	// false 目录不存在
	// true 目录存在
	function validateDirExist(dirpath){
	
		// console.log(dirpath);
		// return app.vault.getFolderByPath(dirpath) == null?false:true;
		// console.log(app.vault.getFolderByPath(dirpath));
		let dir = app.vault.getFolderByPath(dirpath)
		// console.log(dir);
		if( dir == null){
			return false;
		}else{
			return true;	
		}
	}

	// 创建目录
	function createDirbyPath(dirpath){
	
		// console.log(dirpath);
		let isConfirmed = confirm(`是否创建${dirpath}目录？`)

		// console.log("isConfirmed："+isConfirmed);
	
		if(isConfirmed){
			// 创建目录
			validateDirExist(dirpath) ? alert(`${dirpath}目录已存在，无须创建！`) : app.vault.createFolder(dirpath);
			return true;
		}else{
			// 放弃创建目录
			return false;
		}
	
	}

	// 生成书籍信息笔记文件
	// 参数 dirpth 将要存放笔记的目录路径
	async function createNoteFile(dirpath){
	// 判断是否以 / 结束
		// 尾部没有目录分隔符/就补上
		dirpath = dirpath.endsWith("/")?dirpath:dirpath+"/";
		// 加上根路径，无论当前焦点位于什么目录，都从根定位选择或输入的目录路径
		dirpath = dirpath.startsWith("/")?dirpath:"/"+dirpath;

		// console.log(dirpath);
		// 重新组装文件路径 
		// let fileFullPath = tp.file.exists((inputDir+inputFileName)+".md")?  : (inputDir+inputFileName)
		// 文件完整路径
		let fileFullPath = dirpath+file_title;
		// console.log(fileFullPath);
		// 移动文件到指定目录
		await tp.file.move(fileFullPath);
	
	}



	////////////////////////////////////////////////////////////////

	let file_title= tp.file.title;
	// 默认文件名
	let defaultFileName = "Defualt_Book_Info";
	
	// 检测是否已命名 
	file_title = ( file_title.includes("未命名") || file_title.toLowerCase().includes("untitled") ) ? (file_title=await tp.system.prompt("请输入要创建的文件名")) : file_title;
	// 如果没输入任何文件名则赋个默认文件名
	file_title == ""?(file_title=defaultFileName) : (await tp.file.rename(file_title));

	// 默认书籍目录路径
	let defaultDir="/Books/";
	
	// vault库的根目录
	// const vaultRoot = app.vault.getRoot()
	// console.log(vaultRoot);
	
	// 所有目录
	// 即vault库的所有目录
	// const folders = app.vault.getAllFolders(true)
	// 取出目录路径
	// let folders = app.vault.getAllFolders(true).map(f=>f.path)
	let folders = app.vault.getAllFolders().map(f=>f.path)
	
	// 列出所有目录
	// 获取目录对象
	let selectedFolder = await tp.system.suggester(e=>e,folders,false,"选择存放的目录")
	
	// console.log("目录路径："+selectedFolder)
	let tempDir = defaultDir
	if(selectedFolder != ""){
		// tempDir="/"+selectedFolder
		tempDir=selectedFolder
	}
	
	// 输入将保存书籍笔记的目录路径
	let inputDir = await tp.system.prompt("请输入目录地址",tempDir);
	

	// 如果没有输入任何目录则赋个默认目录路径
	if(inputDir == ""){
		inputDir = defaultDir+"/Books_Temp";
	}

	// console.log(inputDir);
	// let validateResult=validateDirExist(inputDir);
	// 检测目录路径是否存在
	if(!validateDirExist(inputDir)){
		// 目录不存在将创建目录
		// let isCreate = createDirbyPath(inputDir);
		// createDirbyPath(inputDir);
		if(!createDirbyPath(inputDir)){
			// 放弃生成笔记
			return;
		}
	}

	// console.log(inputDir);
	// 生成笔记
	await createNoteFile(inputDir);
		

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

