---
aliases: []
tags:
  - java
  - maven
  - jdk
created: 2023-01-31 11:31:14
modified: 2025-10-10 21:49:08
---

# Maven 笔记

---

## 目录

* [配置](#mvn_settings)
* [仓库](#mvn_repository)
	* [本地仓库](#mvn_repository_local)
	* [远程仓库](#mvn_repository_remote)
		* [中央仓库](#mvn_repository_remote_central)
* [镜像](#mvn_mirror)
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

Maven 配置主要有以下几个：

1. [本地仓库](#mvn_repository_local) 
2. [远程仓库](#mvn_repository_remote)
3. [配置镜像](#配置镜像)

---

## <span id="mvn_repository">仓库</span>

### <span id="mvn_repository_local">本地仓库</span>

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

### <span id="mvn_repository_remote">远程仓库</span>

#### <span id="mvn_repository_remote_central">中央仓库</span>

在 `org/apache/maven/maven-model-builder/3.0/` 中有个 `maven-model-builder-3.0.jar`。解包，其中有个 `pom.xml`，其中可以看到中央仓库的配置：

```xml
<repositories>
    <repository>
      <id>central</id>
      <name>Maven Repository Switchboard</name>
      <url>http://repo1.maven.org/maven2</url>
      <layout>default</layout>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
    </repository>
  </repositories>

  <pluginRepositories>
    <pluginRepository>
      <id>central</id>
      <name>Maven Plugin Repository</name>
      <url>http://repo1.maven.org/maven2</url>
      <layout>default</layout>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
      <releases>
        <updatePolicy>never</updatePolicy>
      </releases>
    </pluginRepository>
  </pluginRepositories>
```

> [!tip] 
> 
> 可以看到中央仓库和 [插件仓库](#mvn_repository_plugin)，都使用了 [http://repo1.maven.org/maven2](http://repo1.maven.org/maven2) 这个地址。

Maven 的中央仓库，其实是有两大中央仓库：

* [Maven Central](#MavenCentral)
* [JCenter](#JCenter)

> [!info] 
> 
> 事实上两个仓库都具有相同的使命：提供 Java 或者 Android library 服务。上传到哪个（或者都上传）取决于开发者。
>
> 起初，Android Studio 选择 [MavenCentral](#MavenCentral)] 作为默认仓库。如果你使用老版本的 Android Studio 创建一个新项目，`mavenCentral()` 会自动的定义在 `build.gradle` 中 。
>
> 但是 Maven Central 的最大问题是对开发者不够友好。上传 library 异常困难。上传上去的开发者都是某种程度的极客。同时还因为诸如安全方面的其他原因，Android Studio 团队决定把默认的仓库替换成 [JCenter](#JCenter)。正如你看到的，一旦使用最新版本的 Android Studio 创建一个项目，`jcenter()` 自动被定义，而不是 `mavenCentral()`。

##### MavenCentral

Maven 中央仓库（[http://repo1.maven.org/maven2/](http://repo1.maven.org/maven2/)）是由 Sonatype 公司提供的服务，它是 Apache Maven、SBT 和其他构建系统的默认仓库。

* [https://mvnrepository.com/](https://mvnrepository.com/)

> [!tip] 
> 
> 如果是查询 jar 包，除了上面的中央仓库中查还可以到以下网站去查：
> * [Sonatype Maven Central](https://central.sonatype.com)（老的域名是：[ https://search.maven.org](https://search.maven.org)）

##### JCenter

JCenter 仓库（[https://jcenter.bintray.com](https://jcenter.bintray.com)）是由 JFrog 公司提供的 Bintray 中的 [Java](../Java_Note.md) 仓库。

它是当前世界上最大的 Java 和 Android 开源软件构件仓库。 所有内容都通过内容分发网络（CDN）使用加密 https 连接获取。JCenter 是 [Groovy](../Groovy_Gradle/Groovy_Note.md) Grape 内的默认仓库。

JCenter 相比 [mavenCentral](#mavenCentral) 构件更多，性能也更好。但还是有些构件仅存在 [mavenCentral](#mavenCentral) 中。

> [!important] 
> 
> JCenter 已经停止运营了，所以只能用 [MavenCentral](#MavenCentral)。

### <span id="mvn_repository_plugin">插件仓库</span>

> [!info] 
> 
> 通过对 `maven-model-builder-3.0.jar` 解包，从其中的 `pom.xml` 文件得知，默认的插件仓库与 [中央仓库](#mvn_repository_remote_central) 默认地址是一样的： [http://repo1.maven.org/maven2](http://repo1.maven.org/maven2) 。

插件仓库使用 `<pluginRepositories>` 及 `pluginRepository` 标签来配置。

示例（Maven 官方默认的插件仓库配置）：

```xml
  <pluginRepositories>
    <pluginRepository>
      <id>central</id>
      <name>Maven Plugin Repository</name>
      <url>http://repo1.maven.org/maven2</url>
      <layout>default</layout>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
      <releases>
        <updatePolicy>never</updatePolicy>
      </releases>
    </pluginRepository>
  </pluginRepositories>
```

插件仓库定义标签说明：

* `<pluginRepositories>`：插件仓库组，其中可以定义多个插件仓库。
* `<pluginRepository>`：插件仓库定义
	* `<id>`：插件仓库唯一标识
	* `<name>`：插件仓库描述
	* `<url>`：插件仓库地址

### <span id="mvn_repository_multiple">多仓库</span>

#### Maven 依赖查找顺序 (按优先级排序，由高到低)

* 1.[本地仓库](#mvn_repository_local) 
    * 路径：`settings.xml` 中 `<localRepository>` 指定的位置，默认 `~/.m2/repository`
    * 优先级最高：如果本地已经有对应版本，Maven 默认直接使用，不会再去 [远程仓库](#mvn_repository_remote)（除非 `-U` 或本地缺失/损坏）。
* 2.`settings.xml` 中的 `Profile` 仓库
    * 路径：`settings.profiles.profile.repositories.repository`
    * 只有在 `settings.xml` 里激活了 `<activeProfiles>`，对应的 `Profile` 才会生效。
    * 优先级高于 `pom.xml` 里的所有仓库配置。
* 3.`pom.xml` 中的 `Profile` 仓库
    * 路径：`project.profiles.profile.repositories.repository`
    * 必须在 `settings.xml` 中通过 `<activeProfiles>` 激活对应的 `Profile` 才会生效。
    * 优先级低于 `settings.xml` 的 `Profile` 仓库，高于 `pom.xml` 普通仓库。
* 4.`pom.xml` 中的 [普通仓库](#普通仓库)
    * 路径：`project.repositories.repository`
    * 通常用来指定项目专用的仓库，比如公司内部的 Nexus、Artifactory。
* 5.Maven 默认 [中央仓库](#mvn_repository_remote_central) 
    * URL: [https://repo.maven.apache.org/maven2](https://repo.maven.apache.org/maven2)
    * 当以上所有仓库都找不到依赖时，才会去中央仓库。
* 6.[镜像](#mvn_mirror)（Mirror）会改变顺序
    * 如果你在 `settings.xml` 里配置了 `<mirrors>`，那么所有远程仓库的请求都会被重定向到镜像地址。
    * 比如配置了 `Nexus` 作为中央仓库的镜像，所有原本去 [https://repo.maven.apache.org/maven2](https://repo.maven.apache.org/maven2) 的请求，都会被重定向到 Nexus。
    * **即便是 `Profile` 声明的仓库，其在拉取镜像时也会优先匹配是否存在镜像仓库，如果匹配成功，则优先使用镜像仓库**（因为镜像实质就是一个「拦截器」）。

#### 项目级配置多仓库

在 [项目](#mvn_project) 的 `pom` 文件中，配置仓库有两种方式：

##### 普通仓库

`pom.xml` 文件根节点 `<project>` 下，直接使用 `<repositories>` 及 `<repository>` 标签定义的仓库。

每一个 `<repository>` 定义一个仓库，`<repositories>` 可以定义多个 `<repository>`。

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0">
  
  <repositories>
    <repository>
      <id>my-repo1</id>
      <name>your custom repo</name>
      <url>http://jarsm2.dyndns.dk</url>
    </repository>
    <repository>
      <id>my-repo2</id>
      <name>your custom repo</name>
      <url>http://jarsm2.dyndns.dk</url>
    </repository>
  </repositories>
</project>
```

##### profile 仓库

`pom.xml` 文件中，在 `<profile>` 节点中标签定义的仓库，与 [settings 级配置多仓库](#settings%20级配置多仓库) 是一样的。

同样的，也得使用 `<activeProfiles>` 激活才能生效。

#### settings 级配置多仓库

而在 `setttins.xml` 中，是将整个「仓库组」`<repositories>` 放在 `<profile>` 标签中定义的。而为了一个 `<profile>` 定义一个仓库，所以 `<repositories>` 标签中只放一个仓库 `<repository>`。

##### 启用 profile

###### 自动启动

另外，如果想要**自动**启动 `<profile>` 中配置的仓库，还得在 `<activeProfiles>` 标签中配置「激活」它。
> [!info] 
> 
> 想要「激活」l 哪个 Profile，只需在 `<activeProfiles>` 中添加一个 `<activeProfile>` 项，其中的值就是 `<profile>` 中的 `<id>` 的值。
> 
> 示例：
> ```xml
> <activeProfiles>
>	<activeProfile>profi id</activeProfile>
></activeProfiles>
> ```

###### 手动

在使用 [profile 仓库](#profile%20仓库) 时，可以在命令中加入 `-P` 「**手动**」显式指定使用的 profile 仓库，如 `mvn install -P myprofile`，`-P` 的参数值是 `profile` 的 `<id>`，这样就能在不同的 profile 仓库切换。

如不使用 `-P` 指定 profile，如果 `settings.xml` 中有 [自动启动](#自动启动) 的配置，则按 `<activeProfile>` 顺序从上至下依次查询，如果找不到则到其他 Profile 中找。

例如上面的示例中 `jboss-repos` 的 Profile 中没有使用 `<pluginRepositoies>` 配置 [Maven 插件](#mvn_plugins) 仓库，而使用 `jboss-repos` 这个 Profile 时，肯定是找不到的，则 Maven 会非常「智能」地跑到其他 Profile 中查找，如果都没找到，那才会到 [中央仓库](#mvn_repository_remote_central) 查找。

而如果没有配置 [自动启动](#自动启动)，又没用 `-P` 进行显式的指定启用哪个 `profile`，那 Maven 就什么使用默为的 [中央仓库](#mvn_repository_remote_central)。

> [!info] 
> 
> 总体看来，使用 `profile` 及 `<repository>` 配置仓库，比使用 [镜像](#mvn_mirror) 更灵活而「智能」！当然配置起来也相较使用镜像销微复杂一点。

示例：

```xml
<settings>
	<profiles>
		<profile>
		 <id>myprofile</id>
		 <repositories>
		   <repository>
			 <id>my-repo1</id>
			 <name>your custom repo</name>
			 <url>http://jarsm2.dyndns.dk</url>
		   </repository>
		 </repositories>
		</profile>
		
		<profile>
		 <id>myprofile2</id>
		 <repositories>
		   <repository>
			 <id>my-repo2</id>
			 <name>your custom repo2</name>
			 <url>http://jarsm2.dyndns2.dk</url>
		   </repository>
		 </repositories>
		</profile>
		
		
	</profiles>
	
	<activeProfiles>
		<activeProfile>myprofile</activeProfile>
		<activeProfile>myprofile2</activeProfile>
	</activeProfiles>
</settings>
```

如果是 [Maven 插件](#mvn_plugins)，也得单独是使用 `<pluginRepositories>` 及 `<pluginRepository>` 标签来配置。
> [!info] 
> 
> 如果没有使用 `<pluginRepositories>` 及 `<pluginRepository>` 为 Maven 插件配置仓库，即便 `<Reopository>` 中配的仓库有组件，Maven 也不会去查询，它只能跳到别的 Profile 中查询是否配了 `<pluginRepository>`，如果都没配，那就到默认的 [中央仓库](#mvn_repository_remote_central) 中下了。
> 
> 因为 [Maven 插件](#mvn_plugins) 所有 Maven 项目或多或少都会用到，所以建议每个 Profile 中最好还是添加 `<pluginRepositories>` 及 `<pluginRepository>` 的配置。
> 

```xml
<pluginRepositories>
	<pluginRepository>
		<id>aliyun-plugins</id>
		<name>Aliyun Plugin Repository</name>
		<url>https://maven.aliyun.com/repository/public</url>
		<releases>
			<enabled>true</enabled>
		</releases>
		<snapshots>
			<enabled>false</enabled>
		</snapshots>
	</pluginRepository>
</pluginRepositories>
```

```xml
<profile>
	<id>china-repos</id>
	<repositories>
		<repository>
			<id>aliyun-central</id>
			<name>aliyun central</name>
			<url>https://maven.aliyun.com/repository/public</url>
			<releases>
				<enabled>true</enabled>
			</releases>
			<snapshots>
				<enabled>false</enabled>
			</snapshots>
		</repository>
	</repositories>
	
	<pluginRepositories>
		<pluginRepository>
			<id>aliyun-plugins</id>
			<name>Aliyun Plugin Repository</name>
			<url>https://maven.aliyun.com/repository/public</url>
			<releases>
				<enabled>true</enabled>
			</releases>
			<snapshots>
				<enabled>false</enabled>
			</snapshots>
		</pluginRepository>
	</pluginRepositories>
</profile>

<profile>
	<id>jboss-repos</id>
	<repositories>
		<repository>
			<id>jboss-public</id>
			<name>jboss public</name>
			<url>https://repository.jboss.org/nexus/repository/public/</url>
		</repository>
	</repositories>
</profile>
	
</profiles>

<activeProfiles>
	<activeProfile>jboss-repos</activeProfile>
	<activeProfile>china-repos</activeProfile>
</activeProfiles>

```

---

## <span id="mvn_mirror">镜像</span>

「镜像」实质是一个「拦截器」，它会拦截 Maven 对远程仓库的相关为请求，把请求里的远程仓库重新定向到镜像里配置的地址。

> [!info] 
> 
> 同一个 [仓库](#mvn_repository) 的多个「镜像」时，相互之间是备份的关系，只有在仓库连不上的 时候才会切换另一个，而如果在能连上的情况下找不到包，是不会尝试下一个地址的

### mirrorOf

* 当 `mirrorOf` 中设置为 `*` 时,所有请求都会转到 镜像中所指的 `url` 中,此时 `profile` 中配置的 `repositories` 失效,当然 `profile` 中的其他配置还是有效的。
* `mirrorOf` 设置是您正在使用的镜像的 [仓库](#mvn_repository)（`repository`）的 `id`。例如,默认情况下包含的主 Maven[中央仓库](#mvn_repository_remote_central) 的 ID 是 `central`，且 Maven **仅使用匹配到的第一个镜像**，其余符合匹配条件的镜像将不起作用。也就是说，如果你的第一个仓库的 `mirrorOf` 配置为 `*`，则其余镜像配置将不起作用。
* 镜像的 `mirrorOf` 不能和任何镜像的 id 一致，因为 id 在 setting 中唯一，`mirrorOf` 要和 [仓库](#mvn_repository") 的 id 一致

```xml
<mirrors>  
...  
	<mirror>  
		<id>aliyunmaven</id>  
		<mirrorOf>central</mirrorOf>  
		<name>阿里云公共仓库</name>  
		<url>https://maven.aliyun.com/repository/public</url>  
	</mirror>  
</mirrors>
```

> `mirrorOf` 中的必须为 `central`，这样才能作为 [中央仓库](#mvn_repository_remote_central) 的镜像。

语法：`mirrorOf = [匹配模式],![排除模式1],![排除模式2],...`

> [!tip] 
> 
> `mirrorOf` 中除了 `*` 外，匹配模式中的的 `id` 是 [仓库](#mvn_repository)（`repostory`）的 ID，非其他镜像的 ID。
> 
> 镜像与镜像间，是备份关系。

规则说明：

* `*`：匹配所有仓库
* `!repo-id`：排除特定 ID 的仓库
* `external:*`：匹配所有外部仓库
* `*,!repo1,!repo2`：匹配除 repo1 和 repo2 外的所有仓库

> [!info] 
> 
> Maven 按以下顺序处理镜像：
> 
> 1. **精确匹配**：`mirrorOf` 与仓库 ID 完全匹配
> 2. **模式匹配**：使用通配符的模式
> 3. **第一个匹配的镜像生效** - 一旦找到匹配的镜像，就不会继续查找

> [!tip] 
> 
> 最佳实践建议
> 
> 1. **避免镜像冲突**：确保不同镜像的 `mirrorOf` 不会重叠
> 2. **合理排序**：将最具体的镜像放在前面，通用的放在后面
> 3. **排除内部仓库**：公司内部仓库通常不需要镜像
> 4. **测试配置**：使用 `mvn help:effective-settings` 验证配置
>> [!info] 
>> `mvn clean help:effective-settings` 清理 `effective-settings`
>> 
>> `mvn help:effective-settings` 及 `mvn clean help:effective-settings` 命令，应该在 Maven 项目中执行，不然第二次执行 `mvn help:effective-settings` 是没有下载信息，而如果要使用 `mvn clean help:effective-settings` 清理，也会报 `The goal you specified requires a project to execute but there is no POM in this directory` 错误。

|        特性        | `mirrorOf *` | `mirrorOf external:*` |
|:------------------:|:------------:|:---------------------:|
|  **本地文件仓库**  |   ✅ 匹配    |       ❌ 不匹配       |
| **localhost 仓库** |   ✅ 匹配    |       ❌ 不匹配       |
|   **局域网仓库**   |   ✅ 匹配    |        ✅ 匹配        |
|   **互联网仓库**   |   ✅ 匹配    |        ✅ 匹配        |
|    **中央仓库**    |   ✅ 匹配    |        ✅ 匹配        |

### 国内镜像

国内镜像主要是 [阿里](https://developer.aliyun.com/mvn/guide)、[网易](https://mirrors.163.com/.help/maven.html)、[华为](https://mirrors.huaweicloud.com/mirrorDetail/5ea0025f2ab89b484a4dd5ce?mirrorName=maven&catalog=language) 和 [腾讯](https://mirrors.cloud.tencent.com)。

大体以阿里的镜像为主。

阿里仓库列表：[https://maven.aliyun.com/mvn/view](https://maven.aliyun.com/mvn/view)

国内镜像配置如下：

```xml
<mirrors>
	<mirror>
	  <id>maven-default-http-blocker</id>
	  <mirrorOf>external:http:*</mirrorOf>
	  <name>Pseudo repository to mirror external repositories initially using HTTP.</name>
	  <url>http://0.0.0.0/</url>
	  <blocked>true</blocked>
	</mirror>
	
	
	<mirror>
	  <id>aliyun-central</id>
	  <mirrorOf>central</mirrorOf>
	  <name>阿里云公共仓库</name>
	  <url>https://maven.aliyun.com/repository/central</url>
	</mirror>
	
	<mirror>
	  <id>aliyun-public</id>
	  <mirrorOf>external:*,!aliyun-central</mirrorOf>
	  <name>阿里云jcenter和mavenCentral的聚合仓库</name>
	  <url>https://maven.aliyun.com/repository/public</url>
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
	
	<mirror>
	  <id>aliyun-google</id>
	  <mirrorOf>google</mirrorOf>
	  <name>阿里云google仓库</name>
	  <url>https://maven.aliyun.com/repository/google</url>
	</mirror>
	
	
	<mirror>
	  <id>netcn</id>
	  <name>Mirror from Maven in china</name>
	  <url>http://maven.net.cn/content/groups/public/</url>
	  <mirrorOf>external:*,!aliyun-central,!aliyun-public</mirrorOf>
	</mirror>
	
	<mirror>
	  <id>apachemaven</id>
	  <mirrorOf>external:central,!aliyun-central,!aliyun-public,!netcn</mirrorOf>
	  <name>apache repo</name>
	  <url>https://repo.maven.apache.org/maven2/</url>
	</mirror>
	
	<mirror>
	  <id>repomaven</id>
	  <mirrorOf>external:*,!aliyun-central,!aliyun-public,!netcn</mirrorOf>
	  <name>central repo1</name>
	  <url>https://repo1.maven.org/maven2/</url>
	</mirror>

</mirrors>

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
		<activeByDefault>false</activeByDefault> 
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
> `properties` 节点中指定的 JDK 版本是可选配置。
> 
> 另外，`properties` 中 `source` 和 `target` 可以使用 `release` 来替代。
> 
> ```xml
> <properties>
>	<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
>	<maven.compiler.release>21</maven.compiler.release>
></properties>
> ```
> 
> [Maven Repository: maven-compiler-plugin](https://mvnrepository.com/artifact/org.apache.maven.plugins/maven-compiler-plugin)
>
> 真正关键是 maven 插件 `maven-compiler-plugin` 中的 `configuration` 节点也可以配置 JDK 版本，与 `properties` 节点配置可以二选一：
> ```xml
> <configuration>
>	<release>21</release>
>	<encoding>UTF-8</encoding>
></configuration>
> ```
>
> 另外，[Maven](Maven_Note.md) 全局配置文件 `settings.xml` 中 `<activeByDefault>` 这里的配置应该不存在，或值为 `false`，也就是不要设置默认 JDK 版本，不然项目中的设置 JDK 版本会失效。
> 

---

### <span id="mvn_project_create">创建项目</span>

示例：

```shell
mvn archetype:generate -DgroupId=com.silascript.exercise -DartifactId=e02 -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.5
```

> [!tip] 
> 
> 如果在 Windows 下，参数有可能得使用双引号括起来：
> 
> `mvn archetype:generate "-DarchetypeGroupId=org.apache.maven.archetypes" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DarchetypeVersion=1.5"`

#### 命令组成

`mvn`：核心命令

`archetype:generate`：创建项目。`archetype` 其实是一个插件 [`maven-archetype-plugin`](#Archetype%20Plugin)。

##### 项目相关参数

`-DgroupId=com.silascript.exercise`：包名的写法，域名的反写

`-DartifactId=e02`：项目名称

##### Archetype 的指定

[Archetype](#mvn_project_archetype) 的指定包括两块：

* Archtype 的类型指定：[ArchetypeArtifactId](#ArchetypeArtifactId)
* Archetype 的版本：[ArchetypeVersion](#ArchetypeVersion)

#### 创建项目注意事项

创建项目时，项目的根目录的目录名是由 `artifactId` 中指定的项目名称决定的，会在命令所有的当前目录自动创建。

```shell
# silascript @ (base) in ~/DevWorkSpace/JavaExercise [11:14:18] 
$ mvn archetype:generate -DgroupId=com.silascript.exercise -DartifactId=e02 -DarchetypeArtifactId=maven-archetype-quickstart
```

示例：如当前目录是 `~/DevWorkSpace/JavaExercise`，在此目录中使用 `mvn archetype:generate` 命令创建项目，则会在当前目录中创建一个 `e02` 项目录：

```shell
# silascript @ (base) in ~/DevWorkSpace/JavaExercise [11:26:23] 
$ ll
Permissions Size User       Group      Date Modified    Name
drwxr-xr-x     - silascript silascript 2025-10-06 11:17 .
drwxr-xr-x     - silascript silascript 2025-09-24 02:51 ..
drwxr-xr-x     - silascript silascript 2025-10-06 11:17 e02
```

### <span id="mvn_project_archetype">Archetype</span>

`archetype` （翻译为「骨架」或「模板」）是 [创建项目](#mvn_project_create) 时使用的项目模板，这个模板定义了项目的基本架构。

#### Archetype Plugin

Archetype 不是 Maven 的核心，它是通过插件来实现的，这插件就是 [maven-archetype-plugin](https://maven.apache.org/archetype/maven-archetype-plugin)

maven 内置骨架：[Apache Maven Archetypes – Maven Archetypes](https://maven.apache.org/archetypes/index.html)

###### ArchetypeArtifactId

`-DarchetyDpeArtifactId=maven-archetype-quickstart`：表示使用 `maven-archetype-quickstart` 这个 [骨架](#mvn_project_archetype) 来创建项目。
此参数是可选，如果未指定此参数，maven 会输出一个 [骨架](#mvn_project_archetype)（`archetype`）列表供用户选择，默认是选择 `maven-archetype-quickstart` 这个骨架：
```shell
Choose archetype:
1: internal -> org.apache.maven.archetypes:maven-archetype-archetype (An archetype which contains a sample archetype.)
2: internal -> org.apache.maven.archetypes:maven-archetype-j2ee-simple (An archetype which contains a simplifed sample J2EE application.)
3: internal -> org.apache.maven.archetypes:maven-archetype-plugin (An archetype which contains a sample Maven plugin.)
4: internal -> org.apache.maven.archetypes:maven-archetype-plugin-site (An archetype which contains a sample Maven plugin site.
      This archetype can be layered upon an existing Maven plugin project.)
5: internal -> org.apache.maven.archetypes:maven-archetype-portlet (An archetype which contains a sample JSR-268 Portlet.)
6: internal -> org.apache.maven.archetypes:maven-archetype-profiles ()
7: internal -> org.apache.maven.archetypes:maven-archetype-quickstart (An archetype which contains a sample Maven project.)
8: internal -> org.apache.maven.archetypes:maven-archetype-site (An archetype which contains a sample Maven site which demonstrates
      some of the supported document types like APT, XDoc, and FML and demonstrates how
      toDi18n your site. This archetype can be layered upon an existing Maven project.)
9: internal -> org.apache.maven.archetypes:maven-archetype-site-simple (An archetype which contains a sample Maven site.)
10: internal -> org.apache.maven.archetypes:maven-archetype-webapp (An archetype which contains a sample Maven Webapp project.)
Choose a number or apply filter (format: [groupId:]artifactId, case sensitive contains): 7:
```

###### ArchetypeVersion

`-DarchetypeVersion=1.5`：这是指定 [Archetype](#mvn_project_archetype) 的版本。

这也是可选。

如果未指定 [ArchetypeArtifactId](#ArchetypeArtifactId)，将使用最新版本：
```shell
$ mvn archetype:generate -DgroudId=com.sialscript.exercise -DartifactId=e02                                                 
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------< org.apache.maven:standalone-pom >-------------------
[INFO] Building Maven Stub Project (No POM) 1
[INFO] --------------------------------[ pom ]---------------------------------
[INFO] 
[INFO] >>> archetype:3.4.0:generate (default-cli) > generate-sources @ standalone-pom >>>
[INFO] 
[INFO] <<< archetype:3.4.0:generate (default-cli) < generate-sources @ standalone-pom <<<
[INFO] 
[INFO] 
[INFO] --- archetype:3.4.0:generate (default-cli) @ standalone-pom ---
[INFO] Generating project in Interactive mode
[WARNING] failed to download from remoteorg.eclipse.aether.transfer.MetadataNotFoundException: /archetype-catalog.xml was not found in https://maven.aliyun.com/repository/public during a previous attempt. This failure was cached in the local repository and resolution is not be reattempted until the update interval of aliyunmaven has elapsed or updates are forced
[WARNING] No archetype found in remote catalog. Defaulting to internal catalog
[INFO] No archetype defined. Using maven-archetype-quickstart (org.apache.maven.archetypes:maven-archetype-quickstart:1.0)
Choose archetype:
1: internal -> org.apache.maven.archetypes:maven-archetype-archetype (An archetype which contains a sample archetype.)
2: internal -> org.apache.maven.archetypes:maven-archetype-j2ee-simple (An archetype which contains a simplifed sample J2EE application.)
3: internal -> org.apache.maven.archetypes:maven-archetype-plugin (An archetype which contains a sample Maven plugin.)
4: internal -> org.apache.maven.archetypes:maven-archetype-plugin-site (An archetype which contains a sample Maven plugin site.
      This archetype can be layered upon an existing Maven plugin project.)
5: internal -> org.apache.maven.archetypes:maven-archetype-portlet (An archetype which contains a sample JSR-268 Portlet.)
6: internal -> org.apache.maven.archetypes:maven-archetype-profiles ()
7: internal -> org.apache.maven.archetypes:maven-archetype-quickstart (An archetype which contains a sample Maven project.)
8: internal -> org.apache.maven.archetypes:maven-archetype-site (An archetype which contains a sample Maven site which demonstrates
      some of the supported document types like APT, XDoc, and FML and demonstrates how
      to i18n your site. This archetype can be layered upon an existing Maven project.)
9: internal -> org.apache.maven.archetypes:maven-archetype-site-simple (An archetype which contains a sample Maven site.)
10: internal -> org.apache.maven.archetypes:maven-archetype-webapp (An archetype which contains a sample Maven Webapp project.)
Choose a number or apply filter (format: [groupId:]artifactId, case sensitive contains): 7: 
[INFO] Using property: javaCompilerVersion = 17
[INFO] Using property: junitVersion = 5.11.0
Define value for property 'groupId': com.silascript.exercise
[INFO] Using property: artifactId = e02
Define value for property 'version' 1.0-SNAPSHOT: 
Define value for property 'package' com.silascript.exercise: 
Confirm properties configuration:
javaCompilerVersion: 17
junitVersion: 5.11.0
groupId: com.silascript.exercise
artifactId: e02
version: 1.0-SNAPSHOT
package: com.silascript.exercise
 Y: y
[INFO] ----------------------------------------------------------------------------
[INFO] Using following parameters for creating project from Archetype: maven-archetype-quickstart:1.5
```
而如果是通过命令参数 `-DarchetyDpeArtifactId` 显式指定 Archetype，那如果 `DarchetypeVersion` 参数未指定，最终它会使用 archetype 的最旧版本：

```shell
$ mvn archetype:generate -DgroudId=com.sialscript.exercise -DartifactId=e02 -DarchetypeArtifactId=maven-archetype-quickstart
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------< org.apache.maven:standalone-pom >-------------------
[INFO] Building Maven Stub Project (No POM) 1
[INFO] --------------------------------[ pom ]---------------------------------
[INFO] 
[INFO] >>> archetype:3.4.0:generate (default-cli) > generate-sources @ standalone-pom >>>
[INFO] 
[INFO] <<< archetype:3.4.0:generate (default-cli) < generate-sources @ standalone-pom <<<
[INFO] 
[INFO] 
[INFO] --- archetype:3.4.0:generate (default-cli) @ standalone-pom ---
[INFO] Generating project in Interactive mode
[WARNING] failed to download from remoteorg.eclipse.aether.transfer.MetadataNotFoundException: /archetype-catalog.xml was not found in https://maven.aliyun.com/repository/public during a previous attempt. This failure was cached in the local repository and resolution is not be reattempted until the update interval of aliyunmaven has elapsed or updates are forced
[WARNING] No archetype found in remote catalog. Defaulting to internal catalog
[INFO] Artifact org.apache.maven.archetypes:maven-archetype-quickstart:jar:1.0 is present in the local repository, but cached from a remote repository ID that is unavailable in current build context, verifying that is downloadable from [aliyunmaven (https://maven.aliyun.com/repository/public, default, releases)]
[INFO] Artifact org.apache.maven.archetypes:maven-archetype-quickstart:jar:1.0 is present in the local repository, but cached from a remote repository ID that is unavailable in current build context, verifying that is downloadable from [aliyunmaven (https://maven.aliyun.com/repository/public, default, releases)]
Downloading from aliyunmaven: https://maven.aliyun.com/repository/public/org/apache/maven/archetypes/maven-archetype-quickstart/1.0/maven-archetype-quickstart-1.0.jar
Downloaded from aliyunmaven: https://maven.aliyun.com/repository/public/org/apache/maven/archetypes/maven-archetype-quickstart/1.0/maven-archetype-quickstart-1.0.jar (0 B at 0 B/s)
Define value for property 'groupId': com.silascript.exercise
[INFO] Using property: artifactId = e02
Define value for property 'version' 1.0-SNAPSHOT: 
Define value for property 'package' com.silascript.exercise: 
Confirm properties configuration:
groupId: com.silascript.exercise
artifactId: e02
version: 1.0-SNAPSHOT
package: com.silascript.exercise
 Y: y
[INFO] ----------------------------------------------------------------------------
[INFO] Using following parameters for creating project from Old (1.x) Archetype: maven-archetype-quickstart:1.0
```

> [!tip] 
> 
> 综上所述，如果要使用 Archetype 最新版本，要么显示指定 `archetypeVersion`，要么连 `archetypeArtifactId` 也不要指定，不然会使用 Archetype 的最旧版本。

#### Archetype Catalog

当用户不指定 Acchetype 坐标的方式使用 [Archetype Plugin](#Archetype%20Plugin) 时，会输出一个 Archetype 列表供用户选择。而这个列表来自一个 `archetype-catalog.xml` 文件。

`archetype-catalog.xml` 来源：

* internal：内置 Archetype Catalog，包含 58 个 Archetype 信息。
* local：指向用户本地的 Archetype Catalog，位置：`~/.m2/archetype-catalog.xml`，默认此文件是不存在的。
* remote：远程，指向了 Maven[中央仓库](#mvn_repository_remote_central) 的 Archetype Catalog，确切地址：[https://repo.maven.apache.org/maven2/archetype-catalog.xml](https://repo.maven.apache.org/maven2/archetype-catalog.xml)
* file：用户指定本机任何位置的 `archetype-catalog.xml` 文件
* http：用户指定使用 [Http](../../Network/Http_Note.md) 协议远程的 `archetype-catalog.xml` 文件

##### local

 `-DarchetypeCatalog=local`，默认情况，会读取 `~/.m2/archetype-catalog.xml` 文件，但如果设置过 [本地仓库](#mvn_repository_local) 后， 这个 `local` 指的就是「本地仓库」的根目录。

如：`<localRepository>${user.home}/mvn_repository</localRepository>`，本地仓库设在了 `~/mvn_repository` 这个目录，那 `archetype-catalog.xml` 就应该放在此目录中，这样使用 `-DarchetypeCatalog=local` 参数时，才会去读取到此 xml 文件。

这文件有 10m 大小，Archetype 非常的多，有 3000 多个，总有一款适合你：

```shell
3587: local -> za.co.absa.hyperdrive:component-archetype_2.12 (-)
Choose a number or apply filter (format: [groupId:]artifactId, case sensitive contains): 2275: 
```

##### remote

使用 `-DarchetypeCatalog=remote` 指定远程 Archetype Catalog，跟默认情况，即不使用 `-DarchetypeCatalog` 参数来创建目录，是一致的：

```shell
$ mvn archetype:generate -DarchetypeCatalog=remote                       
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------< org.apache.maven:standalone-pom >-------------------
[INFO] Building Maven Stub Project (No POM) 1
[INFO] --------------------------------[ pom ]---------------------------------
[INFO] 
[INFO] >>> archetype:3.4.0:generate (default-cli) > generate-sources @ standalone-pom >>>
[INFO] 
[INFO] <<< archetype:3.4.0:generate (default-cli) < generate-sources @ standalone-pom <<<
[INFO] 
[INFO] 
[INFO] --- archetype:3.4.0:generate (default-cli) @ standalone-pom ---
[INFO] Generating project in Interactive mode
Downloading from aliyunmaven: https://maven.aliyun.com/repository/public/archetype-catalog.xml
[WARNING] failed to download from remoteorg.eclipse.aether.transfer.MetadataNotFoundException: Could not find metadata /archetype-catalog.xml in aliyunmaven (https://maven.aliyun.com/repository/public)
[WARNING] No archetype found in remote catalog. Defaulting to internal catalog
[INFO] No archetype defined. Using maven-archetype-quickstart (org.apache.maven.archetypes:maven-archetype-quickstart:1.0)
Choose archetype:
1: internal -> org.apache.maven.archetypes:maven-archetype-archetype (An archetype which contains a sample archetype.)
2: internal -> org.apache.maven.archetypes:maven-archetype-j2ee-simple (An archetype which contains a simplifed sample J2EE application.)
3: internal -> org.apache.maven.archetypes:maven-archetype-plugin (An archetype which contains a sample Maven plugin.)
4: internal -> org.apache.maven.archetypes:maven-archetype-plugin-site (An archetype which contains a sample Maven plugin site.
      This archetype can be layered upon an existing Maven plugin project.)
5: internal -> org.apache.maven.archetypes:maven-archetype-portlet (An archetype which contains a sample JSR-268 Portlet.)
6: internal -> org.apache.maven.archetypes:maven-archetype-profiles ()
7: internal -> org.apache.maven.archetypes:maven-archetype-quickstart (An archetype which contains a sample Maven project.)
8: internal -> org.apache.maven.archetypes:maven-archetype-site (An archetype which contains a sample Maven site which demonstrates
      some of the supported document types like APT, XDoc, and FML and demonstrates how
      to i18n your site. This archetype can be layered upon an existing Maven project.)
9: internal -> org.apache.maven.archetypes:maven-archetype-site-simple (An archetype which contains a sample Maven site.)
10: internal -> org.apache.maven.archetypes:maven-archetype-webapp (An archetype which contains a sample Maven Webapp project.)
Choose a number or apply filter (format: [groupId:]artifactId, case sensitive contains): 7: 
```

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

## 相关文档

* [Using Mirrors for Repositories – Maven docs](https://maven.apache.org/guides/mini/guide-mirror-settings.html)
* [guide-multiple-repository - Maven docs](https://maven.apache.org/guides/mini/guide-multiple-repositories.html)
* [将红帽软件仓库添加到 Maven\| Red Hat Documentation](https://docs.redhat.com/zh-cn/documentation/red_hat_fuse/7.6/html/installing_on_jboss_eap/add-red-hat-repositories-to-maven)

---

## 相关连接

* [个人Maven settings.xml配置](https://github.com/silascript/LinuxConfigs/blob/master/.m2/settings.xml)

---

## 相关笔记

* [Maven 资料清单](Maven_Material.md)
* [Maven 视频清单](./Maven_Videos.md)

