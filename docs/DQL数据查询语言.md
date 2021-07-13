# [主页](../README.md)>[SQL语言](SQL语言.md)>DQL数据查询语言

```SQL
select
from 
where
group by
order by
以上语句顺序固定，执行顺序为 from;where;group by;select;order by;
```
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
select concat(lower(substr(ename,1,1)),substr(ename,2)) from emp;
select (ifnull(sal,0)+ifnull(comm,0))*12 from emp;
select ename,job,sal as 原工资,case job when 'clerk' then sal*2 when 'salesman' then sal*4 else 0 end as 新工资 from emp;
```
- lower 将所有大写字母转换为小写字母；upper 将小写字母转大写
- substr([字段1],开始位置，长度) 截取字段的某部分值，下标从1开始
- concat([字符1],字段2,) 将多个字符连接成一个字符串
- length([字段1]) 获取字段的长度
- trim([字段]) 去掉字符前后的空白
- str_to_date() 将日期字符串转为日期
- date_format() 日期格式化
- format() 数字格式化
- round([字段1],需要保留的小数位) 四舍五入
- rand() 生成0-1之间的随机数
- ifnull([字段1],如果是null则) 空处理，在数据库中NULL运算的结果必定是NULL
- case[字段1] when[条件1] then[处理1]  when[条件2] then[处理2] else [处理3] end 字段的条件分支处理
## 分组函数
```SQL
select max(sal) as 最高工资,min(sal) as 最低工资,max(sal)-min(sal) as 工资差 from emp;
```
- count([字段1]) 对不为NULL的字段计数，count(*) 计算表的行数
- sum([字段1]) 求和
- avg([字段1]) 求平均数
- max([字段1]) 求最大值
- min([字段1]) 求最小值

>分组函数必须先进行分组之后才能使用，如果没有分组，那么表默认为一组  
>where后不能直接使用分组函数，因为分组函数执行在where之后  
>分组函数自动忽略NULL，即NULL值不参数计算，如avg中的NULL不统计个数
## 分组查询
```SQL
select job as 岗位,avg(mgr) as 岗位平均工资 from emp group by job;
select job as 岗位,max(mgr) as 岗位最高工资 from emp group by job;
```
>当不存在group by时，默认将表分为一组  
>当使用分组语句时，select后只能存在参加分组的字段，否则没有意义
>