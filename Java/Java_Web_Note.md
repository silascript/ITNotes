---
aliases: []
tags:
  - java
  - javaweb
  - jdbc
  - http
created: 2023-01-31 11:31:14
modified: 2025-03-16 23:04:48
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

## Servlet

Tomcat 与 Servlet 版本对应关系：[Apache Tomcat® - Which Version Do I Want?](https://tomcat.apache.org/whichversion.html)

### JSP

JSP 本质还是 Servlet。

### Filter

### 乱码

1. [Html](../Frontend/Html_Note.md) 部分设置字符编码：

```html
<head>
	<meta charset="UTF-8">
</head>
```

2. JSP 页面顶部加入以下代码：

```jsp
 <%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
```

3. 使用 [Filter](#Filter) 来设置请求和响应的字符编码

过滤器相关代码：

```java
package tools;

import java.io.IOException;

import jakarta.servlet.Filter;
import jakarta.servlet.FilterChain;
import jakarta.servlet.FilterConfig;
import jakarta.servlet.ServletException;
import jakarta.servlet.ServletRequest;
import jakarta.servlet.ServletResponse;

public class CharacterEncoding implements Filter {

	String encoding = null;
	
	@Override
	public void init(FilterConfig filterConfig) throws ServletException {
		encoding = filterConfig.getInitParameter("encoding");
	}

	@Override
	public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain)
	throws IOException, ServletException {
	
	if (encoding != null) {
		servletRequest.setCharacterEncoding(encoding);
		servletResponse.setCharacterEncoding(encoding);
	}
	
	filterChain.doFilter(servletRequest, servletResponse);
	
	}

	@Override
	public void destroy() {
		encoding = null;
	}

}
```

过滤器 `web.xml` 中的设置：

```xml
<filter>
	<filter-name>setCharacterEncoding</filter-name>
	<filter-class>tools.CharacterEncoding</filter-class>
<init-param>
	<param-name>encoding</param-name>
	<param-value>UTF-8</param-value>
	</init-param>
</filter>

<filter-mapping>
	<filter-name>setCharacterEncoding</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>
```

---

## <span id="java_web_jdbc">JDBC</span>

### <span id="java_web_jdbc_newversion">新版 JDBC 相关</span>

配合 [MySQL](../DataBase/mysql/MySQL_Note.md) 8.x 版本，JDBC 的版本也更新为 6.x 及 最新的 8.x 版本，而新版本的一些设置也发生了变化。大概有如下这些：

1. 驱动名称更改为 `com.mysql.cj.jdbc.Driver`。
2. mysql 8 驱动的 `url` 必须设置时区，即 `serverTimezone=UTC`，否则会报错误。
   > [!info] 
   > 
   > 相关配置：[MySQL Time-zone](../DataBase/mysql/MySQL_Config_Note.md#time-zone)
   > 

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

[HikariCP](https://github.com/brettwooldridge/HikariCP) 是 [Spring](Spring/Spring_Note_1.md) 默认使用的数据链接池。

特点，就如其项目 [ReadMe](https://github.com/brettwooldridge/HikariCP#-hikaricpits-fasterhikari-hikal%C4%93-origin-japanese-light-ray) 中说的那样：「Fast, simple, reliable」。

---

## <span id="java_web_about">相关笔记</span>

* [Spring 笔记 1](./Spring/Spring_Note_1.md) 
* [Java 资料清单](Java_Material.md)
* [Java 笔记](Java_Note.md)

