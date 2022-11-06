# Tomcat 笔记

---

## 目录
* [配置](#tomcat_config)
	* [配端口](#tomcat_config_port)


---


## <span id="tomcat_config">配置</span>

tomcat的配置文件都放在`conf`目录下。

### <span id="tomcat_config_port">配端口</span>
使用文本编辑器打开该目录下的`server.xml`文件，最开始的 `port`，就是 tomcat 的端口：
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
