MySQL的事务主要的作用在于，处理操作量大，复杂度高的数据。
比如在人员管理系统要删除一个人员，需要删除基本资料、邮箱、文章等等，这些操作一起构成了一个事务

- 在MySQL中只有使用了 InnoDB 数据库引擎的数据库 或 表 才支持事务
- 事务处理可以用来维护数据库的完整性，保证成批的SQL语句要么都执行，要么全部不执行
- 事务主要用来管理 insert、update、delete语句

### 事务的 ACID 特性：

- Atomicity 原子性：一个事务（transaction）中的所有操作，要么全部完成，要么都不完成。如果事务在执行过程中出现错误，会回滚到事务开始前的状态。
- Consistency 一致性：事务的执行不能破坏数据库数据的完整性和一致性，一个事务在执行之前和执行之后，数据库都必须处于一致性状态。
- Isolation 隔离性：并发环境中，数据库允许多个并发事务同时执行，一个事务的执行不被其它事务所干扰，事务的隔离级别又可以分为：
  - 读未提交 read uncommited
  - 读已提交 read commited
  - 可重复读 repeatable read
  - 串行化 serializable
- Durability 持久性： 一旦事务处理完，它对数据库中对应数据的变更是永久的

### 事务控制语句：

- BEGIN 或 START TRANSACTION 显式地开启一个事务
- COMMIT 会提交事务，并使当前已对数据库产生的修改成为永久性的
- ROLLBACK 回滚并结束用户的事务，并撤销正在进行的所有未提交的修改
- SAVEPOINT identifier 可以在事务中创建一个保存点，一个事务可以有多个保存点
- RELEASE SAVEPOINT identifier 删除一个事务的保存点  如果没有保存点，会抛出异常
- ROLLBACK identifier 把事务回滚到标记点
- SET TRANSACTION 用来设置事务的隔离级别 InnoDB可以设置四个级别

### MYSQL 事务处理的两种方法

1. 用BEGIN、ROLLBACK、COMMIT来实现

- BEGIN 开始一个事务
- ROLLBACK 事务回滚
- COMMIT 事务确认

2. 直接用 SET 来改变 MySQL 的自动提交模式

- SET AUTOCOMMIT = 0 禁止自动提交
- SET AUTOCOMMIT = 1 开启自动提交

```SQL
mysql> begin;  # 开始事务
Query OK, 0 rows affected (0.00 sec)
 
mysql> insert into runoob_transaction_test value(5);
Query OK, 1 rows affected (0.01 sec)
 
mysql> insert into runoob_transaction_test value(6);
Query OK, 1 rows affected (0.00 sec)
 
mysql> commit; # 提交事务
Query OK, 0 rows affected (0.01 sec)
 
mysql>  select * from runoob_transaction_test;
+------+
| id   |
+------+
| 5    |
| 6    |
+------+
2 rows in set (0.01 sec)
 
mysql> begin;    # 开始事务
Query OK, 0 rows affected (0.00 sec)
 
mysql>  insert into runoob_transaction_test values(7);
Query OK, 1 rows affected (0.00 sec)
 
mysql> rollback;   # 回滚
Query OK, 0 rows affected (0.00 sec)
 
mysql>   select * from runoob_transaction_test;   # 因为回滚所以数据没有插入
+------+
| id   |
+------+
| 5    |
| 6    |
+------+
2 rows in set (0.01 sec)
 
mysql>
```