---
aliases: []
tags:
  - java
  - javaweb
  - javaee
  - jakartaee
  - servlet
  - jsp
  - http
created: 2023-01-31 11:31:14
modified: 2025-03-17 23:20:54
---

# Java Servlet 笔记

---

## 目录

* [HTTP及Web请求](#servlet_http_wrequest)
* [Servlet](#Servlet)

---

## <span id="servlet_http_wrequest">HTTP 及 Web 请求</span>

---

## Servlet

Tomcat 与 Servlet 版本对应关系：[Apache Tomcat® - Which Version Do I Want?](https://tomcat.apache.org/whichversion.html)

---

## JSP

JSP 本质还是 [Servlet](#Servlet)。

### JSP 标签

JSP 标签分两类：

* [指令标签](#指令标签)
* [动作标签](#动作标签)

#### 指令标签

JSP 指令标签是由 JSP 服务器解释并处理的用于设置 JSP 页面的相关属性或执行动作的一种标签，在一下指令标签中可以设置多个属性。这些属性的作用域范围是整个页面。

语法：`<%@指令名称 属性1="属性值" 属性2="属性值"... %>`

#### 动作标签

---

## Filter

---

## 乱码

1. [Html](../Frontend/Html_Note.md) 部分设置字符编码：

```html
<head>
	<meta charset="UTF-8">
</head>
```

2. JSP 页面顶部加入以下代码：

```jsp
 <%@ page pageEncoding="UTF-8"%>
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

## 相关笔记

* [JAVA EE 笔记](JAVA_EE_Note.md)
* [JDBC 笔记](Java_JDBC_Note.md)
* [Spring 笔记 1](./Spring/Spring_Note_1.md) 
* [Java 资料清单](Java_Material.md)
* [Java 笔记](Java_Note.md)

