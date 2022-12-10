# Http 笔记
---
## 目录

* [一些概念](#http_concepts)
  * [资源](#http_concepts_resource)
  * [MIME](#http_concepts_mime)
  * [URI](#http_concepts_uri)
	  * [URL](#http_concepts_url) 
	  * [URN](#http_concepts_urn) 
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


#### <span id="http_concepts_uri">URI</span>

Web 服务器资源都有一个名字，这个名称被称为*统一资源标识符*（Uniform Resource Identifier）,即 *URI*。

给定了 **URI**， HTTP 就可以解析出对象。

**URI** 有两种形式： **URL** 和 **URN**。


#### <span id="http_concepts_url">URL</span>

URL 都遵循一种标准格式，这格式分为三个部分：
1. 方案（scheme） 指定访问资源所使用的协议类型，通常为 HTTP 协议。
2. 指定服务器的网络地址。
3. 其余部分指定了服务器上的某个资源



### <span id="http_concepts_urn">URN</span>

**URN**：统一资源名，是作为特定内容的唯一名称使用，与目前的资源所在地无关。使用这些与位置无关的 URN，就可以将资源四处搬移。



### <span id="http_concepts_message">报文</span>

**报文**，英文 「**message**」,其实就是要发送的数据信息。它是用于 HTTP 协议交换与传输的数据单元，即站点一次性要发送的数据块。

请求端的 HTTP 报文称为 「**请求报文**」；响应端的称为「**响应报文**」。

HTTP 报文本身是由多行数据构成的字符串文本，**纯文本**，非二进制代码，人们可以很方便对其进行读写。

HTTP  报文可分为报文首部和报文主体两块。两者由空行来划分。通常，不并不一定要有报文主体。


---


## <span id="http_parsing">HTTP 解析</span>


---



