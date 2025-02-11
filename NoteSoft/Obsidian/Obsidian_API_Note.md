---
aliases: []
tags:
  - obsidian
  - api
created: 2025-02-11 22:00:33
modified: 2025-02-11 23:33:31
---
# Obsidian 接口笔记

---

## 文件系统

### getFolderByPath

在 Obsidian 中，你可以使用 **Templater** 插件结合 JavaScript 代码来检测某个目录路径是否存在相应的目录。Obsidian 的 API 提供了 `app.vault.getFolderByPath` 方法，可以用来检查指定路径是否存在文件夹。

#### 实现方法

##### 1. 使用 `app.vault.getFolderByPath`
`app.vault.getFolderByPath` 方法会根据给定的路径返回对应的文件夹对象。如果路径不存在，则返回 `null`。通过判断返回值是否为 `null`，可以确定目录是否存在。

##### 2. 示例代码
以下是一个完整的示例代码，用于检测指定目录路径是否存在：

```javascript
<%*
// 定义要检测的目录路径
const folderPath = "你的目录路径"; // 例如 "Notes/Projects"

// 获取文件夹对象
const folder = app.vault.getFolderByPath(folderPath);

if (folder) {
    tR += `目录 "${folderPath}" 存在。`;
} else {
    tR += `目录 "${folderPath}" 不存在。`;
}
%>
```

#### 代码说明
1. `folderPath`  
   这是你要检测的目录路径。例如，如果你的目录结构如下：
   ```
   Notes/
     - Projects/
     - Personal/
   ```
   你可以将 `folderPath` 设置为 `"Notes/Projects"`。

2. `app.vault.getFolderByPath`  
   该方法会根据路径查找文件夹。如果找到，返回文件夹对象；否则返回 `null`。

3. **判断逻辑**  
   * 如果 `folder` 不为 `null`，说明目录存在。
   * 如果 `folder` 为 `null`，说明目录不存在。

---

#### 示例运行

假设你的目录结构如下：
```
Notes/
  - Projects/
  - Personal/
```

##### 情况 1：目录存在

将 `folderPath` 设置为 `"Notes/Projects"`，运行脚本后会输出：
```
目录 "Notes/Projects" 存在。
```

##### 情况 2：目录不存在

将 `folderPath` 设置为 `"Notes/Work"`，运行脚本后会输出：
```
目录 "Notes/Work" 不存在。
```

#### 扩展功能

你可以结合 `tp.system.prompt` 或 `tp.system.suggester` 实现动态检测用户输入的目录路径是否存在。例如：

```javascript
<%*
// 获取用户输入的目录路径
const folderPath = await tp.system.prompt("请输入要检测的目录路径：");

if (folderPath !== null) {
    // 检测目录是否存在
    const folder = app.vault.getFolderByPath(folderPath);

    if (folder) {
        tR += `目录 "${folderPath}" 存在。`;
    } else {
        tR += `目录 "${folderPath}" 不存在。`;
    }
} else {
    tR += "你取消了输入。";
}
%>
```

> [!important] 
> 
> 要检测的路径，不能以 `/` 开头，即不能是「绝对路径」。而都是以 [vault](Obsidian_Note.md#vault) 为根的「相对路径」。

#### 总结

通过 `app.vault.getFolderByPath` 方法，你可以轻松检测 Obsidian 中某个目录路径是否存在。结合 Templater 插件的交互功能（如 `tp.system.prompt`），可以实现更动态和灵活的操作。

----

## 相关笔记

* [Obsidian笔记](Obsidian_Note.md)
* [Obsidian资料清单](Obsidian_Material.md)
* [Obsidian 部分插件笔记](Obsidian_Plugins_Note.md)

