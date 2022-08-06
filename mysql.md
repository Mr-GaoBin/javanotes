# mysql

```
https://www.kaggle.com/datasets)是一个数据网站，里面有各种各样的数据
```

## MySQL特点

>体积小，速度快，成本低，开源免费
>
>不区分大小写
>以分号结尾
>#和--都表示单行注释
>/*多行注释*/

## sqlserver，mysql，Oracle的对比

	1、Oracle 贵，安装不方便，不开源，安全性高
	
	2、sqlserver 较贵，安装比较方便，不开源，容易上手，安全性较高
	
	3、MySQL  开源免费，体积小，安装方便，速度快



## 建表建库

>net start mysql                                                                               -- 启动服务

### 数据库和表操作

```sql
distinct                                                     -- 去重复关键字
show databases										     -- 查看数据库命令
show tables												-- 查看数据库中的表
create database 数据库名								  -- 创建数据库命令
use 数据库名										     -- 进入数据库
create table 表名	     								    -- 创建表
auto_increment											 -- 自增约束


show variables like 'datadir';#查看数据库的位置
show databases;#查看所有的数据库
show tables;#查看当前数据库中所有的表
show create table users;#查看表的x信息
show create database mytest;#查看创建数据语句信息
desc users;#查看表的结构是信息

drop database  if not EXISTS  mysql;#判断mytest是否存在，如果存在则删除数据库
create database mysql character set utf8;#创建数据库并且设置编码为utf8



数据表修改
alter table 表名 add 字段名 数据类型 数据类型属性;——添加字段
alter table 表名 change 字段名 新名称 数据类型 数据类型属性;——修改字段
alter table 表名 drop 字段名;——删除字段
alter table 表名 rename to 新表名;——修改表名称
alter talbe 表名 add primary key(主键列);——添加主键

	复制
create table 新表名 select 字段 from 旧表名;——不能复制键
create table 新表名 like 旧表名;





```



### 数据操作

```sql


	添加数据：

		insert into 表名[(字段列表)] values(值列表);
		或则
		insert into 表名[(字段列表)] values(值列表),(值列表),(值列表),....;

		insert into 表名 set 字段名=值,字段名=值,....;

		insert into 表名 select 字段列表 from 表名;


	修改数据：

		update 表名 set 字段名=值,字段名=值,.... [where 条件表达式];

	删除数据：

		delete from 表名 [where 条件表达式];

		truncate table 表名;

		区别：
			delete是一种DML(数据操作语言)，事务提交后生效；而truncate是一种DDL(数据定义语言)，不能够确保事务的安全性

			delete删除数据不会释放表空间，而truncate删除数据后重新创建表，释放表空间

			delete删除数据后自增列继续累加；而truncate删除后自增列还原为初始值
		
```

### drop和delete,truncate的区别

| 操作      | drop   | delete                 | truncate |
| --------- | ------ | ---------------------- | -------- |
| 删除内容  | true   | true                   | true     |
| 删除结构  | true   | fales                  | fales    |
| 是/否回滚 | fales  | true                   | fales    |
| 操作范围  | 表，库 | 表部分删除，视图，索引 | 清空表   |

### 理论知识

```sql
#什么是数据的完整性
	-- 数据完整性就是保证数据精确性和准确性
#有哪些数据的完整性
	-- 实体完整性，域完整性 ，引用完整性，自定义完整性
	-- 实体完整性又称行完整性
  -- 域完整性又列完整性
  -- 自定义完整性
  -- 引用 又称关系

#通过哪些约束来实现数据完整性
#主键，唯一，默认，标识列，外键，非空
#PRIMARY KEY 主键
#UNIQUE 唯一
#DEFAULT 默认约束
#auto_increment 标识列 整型
# FOREIGN key(外键字段)  REFERENCES 主键表(主键字段)
#非空 not null
#枚举类型实现固定元素 ENUM('元素1','元素2'.....)

#mysql可以使用的数据类型有哪些？

#NOW()获取系统的当前日期和时间 
```



### having 和 where

```



```





### 外键的[级联删除](https://so.csdn.net/so/search?q=级联删除&spm=1001.2101.3001.7020)和级联置空

