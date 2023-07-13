<%*
// 绑定默认新键笔记快捷键

// 用户输入目录路径
// await let userInputDir = tp.system.prompt("请输入目录路径","General_Temp/")
// let userInputDir = tp.system.prompt("请输入目录路径")
// let inputDir = await tp.system.prompt("请输入目录路径")

// console.log(inputDir)
// console.log(app.vault.getAbstractFileByPath(inputDir))

// tp.file.create_new(tp.file.find_tfile("TPM_General"),"/"+userInputDir+"/"+"General_"+tp.date.now("YYYYMMDDHHmmss"))
tp.file.create_new(tp.file.find_tfile("TPM_General"))

%>