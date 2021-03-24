---
title: 'MySQL事务锁命令语句集合'
date: 2021-03-21 19:20:07
tags:
 - MySql
 - 事务
categories:
 - MySql
---



查看数据库innodb状态
`show engine innodb status`

查看当前是否有事务运行
`select * from information_schema.INNODB_TRX;`

通过kill结束当前事务
`kill 'trx_mysql_thread_id'`

查看当前线程处理情况，如果不使用full关键字，信息字段中只会显示每个语句的前100个字符。
`show processlist; `
`show full processlist;`

查询表级锁争用情况 Table_locks_immediate  指的是能够立即获得表级锁的次数  Table_locks_waited  指的是不能立即获取表级锁而需要等待的次数
`show status like 'Table%';`

查看行锁使用情况
`show statue like 'innodb_row_lock%';`

获取锁定次数、锁定造成其他线程等待次数，以及锁定等待时间信息
`show status like '%lock%';`

innodb的dml操作的行级锁的等待时间
`SHOW VARIABLES LIKE 'innodb_lock_wait_timeout';`

查看timeout相关参数
`show variables like '%timeout%';`

查看正在被锁定的的表
`SHOW OPEN TABLES where In_use > 0;`

查看被锁住的
`SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCKS; `

等待锁定
`SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCK_WAITS; `

查看表索引信息
`SHOW INDEX FROM account;`

查看当前的隔离级别:
`SELECT @@tx_isolation;`

设置当前 mySQL 连接的隔离级别:
会话级：只对当前的会话效 
`set  transaction isolation level repeatable read; `

设置数据库系统的全局的隔离级别:
全局级：对所的会话效 
 `set global transaction isolation level repeatable read;`

自动提交模式可以通过服务器变量AUTOCOMMIT来控制。
`show variables like '%auto%';`
`show variables like '%autocommit%';`

通过查询`information_schema`数据库事务表的信息
`select * from information_schema.INNODB_TRX;`
下面对 `innodb_trx` 表的每个字段进行解释：
```
trx_id：事务ID。
trx_state：事务状态，有以下几种状态：RUNNING、LOCK WAIT、ROLLING BACK 和 COMMITTING。
trx_started：事务开始时间。
trx_requested_lock_id：事务当前正在等待锁的标识，可以和 INNODB_LOCKS 表 JOIN 以得到更多详细信息。
trx_wait_started：事务开始等待的时间。
trx_weight：事务的权重。
trx_mysql_thread_id：事务线程 ID，可以和 PROCESSLIST 表 JOIN。
trx_query：事务正在执行的 SQL 语句。
trx_operation_state：事务当前操作状态。
trx_tables_in_use：当前事务执行的 SQL 中使用的表的个数。
trx_tables_locked：当前执行 SQL 的行锁数量。
trx_lock_structs：事务保留的锁数量。
trx_lock_memory_bytes：事务锁住的内存大小，单位为 BYTES。
trx_rows_locked：事务锁住的记录数。包含标记为 DELETED，并且已经保存到磁盘但对事务不可见的行。
trx_rows_modified：事务更改的行数。
trx_concurrency_tickets：事务并发票数。
trx_isolation_level：当前事务的隔离级别。
trx_unique_checks：是否打开唯一性检查的标识。
trx_foreign_key_checks：是否打开外键检查的标识。
trx_last_foreign_key_error：最后一次的外键错误信息。
trx_adaptive_hash_latched：自适应散列索引是否被当前事务锁住的标识。
trx_adaptive_hash_timeout：是否立刻放弃为自适应散列索引搜索 LATCH 的标识。
```
下面对 `innodb_locks` 表的每个字段进行解释：
```
lock_id：锁 ID。
lock_trx_id：拥有锁的事务 ID。可以和 INNODB_TRX 表 JOIN 得到事务的详细信息。
lock_mode：锁的模式。有如下锁类型：行级锁包括：S、X、IS、IX，分别代表：共享锁、排它锁、意向共享锁、意向排它锁。表级锁包括：S_GAP、X_GAP、IS_GAP、IX_GAP 和 AUTO_INC，分别代表共享间隙锁、排它间隙锁、意向共享间隙锁、意向排它间隙锁和自动递增锁。
lock_type：锁的类型。RECORD 代表行级锁，TABLE 代表表级锁。
lock_table：被锁定的或者包含锁定记录的表的名称。
lock_index：当 LOCK_TYPE=’RECORD’ 时，表示索引的名称；否则为 NULL。
lock_space：当 LOCK_TYPE=’RECORD’ 时，表示锁定行的表空间 ID；否则为 NULL。
lock_page：当 LOCK_TYPE=’RECORD’ 时，表示锁定行的页号；否则为 NULL。
lock_rec：当 LOCK_TYPE=’RECORD’ 时，表示一堆页面中锁定行的数量，亦即被锁定的记录号；否则为 NULL。
lock_data：当 LOCK_TYPE=’RECORD’ 时，表示锁定行的主键；否则为NULL。
```
下面对 `innodb_lock_waits` 表的每个字段进行解释：
```
requesting_trx_id：请求事务的 ID。
requested_lock_id：事务所等待的锁定的 ID。可以和 INNODB_LOCKS 表 JOIN。
blocking_trx_id：阻塞事务的 ID。
blocking_lock_id：某一事务的锁的 ID，该事务阻塞了另一事务的运行。可以和 INNODB_LOCKS 表 JOIN。
```

