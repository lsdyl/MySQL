# [主页](../README.md)>[SQL语言](SQL语言.md)>DDL数据定义语言

## 创建表
```SQL
create table NAME(字段名1 数据类型 default 值,字段名2 数据类型...);
create table stu(no char(10),name varchar(32),sex char(1) default '男',age int,email varchar(255));
create table user(no char(10) not null,name varchar(32),sex char(1) default '男',age int,email varchar(255),create_time datetime default now());
drop table users;
create table user_ as select * from user; 将查询结果新建为一张表，并复制记录
```
>default '值'，为某个字段指定默认值，不指定则为NULL，可使用函数  
|数据类型|范围|
|----|----|
|varchar(n)|可变长度字符串（最大长度255），效率比char低|
|char(n)|指定长度为n字符串（最大长度255）|
|int|整型（最大长度11）|
|bigint|长整型|
|float|单精度浮点型|
|double|双精度浮点型|
|date|短日期，年-月-日|
|datetime|长日期 年-月-日 时:分:秒|
|clob|字符大对象，最多存4G字符串|
|blob|用于存储常见流媒体对象|

## 删除表
```SQL
drop table [if exists] NAME; 删除表
truncate table NAME;删除表中所有记录，无法回滚
```
## 修改表结构
```SQL
表一旦完成创建，便不会轻易修改，而且通常不会使用命令来修改表结构
可以使用类似HeidiSQL的MySQL客户端来管理修改
```
## 约束
```SQL
drop table if exists stu;
create table stu(no char(10) primary key,name varchar(32),sex char(1) default '男',age int,email varchar(255));
create table stu(no int primary key auto_increment,name varchar(32),sex char(1) default '男',age int,email varchar(255));

drop table if exists stu;
drop table if exists class;
create table class(class_no int primary key,name varchar(32) not null);
create table stu(id int primary key auto_increment,name varchar(32),sex char(1) not null,age int not null,class_no int not null,foreign key(class_no) references class(class_no));
insert into class values(101,'一年级一班'),(102,'一年级二班'),(302,'三年级二班');
insert into stu(name,sex,age,class_no) values('张三','男',22,302),('李四','男',14,101),('王五','女',16,102);
select * from stu;

```
- not null 指定字段不允许为空
- unique 字段必须唯一，但可空;unique(字段1,字段2)表示将两个字段联合后唯一
- primary key 主键，是每条记录的唯一标识，不能为空，不能为NULL，且只能存在一个主键，通常使用auto_increment来自动生成主键，任何一张表都必须有主键;primary key(字段1,字段2)表示复合主键（不建议使用）
- foreign key 外键，用来约束字段值，该字段的值只能包含在外键字段中，且引用的字段必须有唯一约束
>新增父表，即被外键引用的表；新增子表，即需要外键的表；  
>外键可以为NULL