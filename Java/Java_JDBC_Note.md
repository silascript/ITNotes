---
aliases: []
tags:
  - java
  - javaee
  - jakartaee
  - jdbc
  - db
  - database
created: 2025-03-17 20:02:31
modified: 2025-03-17 23:21:11
---

# JDBC 笔记

---

## 简介

JDBC：Java Database Connectivity 是 Java 程序访问数据库的技术规范。

---

## <span id="jdbc_newversion">新版 JDBC 相关</span>

配合 [MySQL](../DataBase/mysql/MySQL_Note.md) 8.x 版本，JDBC 的版本也更新为 6.x 及 最新的 8.x 版本，而新版本的一些设置也发生了变化。大概有如下这些：

1. 驱动名称更改为 `com.mysql.cj.jdbc.Driver`。
2. mysql 8 驱动的 `url` 必须设置时区，即 `serverTimezone=UTC`，否则会报错误。
   > [!info] 
   > 
   > 相关配置：[MySQL Time-zone](../DataBase/mysql/MySQL_Config_Note.md#time-zone)
   > 

---

## <span id="jdbc_dbconnpooling">数据连接池</span>

### <span id="jdbc_dbconnpooling_druid">Druid</span>

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

### <span id="jdbc_dbconnpooling_hikaricp">HikariCP</span>

[HikariCP](https://github.com/brettwooldridge/HikariCP) 是 [Spring](Spring/Spring_Note_1.md) 默认使用的数据链接池。

特点，就如其项目 [ReadMe](https://github.com/brettwooldridge/HikariCP#-hikaricpits-fasterhikari-hikal%C4%93-origin-japanese-light-ray) 中说的那样：「Fast, simple, reliable」。

---

## 相关笔记

* [JAVA EE 笔记](JAVA_EE_Note.md)
* [Java 笔记](Java_Note.md)