# Spring 笔记 1

## Spring jar包及maven相关

使用maven时，引入Spring依赖时，不同的包有其不同的依赖，  这应该注意，以免重复引用。

**Spring Context 包的依赖：**
* spring-core
* spring-beans
* spring-aop
* spring-express

**Spring Context Support 包的依赖：**
* spring-core
* spring-beans
* spring-context


**Spring Beans 包的依赖：**
* spring-core

**Spring Core 包的依赖：**
* spring-jcl

**Spring AOP 包的依赖：**
* spring-core
* spring-beans

**Sping Web 包的依赖：**
* spring-core
* spring-beans

**Spring Web MVC 包的依赖：**
* spring-core
* spring-context
* spring-beans
* spring-aop
* spring-expression
* spring-web


**Spring JDBC 包的依赖：**
* spring-core
* spring-beans
* spring-tx

**Spring TX 包的依赖：**
* spring-core
* spring-beans


![spring 包依赖结构1](./Spring_Note_1.assets/sping_1.png)

![spring 包依赖结构2](./Spring_Note_1.assets/spring_2.png)

从上面的包依赖，可以看出，spring各组件基础依赖都是core、beans和context，而**Spring Context** 包又包含了core和beans，所以如果做Spring非web应用时，只用在maven的pom文件中添加**Spring Context**的包就可以了。  

如果使用web就多加个web包，如果是使用**Spring MVC**框架，那因为**Spring MVC** 包已经包含了**Spring Context**及其依赖包，所以maven的pom文件只用添加一个Spring MVC依赖就可以。


