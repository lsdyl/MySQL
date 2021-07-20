# [主页](../README.md)>[SQL语言](SQL语言.md)>DQL数据查询语言

```SQL
select
from 
where
group by
having 
order by
limit 
以上语句顺序固定，执行顺序为 from;where;group by; having;select;order by;limit;
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
- and 逻辑与；or 逻辑或；
- [not]in [不]包含，相当于多个or;and优先级高于or
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
- TIMESTAMPDIFF(interval,dt1,dt2) 计算两个日期时间的整数差，间隔为：YEAR年；QUARTER季度；MONTH月；WEEK周；DAY天；HOUR时；MINUTE分；SECOND秒；FRAC_SECOND毫秒；
- TIMESTAMPADD(interval,int,dt1) 计算指定日期相加某间隔时间，间隔同上
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
select job,deptno,avg(sal) as 各部门各岗位平均工资 from emp group by deptno,job;
select deptno as 部门,avg(sal) 平均工资大于2000 from emp group by deptno having avg(sal)>2000;
select job as 岗位,avg(sal) as 岗位不为manager且平均工资大于1500 from emp where job<>'manager' group by job having avg(sal)>1500 order by 岗位不为manager且平均工资大于1500 desc;
```
>当不存在group by时，默认将表分为一组  
>当使用分组语句时，select后只能存在参加分组的字段或分组函数，否则没有意义  
>having子句必须和group by一起使用，用于对已分组字段进行过滤
## 关键字
```SQL
select distinct job,deptno from emp;
```
- distinct 去除某些重复记录，用在所有字段前
## 连接查询
```SQL
select e.ename,d.dname from emp e,dept d where e.deptno=d.deptno;   SQL92语言，不推荐使用
select e.ename,d.dname from emp e inner join dept d on e.deptno=d.deptno; SQL99语法
select e.ename,e.sal,s.grade from emp e inner join salgrade s on e.sal between s.losal and s.hisal;
select e.empno,e.ename,m.ename from emp e inner join emp m on e.mgr=m.empno;
select e.ename,e.deptno,d.dname from emp e right outer join dept d on e.deptno=d.deptno;
select e.ename,e.deptno,d.dname from emp e left outer join dept d on e.deptno=d.deptno;
select e.ename as 姓名,e.sal 薪资,d.dname as 部门,s.grade 薪资等级,ee.ename as 上级 from emp e  join dept d on e.deptno=d.deptno join salgrade s on e.sal between s.losal and s.hisal left join emp ee on e.mgr=ee.empno;
```
- 内连接之等值连接，inner join on 的条件是=
- 内连接之非等值连接，inner join on 的条件不是=
- 内连接之自连接，inner join的表是同一个表，查询时使用不同的别名来确定条件
- 右外连接，right join，首先保证右表的数据全部显示，再匹配左表中条件成立的记录，不匹配则显示为NULL
- 左外连接，left join，首先保证左表的数据全部显示，再匹配右表中条件成立的记录，不匹配则显示为NULL

>表和表进行连接查询时，查询次数会呈现笛卡尔积现象，因此，应尽量减少表的数量和记录数量  
>内连接保证条件成立的记录全部查询出来，其余记录不查询  
>外连接的表分主次关系，不仅仅查询条件匹配的记录  
>内、外连接中的inner和outer可以省略  
>多表查询可以多次使用join on
## 嵌套查询
```SQL
select ename,sal as 工资非最低名单 from emp where sal>(select min(sal) from emp) order by sal;
select e.job,e.avgsal as 岗位平均工资,s.grade 等级 from (select job,avg(sal) as avgsal from emp group by job) e join salgrade s on e.avgsal between s.losal and s.hisal;
```
>select查询结果可以被当作一张临时表格，也可用分组函数查询为某个值
## 合并查询结果
```SQL
select ename,job from emp where job='manager' union select ename,job from emp where job is not NULL;
```
>合并两个查询的结果集，两个结果集必须保证列数和列的数据类型相同  
>合并的结果自动去除重复记录  
>使用union all 会合并所有记录，且效率较高

## 分页查询
```SQL
select * from emp order by sal desc limit 6;
select * from emp order by sal desc limit 1,5;
select e.ename,e.sal,e.deptno from emp e left join (select deptno,max(sal) as maxsal from emp group by deptno) s on e.deptno=s.deptno where e.sal>=s.maxsal;
```
>limit 开始下标(0开始)，前记录条数  
>常用于分页，如每页查询n条记录，则第i页查询的参数为 (i-1)*n,n

