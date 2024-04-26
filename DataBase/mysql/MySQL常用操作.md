
# MySQL常用操作
---

## 目录

---

## [数据库相关命令](#mysql_database_command )

---

## <span id="mysql_database_command">数据库相关命令</span> 

1. 显示数据库
```shell
show databases;
```

2. 显示当前数据库 
```shell
select database();
```

3. 查看数据库使用端口
```shell
show variables like 'port';
```

4. 查看数据库编码
```shell
show variables like 'character%';

show variables like 'collation%';

status;
```

5. 查看当前数据库所有表
```shell
show tables;
```

6. 查看数据文件存放路径
```shell
show variables like '%datadir%';
```
7. 查看 MySQL 状态
```shell
status
```
![mysql_status](MySQL常用操作.assets/mysql_status.png)


8. 查看 MySQL 版本号
	* `select version();`
	> 还有上面的 `status` 命令也能看到 MySQL 的版本号信息




---

MySQL具体介绍请参考：[MySQL笔记](MySQL_Note.md)
