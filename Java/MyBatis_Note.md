# MyBatis 笔记

---

## 目录

---

## 配置


`pom.xml` 配置：

```xml
<dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.13.2</version>
      <scope>test</scope>
    </dependency>

    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.29</version>
    </dependency>


    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.2.9</version>
    </dependency>

    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.9</version>
    </dependency>
  </dependencies>
```

以下依赖，除了 mybatis 包外，还有 mysql 驱动，及 [Druid](https://github.com/alibaba/druid) 数据链接池。



Mybatis 主配置文件：

```xml
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--引入 数据库链接 properties-->
    <properties resource="./db.properties"></properties>

    <environments default="development">


        <environment id="development">
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="config.MyDruidDataSourceFactory">

                <!-- 驱动可配可不配 druid 会根据 url 自动推断-->
                <!-- <property name="driverClassName" value="${db.driver}" /> -->
                <property name="url" value="${db.url}" />
                <property name="username" value="${db.user}" />
                <property name="password" value="${db.password}" />
                <property name="minIdle" value="1" />
                <property name="maxActive" value="10" />
            </dataSource>

        </environment>


        <environment id="development2">
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="POOLED">

                <!-- 驱动可配可不配 druid 会根据 url 自动推断-->
                <property name="driver" value="${db.driver}" />
                <property name="url" value="${db.url}" />
                <property name="username" value="${db.user}" />
                <property name="password" value="${db.password}" />
                <!-- <property name="minIdle" value="3" />
                <property name="maxActive" value="10" /> -->
            </dataSource>

        </environment>


    </environments>

    <mappers>

        <mapper resource="./mapper/StudentDaoMapper.xml" />
        <!-- <package name="Dao" /> -->
    </mappers>


</configuration>
```

主配置文件配了俩数据池，一个是阿里的 Druid，另一个是内置的，可以随时切换。




如果直接将 `<datasource>` 的 `type` 属性设置为 `com.alibaba.druid.pool.DruidDataSource`，会报以下错误： 
```
class com.alibaba.druid.pool.DruidDataSource cannot be cast to class org.apache.ibatis.datasource.DataSourceFactory
```

所以得自行写一个 `org.apache.ibatis.datasource.DataSourceFactory` 接口的实现类，然后设置到 `type` 属性，这样 druid 链接池才能用。

示例：
```java
package config;

import java.util.Properties;

import javax.sql.DataSource;

import com.alibaba.druid.pool.DruidDataSourceFactory;

import org.apache.ibatis.datasource.DataSourceFactory;

public class MyDruidDataSourceFactory implements DataSourceFactory {

    private DataSource dataSource;

    @Override
    public DataSource getDataSource() {

        return this.dataSource;
    }

    @Override
    public void setProperties(Properties properties) {

        try {
            this.dataSource = DruidDataSourceFactory.createDataSource(properties);
        } catch (Exception e) {
            e.printStackTrace();
        }

    }

}
```



Druid 具体使用请参考：[druid wiki](https://github.com/alibaba/druid/wiki)

---





