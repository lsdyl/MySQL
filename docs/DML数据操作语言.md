# [主页](../README.md)>DML数据操作语言

## 增

```SQL
insert into NAME(字段名1,字段名2...) values(值1,值2...),(多条记录);
insert into stu(no,name,sex,age,email) values('1000000001','张三','男',18,'9739887425@163.com');
insert into user(no,name,sex,age,email,create_time) values('1000000001','张三','男',18,null,'1991-1-2 9:9:9');
insert into user(no,name,sex,age,email,create_time) values('1000000001','liusss','男',18,'452255@qq.com',now());
insert into user_ select * from user; 将查询结果增加到表中
```

>字段和值必须一一对应，当不指定某字段时，值是默认值  
>日期类型可使用函数转换，或使用默认格式系统会自动转换

## 删

```SQL
delete from NAME where ;
```

>where条件范围非常重要，没有where条件将导致表中所有记录被删除  
>此方法删除数据较慢，但方便回滚

## 改

```SQL
update NAME set 字段名1=值,字段名2=值... where ;
update user set age=99 where name='张三';
```

>where条件范围非常重要，没有where条件将导致该字段下所有值被修改  
