# TCL 事务控制语言
>### 事务
    事务由单独单元的一个或多个sql语句组成，在这个单元中，每个mysql语句是相互依赖的。而整个单独单元作为一个不可分割的整体，如果单元中的某条sql语句一旦执行失败或者产生错误，整个单元将会回滚。所有受到影响的数据将返回到事物开始以前的状态；如果都执行成功，则事物被顺利执行
## 存储引擎
```php
//通过show engines;来查看mysql支持的存储引擎
在mysql中用的最多的存储引擎有：innodb，myisam，memory等，其中innodb支持事务，myisam、memory等不支持事务
```
## 事务的ACID（acid）属性
### 1. 原子性（Atomicity）
`原子性是指事务是一个不可分割的工作单位，事务中的操作要么都发生，要么都不发生。`
### 2. 一致性（Consistency）
`事务必须使数据库从一个一致性状态变换到另一个一致性状态。`
### 3. 隔离性（Isolation）
`事务的隔离性是指一个事务的执行不能被其他事务干扰，即一个事务内部的操作及使用的数据对并发的其他事务是隔离的，并发执行的各个事务之间不能相互干扰`
### 4. 持久性（Durability）
`持久性是指一个事务一旦被提交，它对数据库中数据的改变就是永久的，接下来的其它操作和数据库故障不应该对其有任何影响。`

#### 总结
>原子性：一个事务不可再分割，要么都执行，要么都不执行

>一致性：一个事务执行会使数据从一个一致状态变换到另一个一致状态

>隔离性：一个事务的执行不会受到其他事务的干扰

>持久性：一个事务一旦提交，则会永久改变数据库的数据

## 事务的创建
    事务的创建：
        隐式事务：
            事务没有明显的开启和结束的标记
            比如insert、update、delete语句
            DELETE FROM stu WHERE id = 1;
            这条语句就是一个事务
        显式事务：
            事务具有明显的开启和结束标记
            前提： 必须先设置自动提交功能为禁用
            SET autocommmit=0;
            SHOW VARIABLES LIKE 'autocommit';
```sql
--步骤1 开启事务
set autocommit = 0;
start transaction;
--步骤2 编写事务中的sql语句（select。insert、update、delete）
语句1
语句2
...
--步骤3 结束事务
commit;--提交
rollback;--回滚

#开启事务
SET autocommit=0;
START TRANSACTION;
#编写一组事务的语句
UPDATE stu SET class_id = 1009 WHERE id = 2;
DELETE FROM stu WHERE id = 2;
#结束事务
ROLLBACK; -- 回滚
```

## 注意
```sql
--对于同时运行的多个事务，当这些事务访问  数据库中相同的数据 时，如果没有采取必要的隔离机制，就会导致各种并发问题
```
>### 脏读
`对于两个事务T1，T2； T1读取了已经被T2更新但还 没有提交 的字段之后， 若T2回滚，T1读取的内容就是临时且无效的。`

>### 不可重复读
`对于两个事务T1， T2； T1读取了一个字段，然后T2更新了该字段之后，T1再次读取同一个字段，值就不同了`

>### 幻读
`对于两个事务T1，T2； T1从一个表中读取了一个字段，然后T2在该表中 插入 了一些新的行。之后，如果T1再次读取同一个表，就会多出几行`

>### 数据库事务的隔离性
`数据库系统必须具有隔离并发运行各个事务的能力，使他们不会相互影响，避免各种并发问题`

## mysql数据库提供四种事务隔离级别
```sql
--查看数据库默认隔离级别
SELECT @@tx_isolation;


READ UNCOMMITTED:
        会出现：脏读  不可重复读  幻读
READ COMMITED
        会出现：不可重复读  幻读
REPEATABLE READ：
        会出现： 幻读
SERIALIZABLE(串行化)
        会出现：不会出现并发问题，但是性能十分低下

--每启动一个mysql程序，就会获得一个单独的数据库连接，每个数据库连接都有一个全局变量@@tx_isolation,表示当前的事务隔离级别

--查看当前的隔离级别
SELECT @@tx_isolation;

--设置当前mysql连接的隔离级别
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
--设置数据库系统的全局隔离级别
SET GLOBAL TRANSACTION ISOLATION LEVEL READ COMMITTED;

```

### 示例
```sql
SET autocommit = 0;
START TRANSACTION;

DELETE FROM stu;
ROLLBACK;

#演示savepoint的使用
SET autocommit = 0;
START TRANSACTION;
DELETE FROM stu WHERE id = 2;
SAVEPOINT a;
DELETE FROM stu WHERE id = 4;
ROLLBACK TO a;
```

# 视图
```sql
-- 视图为虚拟表，和普通表一样使用
-- 是mysql5.1版本出现的新特性，是通过表动态生成的数据。

--虚拟存在的表，行和列的数据来自定义的属兔的查询中使用的表，并且是在使用视图时 动态生成的,只保存了sql逻辑，不保存查询结果


--应用场景：
        --多个地方用到同样的查询结果
        --该查询结果使用的sql语句比较复杂
--示例：
CREATE VIEW get_class
AS
SELECT stu.name, class.name
FROM stu, class
where stu.class_id = class.id;

```