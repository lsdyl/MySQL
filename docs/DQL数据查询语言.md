# # [主页](../README.md)>[常用命令](命令.md)>DQL数据查询语言

## 简单查询
```SQL
select [字段名1] as ['别名'],[字段名2] from [表名];
select ename as 姓名,sal*12 as 年薪 from emp;
```
- 使用select * from table 会消耗性能，as关键字可以省略
- 字段可以使用运算符，如+，-，*，/
- 中文字段可能需要使用单引号括起
## 条件查询
```SQL
select [字段名1] ,[字段名2] from [表名] where [字段名][条件][值];
select ename,sal from emp where sal between 3000 and 5000;
select ename,sal from emp where sal in (800,3000);
select ename from emp where ename like 'A%';
```
- \> 大于；< 小于；= 等于；<> 不等于；>= 大于或等于；<=小于或等于
- between [值1] and [值2] 表示两者或之间的值，相当于[值1]>= and <=[值2]
- is [not] null 表示[非]空值(null)，数据库的NULL不能使用=null查询
- and 逻辑与；or 逻辑或；[not]in [不]包含，相当于多个or;and优先级高于or
- like 模糊查询，%表示任意多个字符，_表示任意一个字符（可使用\进行转义）
## 结果排序
```SQL
select [字段名1] from [表名] order by [字段1] [desc/asc],[字段2] asc;
select sal from emp order by sal asc;
```
- order by 根据指定字段排序，默认升序（asc）
- 多个条件同时存在时，优先使用靠前的条件，只有前一个字段相同才会使用后一个条件
- 排序总在SQL语句最后出现
## 单行处理函数
```SQL
select lower([字段名1]) ,[字段名2] from [表名];
select ename from emp where substr(ename,1,1)='a';
```
- lower 将所有大写字母转换为小写字母；upper 将小写字母转大写
- substr([字段],开始位置，长度) 截取字段的某部分值，下标从1开始
