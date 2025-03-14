---
aliases: []
tags:
  - java
  - maven
  - jdk
created: 2023-01-31 11:31:14
modified: 2025-03-15 01:59:44
---

# Maven 笔记

---

## 目录

* [配置](#mvn_settings)
	* [本地仓库](#mvn_settings_localRepo)
* [Maven 项目](#mvn_project)
	* [JDK 版本指定](#mvn_project_jdk_version)

---

## <span>Maven 版本的 JDK 版本要求</span>

maven 版本与 JDK 版本对应请参考：[Maven – Maven Releases History](https://maven.apache.org/docs/history.html)

> [!tip]
> 
> **3.9.4**开始最低都是 Java8。

## <span id="mvn_settings">配置</span>

Maven 配置主要有两块配置：
1. 配置本地仓库存储位置
2. 镜像配置

配置文件按官方文档分成两级：

1. 用户级（User Level）
	`~/.m2/` 目录下的 `settings.xml`。
2. 全局（Global Level）
	这个全局的配置文件就是放在 maven 安装根目录中的那个 `settings.xml` 文件。

如果没有 `.m2` 目录，可以运行以下命令：

```shell
mvn help:system
```

> [!info] `.m2` 目录
> 
> 生成的 `.m2` 目录，默认在其中生成 `repository` 目录，这是默认的 [本地仓库](#mvn_setttings_localRepo)，可以在 settings.xml 配置中重新自定义别的路径。

生成完全 `.m2` 目录，可以将 maven 安装根目录中那个「全局」配置文件 `settings.xml` 复制到 `.m2` 下，作为「用户级」配置文件，然后再对其进行下一步配置。

### <span id="mvn_settings_localRepo">本地仓库</span>

> [!example] 配置示例
>
> Windows：
> 
>```xml
> <localRepository>D:/mvn_repository/</localRepository>
> ```
>
> Linux：
>
>```xml
> <localRepository>${user.home}/mvn_repository</localRepository>
> ```

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

> [!tip] 3.8.1 新增标签
> 
> Maven 3.8.1 禁止了 http 协议，所以增加了 `maven-default-http-blocker` 这组标签用来拦截 `http` 请求。
> 

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
<profile>
    <properties>
        <!-- 文件拷贝时的编码 -->
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <!-- 编译时的编码 -->
        <maven.compiler.encoding>UTF-8</maven.compiler.encoding>
	</properties>
</profile>
```

---

### <span id="mvn_project_jdk_version">JDK 版本指定</span>

java11 指定 `settings` 文件配置：

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

java17：

```xml
<profile>
  <id>jdk-17</id>    
    <activation>    
		<activeByDefault>true</activeByDefault>    
		<jdk>17</jdk>    
	</activation>    
  <properties>    
    <maven.compiler.source>17</maven.compiler.source>    
    <maven.compiler.target>17</maven.compiler.target>    
    <maven.compiler.compilerVersion>17</maven.compiler.compilerVersion>    
  </properties>
</profile>
```

项目 `pom.xml`：
1. 
```xml
<properties>
	<maven.compiler.source>11</maven.compiler.source>
	<maven.compiler.target>11</maven.compiler.target>
	<maven.compiler.compilerVersion>11</maven.compiler.compilerVersion>
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

示例，最简单的 maven 项目：

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

<modelVersion>4.0.0</modelVersion>

	<groupId>silascript.exercise.func</groupId>
	<artifactId>func_exercise</artifactId>
	<version>1.0-SNAPSHOT</version>
	
	<properties>
		<maven.compiler.source>21</maven.compiler.source>
		<maven.compiler.target>21</maven.compiler.target>
		<maven.compiler.compilerVersion>21</maven.compiler.compilerVersion>
	</properties>
	
	<build>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.14.0</version>
				<configuration>
					<release>21</release>
					<encoding>UTF-8</encoding>
				</configuration>
			</plugin>
		</plugins>
	</build>

</project>
```

---

## <span id="mvn_plugins">Maven 插件</span>

maven 接口依赖关系图：

![maven deps shotcut](https://maven.apache.org/ref/3.9.4/images/maven-deps.png)

---

## 小技巧

### 排查版本依赖

```shell
mvn dependency:tree
```

---

## 相关笔记

* [Maven 资料清单](Maven_Material.md)
* [Maven 视频清单](./Maven_Videos.md)

