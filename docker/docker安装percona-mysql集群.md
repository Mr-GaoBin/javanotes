# **docker安装percona-mysql集群**



## 创建docker内部网络

```
docker network create --subnet=172.18.0.0/24 percona-mysql
```

## 创建docker卷

```
docker volume create --name v1
docker volume create --name v2
docker volume create --name v3
docker volume create --name v4
docker volume create --name v5
```

## 创建容器

```
docker run -di -p 3306:3306 \
-v v1:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=P@88w0rd \
-e CLUSTER_NAME=PXC \
-e XTRABACKUP_PASSWORD=P@88w0rd \
--privileged=true --name=node1 --net=percona-mysql --ip 172.18.0.11 \
docker.io/percona/percona-xtradb-cluster:5.7

docker run -di -p 3308:3306 \
-v v2:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=P@88w0rd \
-e CLUSTER_NAME=PXC \
-e XTRABACKUP_PASSWORD=P@88w0rd \
-e CLUSTER_JOIN=node1 \
--privileged=true --name=node2 --net=percona-mysql --ip 172.18.0.12 \
docker.io/percona/percona-xtradb-cluster:5.7

docker run -di -p 3308:3306 \
-v v3:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=P@88w0rd \
-e CLUSTER_NAME=PXC \
-e XTRABACKUP_PASSWORD=P@88w0rd \
-e CLUSTER_JOIN=node1 \
--privileged=true --name=node3 --net=percona-mysql --ip 172.18.0.13 \
docker.io/percona/percona-xtradb-cluster:5.7

docker run -di -p 3309:3306 \
-v v4:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=P@88w0rd \
-e CLUSTER_NAME=PXC \
-e XTRABACKUP_PASSWORD=P@88w0rd \
-e CLUSTER_JOIN=node1 \
--privileged=true --name=node4 --net=percona-mysql --ip 172.18.0.14 \
docker.io/percona/percona-xtradb-cluster:5.7

docker run -di -p 3310:3306 \
-v v5:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=P@88w0rd \
-e CLUSTER_NAME=PXC \
-e XTRABACKUP_PASSWORD=P@88w0rd \
-e CLUSTER_JOIN=node1 \
--privileged=true --name=node5 --net=percona-mysql --ip 172.18.0.15 \
docker.io/percona/percona-xtradb-cluster:5.7
```

## 切记

>#### 因第一个节点初始化比较耗时一定要等第一个容器创建成功可以使用MySQL客户端连接了才能创建第二个，或者会报错创建不了下面的容器！！！