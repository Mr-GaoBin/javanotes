

# **docker安装nacos**

从[源码仓库](https://gitee.com/mirrors/Nacos)中找出指定mysql数据库执行nacos的脚本

[mysql脚本](https://gitee.com/mirrors/Nacos/raw/develop/distribution/conf/nacos-mysql.sql)

```shell
create database nacos default charset utf8mb4;
use nacos;

# 接下来在navicat或者各种连接工具中执行mysql脚本，执行完成脚本再执行docker创建nacos命令
docker run -d --restart=always \
-p 8848:8848 -p 9848:9848 -p 9849:9849 --link mysql:mysql \
-e MODE=standalone \
-e SPRING_DATASOURCE_PLATFORM=mysql \
-e MYSQL_SERVICE_HOST=mysql \
-e MYSQL_SERVICE_DB_NAME=nacos \
-e MYSQL_SERVICE_PORT=3306 \
-e MYSQL_SERVICE_USER=root \
-e MYSQL_SERVICE_PASSWORD=P@88w0rd \
--name nacos nacos/nacos-server:v2.0.4
```