```
 on delete cascade							-- 级联删除关键字
 on delete set null					         -- 级联置空关键字
 on DELETE RESTRICT							-- 级联拒绝删除关键字
```

### 创建表外外键约束

```
alter table  外键 add CONSTRAINT 外键名称 FOREIGN key(外键列) REFERENCES 主键(主键列)
```

### 删除外键约束

```
ALTER table 外键 DROP FOREIGN key 外键名称;
```

### 分页查询

```
-- 使用limit，要求用两种方式完成；
select *from 表名 LIMIT a;					--返回前a条数据
select *from stuinfo LIMIT a,b;			   -- a表示开始位置  b表示结束位置
```

### 子查询的分页方式

```
```

### 升序和降序

```
order by...asc 升序

order by ...desc 降序
```



### 分组查询

```
group by ...having
```

### 模糊查询

```
模糊查询: like

			%任意字符 

			[]范围  ，查询电话号中包含3,5的数字
			[^ ]不包含的范围
			_ 占用一个字符  查询电话号码第几位的数字是.... __3%

		select * from 表  where 字段名  like 模糊条件  %张
			
		非空: is null   is not null
		
		
	模糊查询：得到和指定值相匹配的数据集合

		select 字段列表 from 表名 where 字段名 like ?;

		匹配符：%(匹配0个或多个任意字符)

			_(匹配任意一个字符)

			[范围](匹配指定范围内的单个字符)    name like [abcde]  <==>  name='a' or name='b' or name='c' or name='d' or name='e'

			[^范围](匹配不在指定范围内的单个字符)    name like [^ab]  <==>  name!='a' and name!='b'
```

### 子查询

```
子查询：将一个查询语句放在其它语句中

		将子查询看做一张表：select * from (select * from 表名) 别名;

		将子查询看做条件：select * from 表名 where 字段=(select 字段 from 表名)

		将子查询用在更新语句中：delete from 表名 where id in(select id from 表名)

```



### 连接查询

```
联合查询：将两张或多张表的数据组合在一个结果集显示

		交叉连接：select * from 表1,表2,....;	注意：结果返回笛卡尔积

		例如：由两个表，学生信息表和学生成绩表
		   查询学生的姓名，性别，电话，成绩

		内连接：select * from 表1 inner join 表2 on 连接条件

		外连接：select * from 表1 left/right join 表2 on 连接条件

		自连接：select * from 表名 别名1,表名 别名2 on 连接条件
```



### 聚合函数



### 去重复关键字distinct

```
distinct关键字必须在select后面第一位
select  distinct * from stuinfo;

-- 》查询出被学生选修过的课程编号
select distinct cno from score where cj is not null;
```

### distinct关键字错误理解的sql使用

```
```



### 储存引擎

```sql
show engines;-- 查看mysql可以只用的存储引擎
show variables like '%storage_engine%';-- 查看当前mysql中默认的存储引擎
ALTER TABLE stumarks ENGINE=myisam; --  修改当前使用的表中存在的引擎


-- 引擎创建
-- 在创建表的过程中将表默认添加engine=INNODB/myisam
-- 查看表的存储引擎
--  show create table 表名.
-- 查看整个数据库支持的存储引擎
-- show engines;
-- 查看表默认的存储引擎
-- show variables like '%storage_engine%';
-- 修改表中已经存在的存储引擎
-- alter table 表名字 ENGINE = INNODB;
-- 在mysql中常用的引擎 myisam和innerDB
-- 区别：myisam添加，查询速度快，空间占用小，但是不支持事务。innoDB是支持事务处理，但是添加，查询速度慢，占用空间高是5.5以后默认的存储引擎



```





|      | innerDB | myisam |
| ---- | ------- | ------ |
| 事务 |         |        |
| 行锁 |         |        |
| 索引 |         |        |
|      |         |        |







## MySQL函数





## 事务

### 什么是事务

>- MySQL 事务主要用于处理操作量大，复杂度高的数据。
>- 事务处理可以用来维护数据库的完整性，保证成批的 SQL 语句要么全部执行，要么全部不执行。

### 事务的特性（ACID）

