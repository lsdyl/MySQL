# [主页](../README.md)>SQL语言

## SQL语句分类

- [DQL数据查询语言](DQL数据查询语言.md)，如select
- [DML数据操作语言](DML数据操作语言.md)，如insert,delete,update
- [DDL数据定义语言](DDL数据定义语言.md)，如create,drop,alter
- [TCL事务控制语言](TCL事务控制语言.md)，如commit,rollback
- [DCL数据控制语言](DCL数据控制语言.md)，grant,revoke

## 索引

```SQL
create index 索引名 on 表名(字段名,);
drop index 索引名 on 表名(字段名);
create index deptno_emp_index on emp(deptno);
explain select * from emp where deptno='10';
```

> 主键字段会自动添加索引，在MYSQL中有uniqe约束的字段也会自动添加索引  
> 索引的原理是对索引字段实现了B-TREE结构，从而加快了字据读取速度  
> 添加索引的条件：数据量庞大；该字段经常出面在where之后；该字段很少进行DML操作;  
> 可以通过explain语句查看SQL查询是否使用了索引

索引失效：

- 当使用模糊查询，且查询条件以“%”开头
- 当使用的“or”且两边的字段不是同时有索引（可使用union避免）
- 使用复合索引时，没有使用左侧的字段查找
- 索引字段参加了运算
- 索引字段使用了函数

## 视图

```SQL
create view 视图名 as DQL查询语句;
drop view 视图名;
create view stu_view as select * from stu;
update 视图名 set ;
insert into stu values('1000','张三','男',12,null);
insert into stu values('1001','linda','女',12,null);
insert into stu values('1010','jack','男',19,null);
insert into stu values('1015','mary','女',19,null);
insert into stu values('1017','susan','女',19,null);
insert into stu values('1019','maria','女',19,null);
insert into stu values('1020','ruth','女',19,null);
insert into stu values('2020','amy','女',19,null);
```

> 视图的作用是简化SQL,相当于给一条SQL语句创建了一个查询结果的表  
> 视图数据和原表数据总是同步更改  

## 数据库设计范式

1. 任何一个表必须有主键，每个字段都具有原子性
2. 所有非主键字段必须完全依赖主键，不能产生部分依赖
3. 所有非主键字段直接依赖主键，不产生传递依赖

> 多对多：三张表，2个外键  
> 一对多：两张表，1个外键（多的表加）
> 一对一：拆分两张表，外键+unique

