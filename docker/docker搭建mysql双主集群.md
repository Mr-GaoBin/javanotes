# **docker搭建mysql双主集群**



# 第一步

注意:    如果为aarch64(arm)平台的话目前docker只支持8.0版本的dokcer镜像需要由mysql/mysql-server:5.7更换为mysql/mysql-server:8.0-aarch64

>docker安装两个mysql

```
docker network create --driver=bridge mysql

server_id=1
server_port=13306
name='mysql'$server_id

docker run -d -p $server_port:3306 \
--net=mysql \
-e MYSQL_ROOT_PASSWORD=P@88w0rd \
--name $name -h $name --restart=always \
-v /data/$name/:/var/lib/mysql mysql/mysql-server:5.7 \
--default-authentication-plugin=mysql_native_password \
--character-set-server=utf8mb4 \
--collation-server=utf8mb4_general_ci \
--explicit_defaults_for_timestamp=true \
--lower_case_table_names=1 \
--group_concat_max_len=102400 \
--max_allowed_packet=128M \
--innodb_log_file_size=256M \
--server_id=$server_id \
--log-bin=mysql-bin \
--relay-log=mysql-relay-bin \
--binlog-format=ROW \
--log-slave-updates=1 \
--binlog-ignore-db=information_schema \
--binlog-ignore-db=mysql \
--binlog-ignore-db=performance_schema \
--binlog-ignore-db=sys \
--binlog-format=ROW \
--auto-increment-increment=2 \
--auto-increment-offset=$server_id \
--transaction-isolation=READ-COMMITTED

server_id=2
server_port=13307
name='mysql'$server_id

docker run -d -p $server_port:3306 \
--net=mysql \
-e MYSQL_ROOT_PASSWORD=P@88w0rd \
--name $name -h $name --restart=always \
-v /data/$name/:/var/lib/mysql mysql/mysql-server:5.7 \
--default-authentication-plugin=mysql_native_password \
--character-set-server=utf8mb4 \
--collation-server=utf8mb4_general_ci \
--explicit_defaults_for_timestamp=true \
--lower_case_table_names=1 \
--group_concat_max_len=102400 \
--max_allowed_packet=128M \
--innodb_log_file_size=256M \
--server_id=$server_id \
--log-bin=mysql-bin \
--relay-log=mysql-relay-bin \
--binlog-format=ROW \
--log-slave-updates=1 \
--binlog-ignore-db=information_schema \
--binlog-ignore-db=mysql \
--binlog-ignore-db=performance_schema \
--binlog-ignore-db=sys \
--auto-increment-increment=2 \
--auto-increment-offset=$server_id \
  --transaction-isolation=READ-COMMITTED

```

## 在mysql1上执行

 

>mysql7创建账号授权

```
GRANT replication SLAVE ON . TO 'slave' @'%' IDENTIFIED BY '123456';
```

>mysql8创建账号授权

```
create user 'slave'@'%' identified by '123456';
grant replication slave on *.* to 'slave'@'%';
```

## 在mysql2上执行

>mysql7创建账号授权

```
GRANT replication SLAVE ON . TO 'slave' @'%' IDENTIFIED BY '123456';
```

>mysql8创建账号授权

```
create user 'slave'@'%' identified by '123456';
grant replication slave on *.* to 'slave'@'%';
```



# 第二步

## 

>在mysql1上执行 
>
>```
>获取mysql1的binlog文件名称及点位，在得到结果后，不要再对此数据库进行修改操作
>SHOW MASTER STATUS;
>```
>
>在mysql2上执行 
>
>```
>获取mysql2的binlog文件名称及点位，在得到结果后，不要再对此数据库进行修改操作
>SHOW MASTER STATUS;
>```



# 第三步

>参数含义
>
>```
>master_host这里的ip就是mysql所在服务器对应的ip
>master_user就是在第一步配置的账号
>master_port就是mysql的端口
>master_password就是在第一步配置的密码
>master_log_file就是在第二步获得结果中的file参数
>master_log_pos就是在第二步中获得结果中的Position参数
>```
>
>

## 在mysql1上执行

>配置mysql2为主节点
>
>```
>CHANGE MASTER TO master_host = 'mysql2',
>master_user = 'slave',
>master_password = '123456',
>master_port = 3306,
>master_log_file = 'mysql-bin.000003',
>master_log_pos = 438;
>
>start slave;
>
>show slave status;
>```
>
>

## 在mysql2上执行

>配置mysql1为主节点
>
>```
>CHANGE MASTER TO master_host = 'mysql1',
>master_user = 'slave',
>master_password = '123456',
>master_port = 3306,
>master_log_file = 'mysql-bin.000003',
>master_log_pos = 438;
>
>start slave;
>
>show slave status;
>```
>
>

# 第四步

## 创建数据库验证

>在mysql1上创建数据库，mysql2上能够看到
>
>```
># mysql1
>create database demo;
># mysql2
>show databases;
>```



## 插入自增数据验证

>mysql1新增的数据为1,3递推，mysql2新增的数据2,4递推
>
>```
># mysql1
>CREATE DATABASE demo;
>use demo;
>CREATE TABLE `test` (
>  `id` int(10) NOT NULL AUTO_INCREMENT,
>  `name` varchar(255) DEFAULT NULL,
>  PRIMARY KEY (`id`)
>) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
>INSERT INTO test(`name`) VALUES ('test');
>INSERT INTO test(`name`) VALUES ('test');
>INSERT INTO test(`name`) VALUES ('test');
>select * from test;
>
># mysql2
>use demo;
>INSERT INTO test(`name`) VALUES ('test');
>INSERT INTO test(`name`) VALUES ('test');
>INSERT INTO test(`name`) VALUES ('test');
>select * from test;
>```



 

## 第五步

>开启root远程访问
>
>```
>use mysql;
>update user set host = '%' where user = 'root';
>flush privileges;
>alter user 'root'@'%' identified with mysql_native_password by 'P@88w0rd';
>```







