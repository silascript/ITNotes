# Java Web 笔记

---

## 目录

* [HTTP及Web请求](#java_web_http_wrequest)
* [JDBC](#java_web_jdbc)
	* [数据库连接池](#java_web_jdbc_dbconnpooling)
---

## <span id="java_web_http_wrequest">HTTP及Web请求</span>



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


