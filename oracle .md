# oracle 

## 简介

```
*.Oracle的使用：
启动服务：oracleserviceORCL 这个服务
启动监听：oracleOraDb19Home1TNS...Listener 通过第三方工具进行链接
可视化工具：plsql
安装好的oracle数据库：默认有三个账号
sys  数据库管理员
system 数据库操作员账户，默认口令 123456
scott  测试账户：默认密码 tiger 默认是锁定状态
需要解锁，授权【系统权限 和 表权限】后才能 登录数据库，操作数据；
```



## Linux docker安装11g

```






```

## Linux docker安装19c

## 重装/卸载

```
docker pull registry.cn-hangzhou.aliyuncs.com/zhuyijun/oracle:19c            --拉取镜像
```

```
regedit                          注册表
```

```
卸载plsql及残留文件
打plsql开注册表
1.HKEY_CURRENT_USER/Software/Allround Automations
2.HKEY_CURRENT_USER/Software/Microsoft/Security
关闭所有oracle服务
卸载oracle及残留文件
删除以下Oracle *文件夹和文件（如果存在）。
C:\Oracle或ORACLE_BASE
C:\Program Files\Oracle
C:\Program Files (x86)\Oracle
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\与Oracle相关的文件夹。
C:\Users与Oracle相关的文件夹。
清空C:\temp并回收站。
打开oracle注册表，依次展开HKEY_LOCAL_MACHINE\SOFTWARE，找到oracle，删除
依次展开HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services中，删除所有oracle开头的项
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Eventlog\Application，删除所有oracle开头的项
在HKEY_CLASSES_ROOT，删除以ora开头的项
```

```
https://localhost:5500/em
```

## 常用命令

```
*.链接数据： sqlplus/ as sysdba
*.查看 当前用户：show user
*.输入命令 sqlplus / as sysdba（以DBA身份登录oracle）
*.输入命令 select * from all_users;（查看DBA所能控制的全部用户。）
*.查看oracle版本号select * from v$version;
*.创建用户：create user c##用户名  identified by  密码;                            
*.修改用户口令 alter user 用户名 identified by 新密码;
删除用户：drop  user  用户名;
解锁用户：alter user 用户名 account unlock;（解锁是unlock，锁定是lock--  注意：必须使用管理员 对其 枷锁 或者解锁）
授予新登陆的用户创建权限：grant create session to  用户名 ;  
授予新创建的用户数据库管理员权限: grant dba to 用户名;
切换到新创建的用户登陆语法：connect 用户名/密码; 
如果用户拥有数据表，则不能直接删除，要用上关键字cascade:drop user 用户名 cascade; 
断开当前用户：
(1)disc
(2)设置显示的行数 set pagesize  数字  默认显示 14行
(3)设置显示的字符数：  set linesize 数字     默认显示80个 
(4)清空窗口内容：clear screen;
(5)退出：exit；

```

### 表空间

```
select * from Dba_Tablespaces              -- 查询所有表空间
elect * from user_users					 -- 当前用户表空间使用情况 
```



### 权限

## 常用函数

```
Ø  字符函数：对字符串操作。

Ø  数字函数：对数字进行计算，返回一个数字。

Ø  转换函数：可以将一种数据类型转换为另外一种数据类型。

Ø  日期函数：对日期和时间进行处理。
```

### 字符函数

```
ASCII(X)						返回字符X的ASCII码
CONCAT(X,Y)						连接字符串X和Y
INSTR(X,STR[,START][,N)			 从X中查找str，可以指定从start开始，也可以指定从n开始
LENGTH(X)						返回X的长度
LOWER(X)						X转换成小写
UPPER(X)						X转换成大写
LTRIM(X[,TRIM_STR])				 把X的左边截去trim_str字符串，缺省截去空格
RTRIM(X[,TRIM_STR])				 把X的右边截去trim_str字符串，缺省截去空格
TRIM([TRIM_STR  FROM]X)		      把X的两边截去trim_str字符串，缺省截去空格
REPLACE(X,old,new)			     在X中查找old，并替换成new
SUBSTR(X,start[,length])		 返回X的字串，从start处开始，截取length个字符，缺省length，默认到结尾
==========================================练习===================================================
        -- 字符串函数的使用
        select lower('Java55') as  小写  from dual;
        select upper('Java55') as  大写  from dual;
         
        select ltrim('    Java55     ') as  大写  from dual;
        
        select * from emp;
        select * from dept;
         
        select lower(ename) from emp;
```

### 转换函数

```
         select to_number('3.147855') from dual;
         
         select to_date('2018-10-22','yyyy/MM/dd') from dual;
         
          select nvl('3.147855',0) from dual;
         
          select nvl2('3.147855',12,55) from dual;
```



```
简单来说，dual表就是oracle与数据字典自动创建的一张表，这张表是一个单行单列的表，这个表只有1列：DUMMY，数据类型为VERCHAR2(1)，dual表中只有一个数据'X', Oracle有内部逻辑保证dual表中永远只有一条数据。dual表主要是用来选择系统变量或是求一个表达式的值。

比如：

求系统当前时间

SELECT sysdate FROM daul

求系统当前时间，并按设定的格式显示

select to_char(sysdate,'yyyy-mm-dd hh24:mi:ss') from dual;
```

## oracle中的常用数据类型