>
>
>- 原子性（ A tomicity，或称不可分割性）
>
>  ​		要么全部完成，要么全部不完成。
>
>- 一致性（ C onsistency）
>
>  ​		在事务开始之前和事务结束以后，数据库的完整性没有被破坏。
>
>- 隔离性（ I solation，又称独立性）
>
>  ​       防止多个事务并发执行时由于交叉执行而导致数据的不一致。
>
>- 持久性（ D urability）
>
>  ​		事务处理结束后，对数据的修改就是永久的

### 事务并发可能出现的情况

(https://developer.aliyun.com/article/743691)

脏读（Dirty Read）

>一个事务读到了另一个未提交事务修改过的数据

不可重复读（Non-Repeatable Read）

>务只能读到另一个已经提交的事务修改过的数据，并且其他事务每对该数据进行一次修改并提交后，该事务都能查询得到最新值。（不可重复读在读未提交和读已提交隔离级别都可能会出现）

幻读（Phantom）

>一个事务先根据某些条件查询出一些记录，之后另一个事务又向表中插入了符合这些条件的记录，原先的事务再次按照该条件查询时，能把另一个事务插入的记录也读出来。（幻读在读未提交、读已提交、可重复读隔离级别都可能会出现）

### 隔离级别

>MySQL的事务隔离级别一共有四个，分别是读未提交、读已提交、可重复读以及可串行化。
>
>MySQL的隔离级别的作用就是让事务之间互相隔离，互不影响，这样可以保证事务的一致性。
>
>隔离级别比较：可串行化>可重复读>读已提交>读未提交
>
>隔离级别对性能的影响比较：可串行化>可重复读>读已提交>读未提交
>
>由此看出，隔离级别越高，所需要消耗的MySQL性能越大（如事务并发严重性），为了平衡二者，一般建议设置的隔离级别为可重复读，MySQL默认的隔离级别也是可重复读。

读未提交（READ UNCOMMITTED）

>在读未提交隔离级别下，事务A可以读取到事务B修改过但未提交的数据。
>
>可能发生脏读、不可重复读和幻读问题，一般很少使用此隔离级别。

读已提交（READ COMMITTED）

>在读已提交隔离级别下，事务B只能在事务A修改过并且已提交后才能读取到事务B修改的数据。
>
>读已提交隔离级别解决了脏读的问题，但可能发生不可重复读和幻读问题，一般很少使用此隔离级别。

可重复读（REPEATABLE READ）

>在可重复读隔离级别下，事务B只能在事务A修改过数据并提交后，自己也提交事务后，才能读取到事务B修改的数据。
>
>可重复读隔离级别解决了脏读和不可重复读的问题，但可能发生幻读问题。
>
>提问：为什么上了写锁（写操作），别的事务还可以读操作？
>
>因为InnoDB有MVCC机制（多版本并发控制），可以使用快照读，而不会被阻塞。

可串行化（SERIALIZABLE）

>各种问题（脏读、不可重复读、幻读）都不会发生，通过加锁实现（读锁和写锁）。

## 视图

## 索引

## 储存过程

## 触发器









## MySQL备份

数据库备份必要性

>- 保证重要数据不丢失
>- 数据转移

MySQL数据库备份方法

>- mysqldump备份工具
>- 数据库管理工具,如SQLyog
>- 直接拷贝数据库文件和相关配置文件

### 导入与导出

可视化工具导入与导出

导出MySQL

```shell
#mysql/bin目录下
mysqldump -u 用户 库 >导出位置 文件名 -p
enter 密码

mysqldump -u root data >d:\\ data.sql -p
enter root
```

导入MySQL

```shell
#查看是否加载本地文件
SHOW GLOBAL VARIABLES LIKE 'local_infile';
#开启全局本地文件配置
SET GLOBAL local_ infile = true;
#创建空库
create database data

#mysql/bin目录下
mysqld -u用户 -p密码 库 >导入文件位置 文件名 

mysqldump -uroot -proot data >d:\\ data.sql 

source d:/car.sql;
```

### windows重装MySQL

```apl
C:\ProgramData\mysql
C:\Program Files\\mysql
#删除注册表
路径1：HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\eventlog\Application\MySQL
路径:2：HKEY_LOCAL_MACHINE\SYSTEM\ControlSet002\services\eventlog\Application\MySQL
路径3：HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\MySQL
```

## 规范化数据库设计

**糟糕的数据库设计**

>- 数据冗余,存储空间浪费
>- 数据更新和插入的异常
>- 程序性能差

**良好的数据库设计**

>- 节省数据的存储空间
>- 能够保证数据的完整性
>- 方便进行数据库应用系统的开发

**软件项目开发周期中数据库设计** :

>- 需求分析阶段: 分析客户的业务和数据处理需求
>- 概要设计阶段:设计数据库的E-R模型图 , 确认需求信息的正确和完整.

**设计数据库步骤**

>- 收集信息
>
>  ​		与该系统有关人员进行交流 , 座谈 , 充分了解用户需求 , 理解数据库需要完成的任务.
>
>- 标识实体[Entity]
>
>  ​		标识数据库要管理的关键对象或实体,实体一般是名词
>
>- 标识每个实体需要存储的详细信息[Attribute]
>
>- 标识实体之间的关系[Relationship]
>
>

## 三大范式

不合规范的表设计会导致的问题：

- 信息重复
- 更新异常
- 插入异常
- 无法正确表示信息
- 删除异常
- 丢失有效信息

>**第一范式** (1st NF)原子性
>
>第一范式的目标是确保每列的原子性,如果每列都是不可再分的最小数据单元,则满足第一范式
>
>**第二范式**(2nd NF)依赖性
>
>第二范式（2NF）是在第一范式（1NF）的基础上建立起来的，即满足第二范式（2NF）必须先满足第一
>
>第二范式要求每个表只描述一件事情
>
>**第三范式**(3rd NF)消除传递依赖
>
>如果一个关系满足第二范式,并且除了主键以外的其他列都不传递依赖于主键列,则满足第三范式.
>
>第三范式需要确保数据表中的每一列数据都和主键直接相关，而不能间接相关



## 五大约束

>1.主键约束（Primay Key Coustraint） 唯一性，非空性；
>
>2.唯一约束 （Unique Counstraint）唯一性，可以空，但只能有一个；
>
>3.默认约束 (Default Counstraint) 该数据的默认值；
>
>4.外键约束 (Foreign Key Counstraint) 需要建立两表间的关系；
>
>5.非空约束（Not Null Counstraint）:设置非空约束，该字段不能为空



## 常见问题

>MySQL 安装后，系统会自动创建名为 information_schema 和 mysql 的两个系统数据库，系统数据库存放一些和数据库相关的信息
>
>如果删除了这两个数据库，MySQL 将不能正常工作

### 远程连接MySQL删除数据库时无响应解决办法

```shell
#在远程主机上登录MySQL，执行show full processlist观察state和info两列，查看有哪些线程在运行。经过查询发现之前远程删除的时候由于网络中断，锁表了。所以导致再次登录的时候删除操作无响应。
show full processlist;
#结束线程
kill id;
```



>远程连接1153错误

```shell
set global max_allowed_packet=1000000000;
set global net_buffer_length=1000000;
FLUSH PRIVILEGES;
```



```shell
#查看innodb引擎的运行时信息
show engine innodb status;
#查询是否锁表
show OPEN TABLES where In_use > 0;

#查看正在锁的事务
SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCKS; 

#查看等待锁的事务
SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCK_WAITS; 

#show status like '%lock%';
查看服务器状态

#show variables like '%timeout%';
查看服务器配置参数。

#隔离级别
select @@tx_isolation;

#自动开启声明式事务
set autocommit=0;

```

1. MySQL里有哪些锁？
2. 如何造成锁表？
3. 如何造成死锁？
4. 全局锁加锁方法的执行命令是什么?主要的应用场景是什么?
5. 做整库备份时为什么要加全局锁?
6. MySQL的自带备份工具, 使用什么参数可以确保一致性视图, 在什么场景下不适用?
7. 不建议使用set global readonly = true的方法加全局锁有哪两点原因?
8. 表级锁有哪两种类型? 各自的使用场景是什么?
9. MDL中读写锁之间的互斥关系怎样的?
10. 如何安全的给小表增加字段?
