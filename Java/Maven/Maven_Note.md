---
aliases:
  - 
tags:
  - java
  - maven
  - jdk
created: 2023-01-13, 12:27:45
modified: 2023-01-31, 9:50:15
---
# Maven 笔记

---

## 目录
* [配置](mvn_settings)
	* [本地仓库](mvn_settings_localRepo)
* [Maven 项目](#mvn_project)
	* [JDK 版本指定](#mvn_project_jdk_version)

---

## <span id="mvn_settings">配置</span>

Maven 配置主要有两块配置：
1. 配置本地仓库存储位置
2. 镜像配置

### <span id="mvn_settings_localRepo">本地仓库</span>

```xml
<localRepository>D:/mvn_repository/</localRepository>
```

### <span id="mvn_settings_remoteRepo">远程仓库</span>

#### <span id="mvn_settings_remoteRepo_mvnrepository">中央仓库</span>

中央仓库地址：[https://mvnrepository.com/](https://mvnrepository.com/)

---

## <span id="mvn_settings_mirror">配置镜像</span>

国内镜像主要是 [阿里](https://developer.aliyun.com/mvn/guide)、[网易](https://mirrors.163.com/.help/maven.html)、华为和腾讯。

大体以阿里为主。

国内镜像配置如下：

```xml
	<mirror>
      <id>aliyun-central</id>
      <mirrorOf>central</mirrorOf>
      <name>阿里云公共仓库</name>
      <url>https://maven.aliyun.com/repository/central</url>
    </mirror>


    <mirror>
      <id>huaweicloud</id>
      <mirrorOf>central</mirrorOf>
      <name>HuaWei</name>
      <url>https://repo.huaweicloud.com/repository/maven/</url>
    </mirror>

    <mirror>
      <id>nju_mirror</id>
      <mirrorOf>central</mirrorOf>
      <url>https://repo.nju.edu.cn/repository/maven-public/</url>
    </mirror>

    <mirror>
      <id>apachemaven</id>
      <mirrorOf>central</mirrorOf>
      <name>apache repo</name>
      <url>https://repo.maven.apache.org/maven2/</url>
    </mirror>

    <mirror>
      <id>repomaven</id>
      <mirrorOf>central</mirrorOf>
      <name>central repo</name>
      <url>https://repo1.maven.org/maven2/</url>
    </mirror>

    <mirror>
      <id>aliyun-spring</id>
      <mirrorOf>spring</mirrorOf>
      <name>阿里云Spring仓库</name>
      <url>https://maven.aliyun.com/repository/spring</url>
    </mirror>

    <mirror>
      <id>aliyun-spring-plugin</id>
      <mirrorOf>spring-plugin</mirrorOf>
      <name>阿里云Spring-plugin仓库</name>
      <url>https://maven.aliyun.com/repository/spring-plugin</url>
    </mirror>

    <mirror>
      <id>aliyun-gradle-plugin</id>
      <mirrorOf>gradle-plugin</mirrorOf>
      <name>阿里云gradle-plugin仓库</name>
      <url>https://maven.aliyun.com/repository/gradle-plugin</url>
    </mirror>

    <mirror>
      <id>aliyun-grails-core</id>
      <mirrorOf>grails-core</mirrorOf>
      <name>阿里云grails-core仓库</name>
      <url>https://maven.aliyun.com/repository/grails-core</url>
    </mirror>

    <mirror>
      <id>aliyun-apache-snapshots</id>
      <mirrorOf>apache-snapshots</mirrorOf>
      <name>阿里云apache snapshots仓库</name>
      <url>https://maven.aliyun.com/repository/apache-snapshots</url>
    </mirror>

```

检测配置是否成功：
```shell
mvn help:system
```

---

## <span id="mvn_project">maven 项目</span>

### 通用目录结构

Maven 默认约定了一套目录结构。

```shell
${basedir}
|-- pom.xml
|-- src
|	|-- main
|	|	`-- java
|	|	`-- resources
|	|	`-- filters
|	`-- test
|	|	`-- java
|	|	`-- resources
|	|	`-- filters
|	`-- it
|	`-- assembly
|	`-- site
`-- LICENSE.txt
`-- NOTICE.txt
`-- README.txt


```

* `src/main/java` 项目源码所在的目录
* `src/main/resources` 目录存放用于项目的资源的文件
* `src/main/filters` 项目的资源过滤文件所在的目录
* `src/main/webapp` 如果是 web 项目，这目录则是 web 应用源码所在的目录，像 html 文件和 `web.xml` 等都在该目录下。
* `src/test/java` 测试代码目录
* `src/test/resources` 测试相关的资源文件所在的目录
* `src/test/filters` 测试相关的资源过滤文件所在的目录

* `src/it` 集成测试代码所在的目录
* `src/assembly` 组件（Assembly） 描述符所在目录
* `src/site` 站点文件

---

### <span id="mvn_project_settings_codeending">编码设置</span>

```xml
<properties>  
	<!-- 文件拷贝时的编码 -->  
	<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>  
	<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>  
	<!-- 编译时的编码 -->  
	<maven.compiler.encoding>UTF-8</maven.compiler.encoding>  
</properties>
```

---

### <span id="mvn_project_jdk_version">JDK 版本指定</span>

java11 指定
`settings` 文件配置：
```xml
<profile>
  <id>jdk-11</id>    
    <activation>    
		<activeByDefault>true</activeByDefault>    
		<jdk>11</jdk>    
	</activation>    
  <properties>    
    <maven.compiler.source>11</maven.compiler.source>    
    <maven.compiler.target>11</maven.compiler.target>    
    <maven.compiler.compilerVersion>11</maven.compiler.compilerVersion>    
  </properties>
</profile>
```

项目 `pom.xml`：
1. 
```xml
<properties>
	<maven.compiler.source>11</maven.compiler.source>
	<maven.compiler.target>11</maven.compiler.target>
</properties>
```

2. 
```xml
<build>
	<plugins>
		<plugin>
		    <groupId>org.apache.maven.plugins</groupId>
		    <artifactId>maven-compiler-plugin</artifactId>
		    <version>3.10.1</version>
		    <configuration>
		        <release>11</release> 
				<encoding>UTF-8</encoding>
		    </configuration>
		</plugin>
	</plugins>
</build>
```

---