## 练习题(自己写的，非答案)
1. `select e.ename,e.sal,e.deptno from emp e left join (select deptno,max(sal) as maxsal from emp group by deptno) s on e.deptno=s.deptno where e.sal>=s.maxsal;`
2. `select e.ename,e.sal from emp e left join (select deptno,avg(sal) as avgsal from emp group by deptno) s  on e.deptno=s.deptno where e.sal>s.avgsal;`
3. `select e.deptno,avg(g.grade) as avggrade from emp e left join  salgrade g on  e.sal between g.losal and g.hisal  group by e.deptno;`
4. `select sal from emp order by sal desc limit 1;  `
5. `select deptno from emp group by deptno order by avg(sal) desc limit 1;`
6. `select dname from dept where deptno=(select deptno from emp group by deptno order by avg(sal) desc limit 1);`
7. `select a.dname,min(a.grade) from (select e.deptno,g.grade,d.dname from (select deptno,avg(sal) as avgsal from emp group by deptno) e join salgrade g on e.avgsal between g.losal and g.hisal join dept d on e.deptno=d.deptno) a;`
8. `select a.ename,a.sal from emp a where a.sal>(select max(e.sal) as maxsal from emp e where empno not in(select distinct mgr from emp where mgr is not null));`
9. `select e.ename,e.sal from emp e order by e.sal desc limit 5;`
10. `select e.ename,e.sal from emp e order by e.sal desc limit 5,5;`
11. `select ename,hiredate from emp order by hiredate desc limit 5;`
12. `select g.grade,count(*) num from emp e left join salgrade g on e.sal between g.losal and g.hisal group by(g.grade);`
13. ``
14. `select e.ename,ifnull(m.ename,'没有上级') from emp e left join emp m on e.mgr=m.empno;`
15. `select e.empno,e.ename,d.dname from emp e left join emp m on e.mgr=m.empno join dept d on e.deptno=d.deptno where e.hiredate<m.hiredate;`
16. `select d.dname,e.ename,e.job,e.mgr,e.hiredate,e.sal,e.comm,e.deptno from emp e right join dept d on e.deptno=d.deptno;`
17. `select d.dname,count(*) as 人数 from emp e join dept d on e.deptno=d.deptno  group by d.deptno having 人数>=5;`
18. `select ename,sal from emp where sal>(select sal from emp where ename='smith');`
19. `select a.*,b.* from (select e.ename,d.dname from emp e join dept d on e.deptno=d.deptno where e.job='clerk') a join (select d.dname,count(*) as count from emp e join dept d on e.deptno=d.deptno group by d.dname) b on a.dname=b.dname;`
20. `select job,count(*) from emp group by job having min(sal)>1500;`
21. `select ename from emp where deptno=(select deptno from dept where dname='sales');`
22. `select e.ename,d.dname,m.ename,g.grade from emp e join dept d on e.deptno=d.deptno left join emp m on e.mgr=m.empno join salgrade g on e.sal between g.losal and g.hisal where e.sal>(select avg(sal) from emp);`
23. `select e.ename,d.dname from emp e join dept d on e.deptno=d.deptno where job=(select job from emp where ename='scott');`
24. `select ename,sal from emp where sal in (select sal from emp where deptno='30' ) and deptno<>'30';`
25. `select ename,sal from emp where sal>(select max(sal) from emp where deptno='30');`
26. `select d.dname,count(e.ename),avg(e.sal),avg(timestampdiff(year,hiredate,now())) from emp e right join dept d on e.deptno=d.deptno group by d.dname;`
27. `select e.ename,d.dname,e.sal from emp e left join dept d on e.deptno=d.deptno;`
28. `select d.*,count(e.ename) from dept d left join emp e on d.deptno=e.deptno group by d.deptno; `
29. `select * from emp e join (select job,min(sal) as minsal from emp group by job ) a on e.job=a.job and e.sal=a.minsal;`
30. `select *,min(sal) from emp where job='manager' group by deptno ;`
31. `select ename,(sal+ifnull(comm,0))*12 as 年薪 from emp order by 年薪 asc;`
32. `select e.ename,a.ename as ld,a.sal from emp e join emp a on e.mgr=a.empno where a.sal>3000;`
33. `select d.dname,sum(e.sal),count(e.ename) from emp e right join dept d on e.deptno=d.deptno where d.dname like '%s%' group by d.dname; `
34. `select *,timestampdiff(year,hiredate,now()) as 工龄,sal*1.1 加薪后 from emp where timestampdiff(year,hiredate,now())>30; `










```