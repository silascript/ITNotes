---
aliases:
  - 
tags:
  - java
  - javaweb
  - jdbc
  - http
created: 2023-01-31 11:31:14
modified: 2023-05-26 5:55:21
---
# Java Web 笔记

---

## 目录

* [HTTP及Web请求](#java_web_http_wrequest)
* [JDBC](#java_web_jdbc)
	* [数据库连接池](#java_web_jdbc_dbconnpooling)
---

## <span id="java_web_http_wrequest">HTTP 及 Web 请求</span>

---

## <span id="java_web_jdbc">JDBC</span>

### <span id="java_web_jdbc_newversion">新版 JDBC 相关</span>

配合 MySQL 8.x 版本，JDBC 的版本也更新为 6.x 及 最新的 8.x 版本，而新版本的一些设置也发生了变化。大概有如下这些：

1. 驱动名称更改为 `com.mysql.cj.jdbc.Driver`。
2. mysql 8 驱动的 `url` 必须设置时区，即 `serverTimezone=UTC`，否则会报如下错误。

### <span id="java_web_jdbc_dbconnpooling">数据连接池</span>

#### <span id="java_web_jdbc_dbconnpooling_druid">Druid</span>

[druid](https://github.com/alibaba/druid) 是阿里出的一个数据连接池。

maven 依赖：

```xml
	<dependency>
		<groupId>com.alibaba</groupId>
		<artifactId>druid</artifactId>
		<version>1.2.9</version>
    </dependency>
```

---

### <span id="java_web_jdbc_dbconnpooling_hikaricp">HikariCP</span>

[HikariCP](https://github.com/brettwooldridge/HikariCP) 是 [Spring_Note_1](Spring/Spring_Note_1.md) 默认使用的数据链接池。

特点，就如其项目 [ReadMe](https://github.com/brettwooldridge/HikariCP#-hikaricpits-fasterhikari-hikal%C4%93-origin-japanese-light-ray) 中说的那样：「Fast, simple, reliable」。

---

## <span id="java_web_tools_frameworks">各种工具框架笔记</span>

* [Spring 笔记 1](./Spring/Spring_Note_1.md) 

