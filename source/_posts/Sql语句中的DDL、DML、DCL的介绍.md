---
title: Sql语句中的DDL、DML、DCL的介绍
date: 2019-03-18 20:38:49
tags: sql
declare: true
---

# 一、DDL #

**DDL is Data Definition Language statements.** 数据定义语言，用于定义和管理 SQL 数据库中的所有对象的语言

1.**CREATE** - to create objects in the database 创建
2.**ALTER** - alters the structure of the database 修改
3.**DROP** - delete objects from the database 删除
4.**TRUNCATE** - remove all records from a table, including all spaces allocated for the records are removed

TRUNCATE TABLE [Table Name]。
下面是对Truncate语句在MSSQLServer2000中用法和原理的说明：
Truncate table 表名 速度快,而且效率高,因为:
TRUNCATE TABLE 在功能上与不带 WHERE 子句的 DELETE 语句相同：二者均删除表中的全部行。但 TRUNCATE TABLE 比 DELETE 速度快，且使用的系统和事务日志资源少。
DELETE 语句每次删除一行，并在事务日志中为所删除的每行记录一项。TRUNCATE TABLE 通过释放存储表数据所用的数据页来删除数据，并且只在事务日志中记录页的释放。
TRUNCATE TABLE 删除表中的所有行，但表结构及其列、约束、索引等保持不变。新行标识所用的计数值重置为该列的种子。如果想保留标识计数值，请改用 DELETE。如果要删除表定义及其数据，请使用 DROP TABLE 语句。
对于由 FOREIGN KEY 约束引用的表，不能使用 TRUNCATE TABLE，而应使用不带 WHERE 子句的 DELETE 语句。由于 TRUNCATE TABLE 不记录在日志中，所以它不能激活触发器。
TRUNCATE TABLE 不能用于参与了索引视图的表。
5.**COMMENT** - add comments to the data dictionary 注释
6.**GRANT** - gives user's access privileges to database 授权
7.**REVOKE** - withdraw access privileges given with the GRANT command 收回已经授予的权限


# 二、DML #

**DML is Data Manipulation Language statements.** 数据操作语言，SQL中处理数据等操作统称为数据操纵语言

1.**SELECT** - retrieve data from the a database 查询
2.**INSERT** - insert data into a table 添加
3.**UPDATE** - updates existing data within a table 更新
4.**DELETE** - deletes all records from a table, the space for the records remain 删除
5.**CALL** - call a PL/SQL or Java subprogram
6.**EXPLAIN PLAN** - explain access path to data 
Oracle RDBMS执行每一条SQL语句，都必须经过Oracle优化器的评估。所以，了解优化器是如何选择(搜索)路径以及索引是如何被使用的，对优化SQL语句有很大的帮助。Explain可以用来迅速方便地查出对于给定SQL语句中的查询数据是如何得到的即搜索路径(我们通常称为Access Path)。从而使我们选择最优的查询方式达到最大的优化效果。
7.**LOCK TABLE** - control concurrency 锁，用于控制并发


# 三、DCL #

**DCL is Data Control Language statements.** 数据控制语言，用来授予或回收访问数据库的某种特权，并控制数据库操纵事务发生的时间及效果，对数据库实行监视等
**COMMIT** - save work done 提交
**SAVEPOINT** - identify a point in a transaction to which you can later roll back 保存点
**ROLLBACK** - restore database to original since the last COMMIT 回滚
**SET TRANSACTION **- Change transaction options like what rollback segment to use 设置当前事务的特性，它对后面的事务没有影响．