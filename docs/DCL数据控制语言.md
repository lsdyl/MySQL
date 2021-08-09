# [主页](../README.md)>DCL数据控制语言

## 数据导出

```SQL
mysqldump 数据库名 表名>路径 -u 用户名 -p
mysqldump employee>d:\employee.sql -u root -p
```

> 使用导mysqldump 数据库名 [表名]>路径 employee.sql -u root -p  

## 数据导入

```SQL
create datebase employee;
use employee;
source d:\employee.sql;
```
