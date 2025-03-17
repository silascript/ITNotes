---
aliases: []
tags:
  - xml
  - schema
created: 2023-08-18 19:44:52
modified: 2025-03-16 22:01:24
---

# XML Schema 笔记

---

## 命名空间

命名空间，Namespace，是一个有效防止同名标签冲突的机制。

### 定义命名空间

`targetNamespace` 属性用来指定该 Schema 的命名空间。

### 引用命名空间

`xmlns` 属性用来引用命名空间。

```xml
<web-app version="6.1" 
xmlns="https://jakarta.ee/xml/ns/jakartaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_6_1.xsd">

</web-app>
```

上面例子时 [Servlet](../Java/Java_Servlet_Note.md#Servlet) 中的配置文件 `web.xml` 的头部引用。

``

`xmlns` 的完整语法为：`xmlns[:xxx]`，**冒号**后是引用命名空间时，为此命名空间取的一个「标识符」，或称为「别名」。xml 使用该命名空间中的标签时，需要加上此标识符。

> [!info] 
>
> `xmlns="https://jakarta.ee/xml/ns/jakartaee"` 这名代码意思是 `web-app` 这个 xml 引用了 [https://jakarta.ee/xml/ns/jakartaee](https://jakarta.ee/xml/ns/jakartaee) 这个命名空间。
>
> `xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"` 同理，`xmlns:xsi` 这句代码意思是引用了 [http://www.w3.org/2001/XMLSchema-instance] (http://www.w3.org/2001/XMLSchema-instance) 这个命名空间，并且给了个「别名」，也就是说要使用 `XMLSchema-instance` 中的组件时，必须带着 `xsi` 这个「标识符」。
>
> `xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_6_1.xsd"` 这句代码就是使用了 `XMLSchema-instance` 的标签组件 `schemaLocation`，因为上面引用 `XMLSchema-instance` 命名空间时，给了 `xsi` 这个别名，所以使用时，得带着 `xsi`，所以写成 `xsi:schemaLocation`。

---

## 相关笔记

* [XML 笔记](XML_Note.md)
* [XML DTD 笔记](XML_DTD_Note.md)

