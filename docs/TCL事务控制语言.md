# [主页](../README.md)>[SQL语言](SQL语言.md)>TCL事务控制语言.md

## 事务

```SQL
set session/global transaction isolation level read uncommitted/read committed/repeatable read/serializable;
start transaction;
commit/rollback;
```

>事务是保证数据完整和安全的，事务本质是保证多条DML语句同时完成或失败  
>提交事务（commit）标志着所有DML语句执行成功；回滚事务（rollback）标志着撤销所有DML语句  
>事务具有原子性Atomicity（事务是不可再分的最小单元）；一致性Consistency（事务必须同时成功或失败，数据是完整和一致的）；隔离性Isolation（不同事务之间有一定的相互隔离）：持久性Durability（事务被提交或回滚后便不可更改）

|事务隔离级别|脏读|不可重复读|幻读|说明|
|--|:--:|:--:|:--:|--|
|读未提交（read-uncommitted）|是|是|是|事务A可以读取事务B未提交的数据|
|不可重复读（read-committed）|否|是|是|事务A只能读取事务B已提交的数据|
|可重复读（repeatable-read）|否|否|是|事务A开启后读取的数据是开启事务时的数据|
|串行化（serializable）|否|否|否|事务A开启后其它事务只能排队等待|