```sql
        number(m,[n]) 数值类型  包括了 整数和小数
           m 整数：
           n 小数
           
         number(3,1) -99.9  ~ 99.9
           
         integer 整数类型
                 
         char 字符类型， 固定长度
         
         char(5)  
           
         varchar2(5) 如果给定的字符个数 不足5个 不会 用空格填充；
         
         long 字符类型；最大可以存储2G的数据
         lob 大数据类型：最大可以存储4G的数据
         
         date 时间类型
```

## oracle中的事务操作

```
      
                   事务特性：原子性
                   			一致性
                   			隔离性
                   			持久性
                   定义:.........
                   
                   oracle中事务 默认开启，但是不会提交，需要手动进行；
                   
                   提交：commit;
                   回滚：rollback；
                   设置保存点：set  savepoint 名称;
                   回滚到某个点：rollback to  名称；
   ==========================================实例===================================================
    --事务的操作
       
        insert into studentinfo_table values(3,'小白1','123456','女',20,sysdate,'重庆');
        insert into studentinfo_table values(4,'小白2','123456','女',20,sysdate,'重庆');
        --设置保存点
        savepoint a_point;
        
        insert into studentinfo_table values(5,'小白3','123456','女',20,sysdate,'重庆');
        --设置保存点
        savepoint b_point;
        
        insert into studentinfo_table values(6,'小白4','123456','女',20,sysdate,'重庆');
        ---程序执行过程中如果出现问题 指定回滚到某个位置：
        rollback to  b_point;
        
        commit;
       
        select * from studentinfo_table;
                    
```

### 索引

### 储存过程

### 视图的使用

### 索引的使用

### 序列的使用

### 函数

### oracle 中的注解

```sql
-- 单行注释
/*多行注释*/
```

### oracle 中的命令规则

```
变量： v_变量名
常量：c_常量名
过程：pro_过程名
函数：fun_函数名
视图：view_视图名
索引：i_索引名
序列：seq_表名
```

### 包的使用

### 触发器的使用

### 数据备份/恢复

 

### 数据优化  

```
*.         查询少用*
*.         多表查询，尽量使用连接查询
*.         数据量较多时，考虑为字段设置合理的索引
*.         进行数据 修改,添加，删除操作比较频繁时，尽量多使用commit；
```

## navicat连接orcale问题

```
问题描述：
在成功安装了oracle19c后发现无法用Navicat进行连接，连接的时候报错ORA-28040 没有匹配的验证协议
解决办法：
找到pracle安装目录的sqlnet.ora文件，并添加下面两句配置

SQLNET.ALLOWED_LOGON_VERSION_SERVER=11

SQLNET.ALLOWED_LOGON_VERSION_CLIENT=11

不用重启，但是需要重新更改用户密码。打开cmd,使用管理员登陆oracle，修改你的用户密码

sql>sqlplus  / as sysdba

sql>alter user username identified by password;(username,改成你的用户名；password改成你的密码)

报错原因：
oracle版本与navicat版本不匹配，导致密码规则不一致，通过配置指定成低版本的密码规则
```

## Oracle中sys和system用户的区别

```
1.数据库的启动需要以SYSDBA/SYSOPER身份登录。

2.如果在同一主机上使用IPC连接到数据库使用操作系统授权，登录任何一个用户都可以拥有as sysdba和as sysoper。

3.sys和system用户的区别

    SYS用户具有DBA权限，并具有SYS模式。只能通过SYSDBA登录数据库，是Oracle数据库中权限最高的帐号。sys用户具有“SYSDBA”和“SYSOPER”权限，登陆em时也只能用这两个身份，不能用normal。而system登录em时只能用normal模式登录。sys拥有数据字典(dictionary),或者说dictionary属于sys schema。

    system用户具有DBA权限，但是没有SYSDBA权限。平常一般用该帐号管理数据库。登录em时只能使用normal登录。 

4.SYSDBA权限和SYSOPER权限区别

    “SYSOPER”权限，即数据库操作员权限，sysoper主要用来启动、关闭数据库，sysoper 登陆后用户是 public。权限包括：打开数据库(STARTUP, ALTER DATABASE OPEN/MOUNT/OPEN),服务器(CREATE SPFILE,etc)

  关闭数据库服务器 
  备份数据库 
  恢复数据库RECOVERY 
  日志归档ARCHIVELOG 
  会话限制RESTRICTED SESSION

    “SYSDBA”权限，即数据库管理员权限，最高的系统权限。任何具有sysdba登录后用户是“SYS”。权限包括：管理功能, 创建数据库(CREATE DATABASE)以及 “SYSOPER”的所有权限

      其他用户需要手动grant权限，show user为该用户的名称。

SQL>conn / as sysdba 
已连接。 
SQL>grant sysoper to test; 
授权成功。 
SQL>grant sysdba to test; 
授权成功。 
SQL>conn test/test as sysoper; 
已连接。 
SQL>show user 
USER 为"PUBLIC" 
SQL>conn test/test as sysdba; 
已连接。 
SQL>show user; 
USER 为"SYS" 
SQL>conn test/test; 
已连接。 
SQL>show user 
USER 为"test"

5.dba和sysdba的区别:

    sysdba，是管理oracle实例的，它的存在不依赖于整个数据库完全启动，只要实例启动了，他就已经存在，以sysdba身份登陆，装载数据库、打开数据库。
    只有在数据库完全启动后，dba角色才有了存在的基础.
```

