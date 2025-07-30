---
aliases: []
tags:
  - java
  - tomcat
  - apache
  - servlet
  - jsp
  - jdk
  - web
  - javaweb
  - container
created: 2023-08-18 19:44:52
modified: 2025-07-30 21:40:07
---

# Tomcat 笔记

---

## 目录
* [配置](#tomcat_config)
	* [配端口](#tomcat_config_port)

---

## <span id="tomcat_versions">Tomcat 版本</span>

### 兼容性

Tomcat 版本与 Java 版本、Servlet 版本、JSP 版本等对应关系：[wichversion](https://tomcat.apache.org/whichversion.html) 。

支持 [JDK8](../Java_Note.md#JDK8) 最后一个版本是 `10.0.*` 版本。

从 `10.1.*` 版本是要求 [JDK11](../Java_Note.md#JDK11)，而 Tomcat 11.0 版本要求 [JDK17](../Java_Note.md#JDK17)。 

> [!info] 
> 
> 版本相关资料：[版本](Tomcat_Material.md#版本)

---

## <span id="tomcat_config">配置</span>

tomcat 的配置文件都放在 `conf` 目录下。

### <span id="tomcat_config_port">配端口</span>
使用文本编辑器打开该目录下的 `server.xml` 文件，最开始的 `port`，就是 tomcat 的端口：
```xml
<Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```

### <span id="tomcat_config_user">用户配置</span>

`tomcat-user.xml` 文件。

注释中已经说明了 tomcat 内置了几个管理员角色：
```xml
	manager-gui    - allows access to the HTML GUI and the status pages
    manager-script - allows access to the HTTP API and the status pages
    manager-jmx    - allows access to the JMX proxy and the status pages
    manager-status - allows access to the status pages only
```

一般使用 `manager-gui` 这个就好了！

示例：
```xml
<user username="manager" password="123456" roles="manager-gui"/>
```

---

## 启动

tomcat 三种启动方式：

> [!tip] 
> 
> 使用 `bin` 目录下的各脚本启动，需要注意的是，权限问题。如果没有「执行」权限，需要自行修改权限。
> 
> 如直接把 `bin` 目录下所有文件都加上可执行权限：`chmod 755 -R bin`

### 直接启动

直接执行 `bin` 目录中的 `startup.sh`（如果是 Windows 则是 `startup.bat`）。

这种启动方式，在终端中没有任何响应信息，鬼知道启动是否成功，得访问页面时才知道。

其实可以查看 `startup.sh` 脚本，它实质最终是这样的：`catalina.sh start "$@"`，调了 [catalina](#catalina) 脚本来启动。

### catalina

#### 前台启动

通过执行 `bin` 目录下的 `catalina.sh` 脚本，并给一个参数 `run` 来启动：`./catalina.sh run`。

这种启动，可以有终端中显示启动信息。

> [!info] 
> 
> 只要看到 `11-Mar-2025 06:25:52.662 信息 [main] org.apache.catalina.startup.Catalina.start [1132]毫秒后服务器启动`，就证明 Tomcat 启动成功了！

此种方式启动，属于「前台」启动，所以终端会被「阻塞」，当前终端窗口就不能执行其他命令了。而停止 Tomcat，也相对「粗暴」，就是使用 `Ctrl+c` 快捷键。

#### 后台启动

如果想要在后台启动 Tomcat，则可以使用 `catalina.sh start` 命令。
> [!tip] 
> 
> 类似的可以执行 `catalina.sh jpda start`，以 JPDA 调试方式来启动 Tomcat。

执行完此命令，终端不会被「阻塞」，可以在终端使用其他命令。

要停止 Tomcat 运行可以使用：`catalina.sh stop` 命令。

### 服务启动

执行：`nohup ./startup.sh &` ，Tomcat 将以后台服务的方式启动。

---

## 目录结构

### lib 目录

```shell
$ ll .sdkman/candidates/tomcat/current/lib 
Permissions Size User       Group      Date Modified    Name
drwxr-xr-x     - silascript silascript 2025-02-28 15:54 .
drwxr-xr-x     - silascript silascript 2025-03-11 20:47 ..
.rw-r--r--   14k silascript silascript 2025-02-28 15:54 annotations-api.jar
.rw-r--r--   56k silascript silascript 2025-02-28 15:54 catalina-ant.jar
.rw-r--r--  127k silascript silascript 2025-02-28 15:54 catalina-ha.jar
.rw-r--r--   65k silascript silascript 2025-02-28 15:54 catalina-ssi.jar
.rw-r--r--   81k silascript silascript 2025-02-28 15:54 catalina-storeconfig.jar
.rw-r--r--  326k silascript silascript 2025-02-28 15:54 catalina-tribes.jar
.rw-r--r--  1.7M silascript silascript 2025-02-28 15:54 catalina.jar
.rw-r--r--  3.3M silascript silascript 2025-02-28 15:54 ecj-4.34.jar
.rw-r--r--   97k silascript silascript 2025-02-28 15:54 el-api.jar
.rw-r--r--  1.3M silascript silascript 2025-02-28 15:54 jakartaee-migration-1.0.9-shaded.jar
.rw-r--r--  178k silascript silascript 2025-02-28 15:54 jasper-el.jar
.rw-r--r--  556k silascript silascript 2025-02-28 15:54 jasper.jar
.rw-r--r--   28k silascript silascript 2025-02-28 15:54 jaspic-api.jar
.rw-r--r--   73k silascript silascript 2025-02-28 15:54 jsp-api.jar
.rw-r--r--  407k silascript silascript 2025-02-28 15:54 servlet-api.jar
.rw-r--r--   12k silascript silascript 2025-02-28 15:54 tomcat-api.jar
.rw-r--r--  307k silascript silascript 2025-02-28 15:54 tomcat-coyote-ffm.jar
.rw-r--r--  929k silascript silascript 2025-02-28 15:54 tomcat-coyote.jar
.rw-r--r--  342k silascript silascript 2025-02-28 15:54 tomcat-dbcp.jar
.rw-r--r--   70k silascript silascript 2025-02-28 15:54 tomcat-i18n-cs.jar
.rw-r--r--   79k silascript silascript 2025-02-28 15:54 tomcat-i18n-de.jar
.rw-r--r--  107k silascript silascript 2025-02-28 15:54 tomcat-i18n-es.jar
.rw-r--r--  183k silascript silascript 2025-02-28 15:54 tomcat-i18n-fr.jar
.rw-r--r--  208k silascript silascript 2025-02-28 15:54 tomcat-i18n-ja.jar
.rw-r--r--  193k silascript silascript 2025-02-28 15:54 tomcat-i18n-ko.jar
.rw-r--r--   53k silascript silascript 2025-02-28 15:54 tomcat-i18n-pt-BR.jar
.rw-r--r--   68k silascript silascript 2025-02-28 15:54 tomcat-i18n-ru.jar
.rw-r--r--  180k silascript silascript 2025-02-28 15:54 tomcat-i18n-zh-CN.jar
.rw-r--r--  153k silascript silascript 2025-02-28 15:54 tomcat-jdbc.jar
.rw-r--r--   16k silascript silascript 2025-02-28 15:54 tomcat-jni.jar
.rw-r--r--  231k silascript silascript 2025-02-28 15:54 tomcat-util-scan.jar
.rw-r--r--  205k silascript silascript 2025-02-28 15:54 tomcat-util.jar
.rw-r--r--  244k silascript silascript 2025-02-28 15:54 tomcat-websocket.jar
.rw-r--r--   14k silascript silascript 2025-02-28 15:54 websocket-api.jar
.rw-r--r--   32k silascript silascript 2025-02-28 15:54 websocket-client-api.jar

```

---

## 相关链接

* [Tomcat中文网](https://tomcat.github.net.cn)

---

## 相关笔记

* [Tomcat 资料清单](Tomcat_Material.md) 
* [Java Web 笔记](../Java_Servlet_Note.md)
* [Java 笔记](../Java_Note.md)
* [Eclipse 笔记](../IDE/Java_IDE_Eclipse.md)

