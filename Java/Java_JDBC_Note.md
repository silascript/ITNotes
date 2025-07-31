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
modified: 2025-07-31 17:41:56
---

# JDBC 笔记

---

## 简介

JDBC：Java Database Connectivity 是 Java 程序访问数据库的技术规范。

---

## <span id="jdbc_version">版本</span>

### <span id="jdbc_version_history">历史</span>

JDBC 3.0 随 JDK1.4 发布，JDBC 3.0 开始对应的 JSR 规范（JSR 是 Java Specification Request ，意思是 Java 规范提案）

JDBC 4.0 随 JDK1.6 发布，JDBC 4.0 开始有对应的 JDBC API 规范

JDBC 4.1 随 JDK1.7 发布

JDBC 4.2 随 JDK1.8 发布

### <span id="jdbc_version_new">新版 JDBC 相关</span>

配合 [MySQL](../DataBase/mysql/MySQL_Note.md) 8.x 版本，JDBC 的版本也更新为 6.x 及 最新的 8.x 版本，而新版本的一些设置也发生了变化。大概有如下这些：

1. 驱动名称更改为 `com.mysql.cj.jdbc.Driver`。
2. mysql 8 驱动的 `url` 必须设置时区，即 `serverTimezone=UTC`，否则会报错误。
   > [!info] 
   > 
   > 相关配置：[MySQL Time-zone](../DataBase/mysql/MySQL_Config_Note.md#time-zone)
   > 

---

## <span id="jdbc_dbconnpooling">数据连接池</span>

### <span id="jdbc_dbconnpooling_dbcp">DBCP</span>

DBCP（DataBase Connection Pool）是 [Apache](www.apache.org) 软件基金会下的开源连接池。

单独使用 DBCP 需要在系统中增加两个 jar 包：

* `commons-dbcp.jar`：连接池的实现
* `commons-pool.jar`：连接池实现的依赖库

### <span id="jdbc_dbconnpooling_c3p0">C3P0</span>

C3P0 也是一个开源的数据库连接池。

C3P0 之前**曾是**[Hibernate](Hibernate/Hibernate_Note.md) 和 [Spring](Spring/Spring_Note_1.md) 的使用到的数据库连接池。

使用 C3P0 同要需要引入两个 jar 包：
* `c3p0.jar`：C3P0 连接池的实现
* `mchange-commons.jar`：C3P0 连接池实现的依赖库

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