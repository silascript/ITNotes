# Http 笔记
---
## 目录

* [一些概念](#http_concepts)
  * [资源](#http_concepts_resource)
  * [MIME](#http_concepts_mime)
  * [URI](#http_concepts_uri)
* [HTTP 解析](#http_parsing)

---

## <span id="http_concepts">一些概念</span>


### <span id="http_concepts_resource">资源</span>

Web 资源是 Web 内容的源头。

静态资源：文本文件、HTML 文件、图片文件等。

动态资源：根据需要生成内容的程序。这些动态内容资源可以根据你的身份、请求的信息或每天不同时段来产生内容。

### <span id="http_concepts_mime">MIME 类型</span>

MIME：Multipurpose Internet Mail Extension 多用途因特网邮件扩展

HTTP 为每种要通过 Web 传输的对象都打上了名为 **MIME** 类型的数据格式标签。

**MIME** 最初是为了解决在不同电子邮件系统间搬移报文时存在的问题。MIME 在电子邮件系统中工作得非常好，所以 HTTP 也采纳了它，用来描述并标记多媒体内容。


### <span id="http_concepts_uri">URI</span>

Web 服务器资源都有一个名字，这个名称被称为*统一资源标识符*（Uniform Resource Identifier）,即 *URI*。








---


## <span id="http_parsing">HTTP 解析</span>


---



