---
aliases: []
tags:
  - java
  - javaee
  - jakartaee
created: 2025-03-17 20:00:46
modified: 2025-03-22 23:13:30
---

# Java EE 笔记

---

## 简介

Java EE：Java Platform Enterprise Edition。

2018 年 3 月更名为**Jakarta EE**。

Java EE 是一系列技术标准所组成的平台，包括：

* EJB - 企业级 JavaBean（Enterprise Java Beans）
* JAAS - Java Authentication and Authorization Service
* JACC - J2EE Authorization Contract for Containers
* JAF - Java Beans Activation Framework
* JAX-RPC - Java API for XML-Based Remote Procedure Calls
* JAX-WS - Java API for XML Web Services
* JAXM - Java API for XML Messaging
* JAXP - Java XML 解析 API（Java API for XML Processing）
* JAXR - Java API for XML Registries
* JCA - J2EE 连接器架构（J2EE Connector Architecture）
* [JDBC](Java_JDBC_Note.md) - Java 数据库联接（Java Database Connectivity）
* JMS - Java 消息服务（Java Message Service）
* JMX - Java Management
* JNDI - Java 名称与目录接口（Java Naming and Directory Interface）
* JSF - Java Server Faces
* [JSP](Java_Servlet_Note.md#JSP) - Java 服务器页面（Java Server Pages）
* JSTL - Java 服务器页面标准标签库（Java Server Pages Standard Tag Library）
* JTA - Java 事务 API（Java Transaction API）
* JavaMail
* [Servlet](Java_Servlet_Note.md) - Java Servlet API
* StAX - Streaming APIs for XML Parsers
* WS - Web Services
* Applet - Java Applet

---

## 一些概念

### Web 容器

抽象层面，可以将 Web 容器视为运行 [Servlet](Java_Servlet_Note.md#Servlet)/[JSP](Java_Servlet_Note.md#JSP) 的 [HTTP](../Network/Http_Note.md) 服务器。

Web 容器介于实体 HTTP 服务器与 Servlet 之间。

---

## 相关笔记

* [Java 笔记](Java_Note.md)
* [Java 资料清单](Java_Material.md)

