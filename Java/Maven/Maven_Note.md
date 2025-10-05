---
aliases: []
tags:
  - java
  - maven
  - jdk
created: 2023-01-31 11:31:14
modified: 2025-10-06 05:43:16
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

| Maven 版本 | [JDK 版本](../Java_Note.md#java_jdk) |
|:----------:|:------------------------------------:|
|    3.8     |               JDK 1.7                |
|    3.9     |     [JDK8](../Java_Note.md#JDK8)     |
|    4.0     |    [JDK17](../Java_Note.md#JDK17)    |

## <span id="mvn_settings">配置</span>

配置文件按 [官方文档](https://maven.apache.org/settings.html) 分成两级：

1. 用户级（User Level）
	`~/.m2/` 目录下的 `settings.xml`。
2. 全局（Global Level）
	这个全局的配置文件就是放在 maven 安装根目录中的那个 `settings.xml` 文件。

一般建议配置 `~/.m2/` 目录下那个 `settings.xml`。

如果没有 `.m2` 目录，可以运行以下命令：

```shell
mvn help:system
```

> [!info] `.m2` 目录
> 
> 生成的 `.m2` 目录，默认在其中生成 `repository` 目录，这是默认的 [本地仓库](#mvn_setttings_localRepo)，可以在 settings.xml 配置中重新自定义别的路径。

生成完全 `.m2` 目录，可以将 maven 安装根目录中那个「全局」配置文件 `settings.xml` 复制到 `.m2` 下，作为「用户级」配置文件，然后再对其进行下一步配置。

Maven 配置主要有两块：
1. [本地仓库](#本地仓库) 存储位置的配置
2. [配置镜像](#配置镜像)

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

中央仓库地址：

* [https://mvnrepository.com/](https://mvnrepository.com/)

> [!tip] 
> 
> 如果是查询 jar 包，除了上面的中央仓库中查还可以到以下网站去查：
> * [Sonatype Maven Central](https://central.sonatype.com)（老的域名是：[ https://search.maven.org](https://search.maven.org)）

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

> [!info] 
> 
> `properties` 中 `source` 和 `target` 可以使用 `release` 来替代。
> 
> ```xml
> <properties>
>	<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
>	<release>21</release>
></properties>
> ```
> 
> 
> [Maven Repository: maven-compiler-plugin](https://mvnrepository.com/artifact/org.apache.maven.plugins/maven-compiler-plugin)

---

## <span id="mvn_plugins">Maven 插件</span>

maven 接口依赖关系图：

![maven deps shotcut](https://maven.apache.org/ref/3.9.4/images/maven-deps.png)

Maven 插件是下载到 [本地仓库](#本地仓库) 目录下的 `org/apache/maven/plugins` 目录中：
```shell
$ ll mvn_repository/org/apache/maven/plugins 
Permissions Size User       Group      Date Modified    Name
drwxr-xr-x     - silascript silascript 2025-03-20 20:58 .
drwxr-xr-x     - silascript silascript 2025-03-16 10:33 ..
drwxr-xr-x     - silascript silascript 2023-08-24 03:18 maven-antrun-plugin
drwxr-xr-x     - silascript silascript 2025-03-15 01:49 maven-archetype-plugin
drwxr-xr-x     - silascript silascript 2025-03-15 02:23 maven-assembly-plugin
drwxr-xr-x     - silascript silascript 2025-03-15 01:52 maven-clean-plugin
drwxr-xr-x     - silascript silascript 2025-03-15 01:52 maven-compiler-plugin
drwxr-xr-x     - silascript silascript 2025-03-15 02:23 maven-dependency-plugin
drwxr-xr-x     - silascript silascript 2025-03-15 03:51 maven-deploy-plugin
drwxr-xr-x     - silascript silascript 2025-03-20 20:58 maven-help-plugin
drwxr-xr-x     - silascript silascript 2025-03-15 03:47 maven-install-plugin
drwxr-xr-x     - silascript silascript 2025-03-05 23:42 maven-jar-plugin
.rw-r--r--   14k silascript silascript 2025-03-09 14:39 maven-metadata-apachemaven.xml
.rw-r--r--    40 silascript silascript 2025-03-20 20:58 maven-metadata-apachemaven.xml.sha1
.rw-r--r--   14k silascript silascript 2023-08-24 03:18 maven-metadata-central.xml
.rw-r--r--    40 silascript silascript 2023-08-24 03:18 maven-metadata-central.xml.sha1
drwxr-xr-x     - silascript silascript 2025-03-15 02:23 maven-plugins
drwxr-xr-x     - silascript silascript 2023-08-24 03:18 maven-release-plugin
drwxr-xr-x     - silascript silascript 2025-03-15 01:52 maven-resources-plugin
drwxr-xr-x     - silascript silascript 2023-08-24 03:18 maven-site-plugin
drwxr-xr-x     - silascript silascript 2025-03-15 03:52 maven-surefire-plugin
drwxr-xr-x     - silascript silascript 2025-03-15 03:07 maven-war-plugin
.rw-r--r--   250 silascript silascript 2025-03-20 20:58 resolver-status.properties

```

如最开始的 `mvn help:system` 命令，实质就是下载了 `maven-help-plugin` 这个插件，这插件包括了 `pom` 文件及 `jar` 包。

```shell

$ mvn help:system
[INFO] Scanning for projects...
Downloading from apachemaven: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-install-plugin/3.1.2/maven-install-plugin-3.1.2.pom
Downloaded from apachemaven: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-install-plugin/3.1.2/maven-install-plugin-3.1.2.pom (8.5 kB at 937 B/s)
Downloading from apachemaven: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-deploy-plugin/3.1.2/maven-deploy-plugin-3.1.2.pom
Downloaded from apachemaven: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-deploy-plugin/3.1.2/maven-deploy-plugin-3.1.2.pom (9.6 kB at 22 kB/s)
Downloading from apachemaven: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-metadata.xml
Downloading from apachemaven: https://repo.maven.apache.org/maven2/org/codehaus/mojo/maven-metadata.xml
Downloaded from apachemaven: https://repo.maven.apache.org/maven2/org/codehaus/mojo/maven-metadata.xml (21 kB at 89 kB/s)
Downloaded from apachemaven: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-metadata.xml (14 kB at 1.3 kB/s)
Downloading from apachemaven: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-help-plugin/maven-metadata.xml
Downloaded from apachemaven: https://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-help-plugin/maven-metadata.xml (807 B at 5.8 kB/s)

$ ll mvn_repository/org/apache/maven/plugins/maven-help-plugin 
Permissions Size User       Group      Date Modified    Name
drwxr-xr-x     - silascript silascript 2025-03-20 20:58 .
drwxr-xr-x     - silascript silascript 2025-03-20 20:58 ..
drwxr-xr-x     - silascript silascript 2024-02-23 06:46 3.4.0
drwxr-xr-x     - silascript silascript 2025-03-15 02:24 3.5.1
.rw-r--r--   807 silascript silascript 2024-10-22 01:52 maven-metadata-apachemaven.xml
.rw-r--r--    40 silascript silascript 2025-03-20 20:58 maven-metadata-apachemaven.xml.sha1
.rw-r--r--   714 silascript silascript 2023-08-24 03:18 maven-metadata-central.xml
.rw-r--r--    40 silascript silascript 2023-08-24 03:18 maven-metadata-central.xml.sha1
.rw-r--r--   250 silascript silascript 2025-03-20 20:58 resolver-status.properties

$ ll mvn_repository/org/apache/maven/plugins/maven-help-plugin/3.5.1 
Permissions Size User       Group      Date Modified    Name
drwxr-xr-x     - silascript silascript 2025-03-15 02:24 .
drwxr-xr-x     - silascript silascript 2025-03-20 20:58 ..
.rw-r--r--   222 silascript silascript 2025-03-15 02:24 _remote.repositories
.rw-r--r--   65k silascript silascript 2024-10-18 18:11 maven-help-plugin-3.5.1.jar
.rw-r--r--    40 silascript silascript 2025-03-15 02:24 maven-help-plugin-3.5.1.jar.sha1
.rw-r--r--   10k silascript silascript 2024-10-18 18:11 maven-help-plugin-3.5.1.pom
.rw-r--r--    40 silascript silascript 2025-03-15 02:24 maven-help-plugin-3.5.1.pom.sha1

```

### 编译插件

```xml
<project>
	<build>
		<pluginManagement>
			<plugins>
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-compiler-plugin</artifactId>
					<version>3.14.0</version>
					<configuration>
					<!-- put your configurations here -->
					</configuration>
				</plugin>
			</plugins>
		</pluginManagement>
	</build>
</project>
```

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

