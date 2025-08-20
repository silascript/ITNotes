---
aliases: []
tags:
  - books
  - list
created: 2025-02-07 04:09:45
modified: 2025-08-20 21:45:25
---

# IT 书籍清单

---

~~ > [!tip] ~~
~~ > ~~
~~ > 此清单依赖插件 [Dataview](../../NoteSoft/Obsidian/Obsidian_Plugins_Note.md#Dataview) 插件。~~

~~```dataview ~~
~~ Table without ID ~~
~~	书名 , ~~
~~	原版书名, ~~
~~	版本, ~~
~~	作者, ~~
~~	译者, ~~
~~	出版日期, ~~
~~	ISBN, ~~
~~	category as 分类,~~
~~	cover as 封面 ~~
~~ From "" ~~
~~ Where econtains(booksdb, "BooksList") ~~
~~ AND contains(file.path,"Templates")=false ~~
~~``` ~~

```base
views:
  - type: table
    name: 表格
    filters:
      and:
        - file.inFolder("Books")
        - file.hasTag("book")
        - file.hasTag("brief")
```

