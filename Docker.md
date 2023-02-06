# docker基础应用

一个项目，开发和上线两套环境，应用环境配置费时费力，而且容易出问题。为了解决这问题，所谓开发即运维，就是开发人员使用 Docker 来解决 “它在我的机器可以正常运行” 的问题，它会将运行程序的相关配置打包（打包成 一个镜像），然后直接搬移到新的机器上运行。从而保证系统稳定性，提高部署效率。

官文地址（https://docs.docker.com/engine/reference/commandline/）

## yum安装docker

* 默认安装路径[/var/lib/docker]

>1.较旧的 Docker 版本称为 docker 或 docker-engine 。如果已安装这些程序，请卸载它们以及相关的依赖项。

```
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

>2.设置docker仓库

```
yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```

>3.安装阿里云镜像

```
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

>4.更新软件索引包

```
yum makecache fast
```

>5.安装docker CE

```
yum -y install docker-ce docker-ce-cli containerd.io  
```

>6.启动docker

```
systemctl start docker
```

>7.查看docker进程

```
ps -aux|grep docker
```

>8.查看docker版本号

```
docker version
```

>#### docker镜像操作

```shell
#查看docker主机所有镜像
docker images
-------------------------------------------------------------------------------------------------------
(镜像仓库源)                                        (镜像标签)   (镜像ID)       (镜像创建时间)  (镜像大小)
REPOSITORY                                             TAG       IMAGE ID       CREATED         SIZE
registry.cn-hangzhou.aliyuncs.com/zhuyijun/oracle      19c       7b5eb4597688   19 months ago   6.61GB
registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g   latest    3fa112fd3642   6 years ago     6.85GB
-------------------------------------------------------------------------------------------------------
#搜索镜像
docker search 镜像名称								 
# 下载镜像
docker pull 镜像名称								
# 删除指定多个镜像，id之间记住空格
docker rmi -f 镜像id							     
# 递归删除所有镜像
docker rmi -f $(docker images -aq)					
# 镜像构建历史
docker history 镜像id                                  
```

>#### 容器 操作

```shell
#启动进入容器
docker run --name 自定义容器名称 镜像名称 -it 			  
#后台方式运行
docker run -d -it --name 自定义容器名称 镜像名称          -- 
@ —name="Name" 容器名字,用于区分容器
@ -d 后台运行
@ -it 使用交互方式运行,进入容器查看内容

# 指定容器端口(小写)								   
@ -p 容器端口
@ -p ip:主机端口:容器端口										 
@ -p 主机端口:容器端口(最常用)
#随机指定端口(大写)
@ -P 													

#列出当前正在运行的容器
docker ps 		  				 					
#列出历史运行过的容器
docker ps -a      				 					 
#显示最近创建的容器
docker ps -n=条数 			    				   
#只显示容器的
docker ps -q      				 					
# 启动容器
docker start 容器id               					
# 重启容器
docker restart 容器id             					
# 关闭容器
docker stop 容器id			    				  
# 强制启动容器
docker kill 容器id                					
# 删除容器
docker rm images	   				 					
# 删除所有容器
docker rm -f $(docker ps -aq)    					  
# 删除所有容器
docker ps -a -q|xargx docker rm  					 
# 删除所有停止的容器
docker container prune
#退出容器
exit						    					
#不停止退出容器
Ctrl+p+q	


#开启新的终端进入容器
docker exec -it 容器id  /bin/bash
#进入正在执行的容器，退出会停止容器
docker attach 容器id 

#拷贝容器文件到主机
docker cp 容器id:容器内路径 主机路径
```



>#### 日志

```shell
#输出日志
docker logs	容器id
#日志帮助命令
docker logs	--help
#出日志格式	
Options:
      --details        Show extra details provided to logs
  -f, --follow         Follow log output
      --since string   Show logs since timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes)
  -n, --tail string    Number of lines to show from the end of the logs (default "all")
  -t, --timestamps     Show timestamps
      --until string   Show logs before a timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes)
#容器启动时循环打印日志
docker run -d centos  /bin/sh -c "while true;do echo sunny;sleep 1;done"
#打印容器日志
docker logs -tf -tail 显示条数 容器id
```

>查看容器内进程

```
docker top 容器id 
```

>查看docker元数据

```
docker inspect 容器id
docker inspect --format "{{.state.pid}}" nginx0
```

>commit镜像

```shell
docker commit -a="创建者" -m="提交信息" 容器id 自定义镜像名称:版本号
```

>卸载docker

```shell
# 卸载依赖
yum remove docker-ce docker-ce-cli containerd.io
# 删除资源
rm -rf /var/lib/docker
rm -rf /etv/docker
#  /var/lib/docker     docker的默认工作路径！！！
```



## 常用命令

```shell
#帮助命令
docker 命令 --help					   
# docker内存使用情况
docker stats                         	  
# 更新docker
yum -y update 							
# docker信息
docker info								
#运行Docker守护进程
systemctl start docker     				  
#停止Docker守护进程
systemctl stop docker      				 
#重启Docker守护进程
systemctl restart docker   				  
#docker运行状态
systemctl status docker
```



## 容器数据卷

* 容器的持久化和同步操作

```
 docker run -it -v /主机路径:/容器路径 tomcat /bin/bash 
```

挂载信息

<img src="https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202206241434433.png" alt="image-20220624143405373" style="zoom:67%;" />

* 具名挂载与匿名挂载

```shell
#卷操作  
docker volume  
#匿名卷挂载
-v 容器内路径
#具名卷挂载
-v 自定义卷名：容器内路径
#指定路径挂载
-v /主机路径:容器路径
#查看具名卷
docker volume inpect 卷名称 
#设置卷读写权限
-v /主机路径:容器路径:ro  
ro#只读
rw#可读可写
top:一旦设置容器权限，将同步作用于挂载出来的文件

#docker容器内的卷，在没有指定目录的情况下，统一目录/var/lib/docker/volume/...

--volumes-from
#挂载容器
```

* 手动挂载

```
```



## DockerFile

```apl
构建DockerFile必须遵守
1.所有指令必须大写
2.从上到下顺序执行
3.#注释
4.每个指令都会创建一个新的镜像层
```

![image-20220606152637128](https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202206061526240.png)

###### docker build构建镜像

```shell
FROM		#基础镜镜像，一切从这里开始构建
MAINTAINER	#镜像是谁写的，姓名+邮箱
RUN			#镜像构建的时候需要运行的命令
ADD			#步骤: tomcat镜像，这个tomcat压缩包!添加内容
WORKDIR		#镜像的工作目录
VOL UME		#挂载的目录
EXPOSE		#暴露端口配置
CMD			#指定这个容器启动的时候要运行的命令,只有最后一个会生效，可被替代
ENTRYPOINT	 #指定这个容器启动的时候要运行的命令，可以追加命令
ONBUILD		#当构建一个被继承DockerFile 这个时候就会运行ONBUILD 的指令。触发指令。
COPY		#类似ADD，将我们文件拷贝到镜像中
ENV			#构建的时候设置环境变量!
```

###### CMD与ENTRYPOINT的区别

```
ENTRYPOINT
```

###### 编写DockerFile

```shell
FROM centos
MAINTAINER sunny<740568660@qq.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80

CMD echo $MYPATH
CMD	echo "-------ok-------"
CMD	echo /bin/bash
```



###### 构建DockerFile

```
docker build  -f 文件名 -t 镜像名:版本号 .
```

###### docker push上传镜像

```
```



## docker网络

```shell
#获取本机IP地址
ip addr
```

<img src="https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202206211548534.png" alt="image-20220621154839452" style="zoom: 67%;" />

```apl
docker容器之间的连接都是使用evth-pair技术（桥接模式）
容器被删除，对应的网桥也会被删除
```

#### link

```shell
-- link  #不推荐使用
```



#### docker network

```shell
docker network --help
[root@sunny ~]# docker network ls        #查看docker网络
NETWORK ID     NAME      DRIVER    SCOPE 
f4afa11cf5a2   bridge    bridge    local #默认，自定义也使用bridge桥接模式
1ee220893d0d   host      host      local #和主机共享网络
406606aaa892   none      null      local #不配置网络
```

* 创建一个自定义网络

```shell
docker network create --driver bridge  --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet(自定义网络名称)
--driver  #网络模式
--subnet  #子网
--gateway #网关
```

<img src="https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202206211035982.png" alt="image-20220621103523903" style="zoom:80%;" />

* 自定义网络源数据

<img src="https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202206211118802.png" alt="image-20220621111848687" style="zoom: 67%;" />

* connect网络连同

```shell
docker network connect 网络 容器 #将指定容器连接指定网络
								#同一网络内的容器之间可以互相通信
								#一个容器可以连同多个网络（容器的健康姓，安全性）
```



![image-20220621154155270](https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202206211541413.png)

## docker compose(容器编排)

* 安装docker compose
* github下载

>curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

* 国内镜像

>curl -L "https://get.daocloud.io/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
>
>-$(uname -s)  #操作系统
>
>-$(uname -m) #操作系统位数

*  授权，否则无操作权限

>chmod 777 docker-compose

* 版本号

>docker-compose --version







* yaml规则

```






```







## docker swarm

## docker可视化工具

* portainer

```shell
#docker安装portainer
docker run -d -p 8088:9000\
 --restart=always -v /var/run/docker.sock:/var/run/docker.sick --privileged=true portainer/portainer
```

* Rancher

```shell

```

## 练习

### docker安装rabbitmq

```shell
#启动容器
docker run -d -p 5672:5672 -p 15672:15672 --name rabbitmq -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=admin rabbitmq:3.8.16
#开启插件
docker exec -it rabbitmq rabbitmq-plugins enable rabbitmq_management



#rabbitmq有一个默认的用户名和密码，guest和guest,但为了安全考虑，该用户名和密码只允许本地访问，如果是远程操作的话，需要创建新的用户名和密码
docker run -di --name rabbitmq -p 5672:5672 -p 15672:15672 -v `pwd`/rabbitmq:/var/lib/rabbitmq --hostname myRabbit -e RABBITMQ_DEFAULT_VHOST=myvhost -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=admin rabbitmq:3.7.14-management

说明：
-i：表示运行容器
-d：在run后面加上-d参数,则会创建一个守护式容器在后台运行（这样创建容器后不会自动登录容器，如果只加-it两个参数，创建后就会自动进去容器）。
-di：后台运行容器；
--name：指定容器名；
-p：指定服务运行的端口（5672：应用访问端口；15672：控制台Web端口号）；
-v：映射目录或文件（文件共享），格式 宿主机目录：容器目录
--hostname：主机名（RabbitMQ的一个重要注意事项是它根据所谓的 “节点名称” 存储数据，默认为主机名）；
-e 指定环境变量；（RABBITMQ_DEFAULT_VHOST：默认虚拟机名；RABBITMQ_DEFAULT_USER：默认的用户名；RABBITMQ_DEFAULT_PASS：默认用户名的密码）


#
docker cp /usrupload/rabbitmq_delayed_message_exchange-3.8.17.8f537ac.ez rabbitmq:/plugins


```





* docker安装启动tomcat

> docker run -d -p 3355:8080 --name mtomcat tomcat				

* 进入tomcat容器

>docker exec -it tomcat01 /bin/bash
>cp webapps.dist/* webapps





<-------------------------------------------------------------------------------------------------->



* docker安装es+kibana

>docker run -d --name elasticsrerch01 -p 9200:9200 
>								  -p 9300:9300 
>								  -e "discovery.type=single-node"
>								  -e ES_JAVA_OPTS="-Xms64m -Xmx256m"
>                                     elasticsrerch:7.6.2

<-------------------------------------------------------------------------------------------------->

* docker安装MySQL

>docker run -d \
>--name mysql \
>-p 3306:3306 \
>-v /usr/local/docker/mysql/config/mysqld.cnf:/etc/mysql/mysql.conf.d/mysqld.cnf \
>-v /usr/local/docker/mysql/data/mysql:/var/lib/mysql \
>-e MYSQL_ROOT_PASSWORD=root\
>mysql:5.7
>
>```
>docker run -p 3306:3306 --name mysql \
>-v /data/mysql/log:/var/log/mysql \
>-v /data/mysql/data:/var/lib/mysql \
>-v /data/mysql/conf:/etc/mysql \
>-e MYSQL_ROOT_PASSWORD=root \
>-d mysql:5.7
>```



<-------------------------------------------------------------------------------------------------->

* docker安装nacos

>	docker  run --name nacos -d -p 8848:8848 --privileged=true --restart=always -e JVM_XMS=256m -e JVM_XMX=256m -e MODE=standalone -e PREFER_HOST_MODE=hostname nacos/nacos-server



<-------------------------------------------------------------------------------------------------->

* docker安装redis

>docker run -p 6379:6379 --name myRedis  -v /data/redis/redis.conf:/etc/redis/redis.conf -v /data/redis/data:/data -d redis redis-server /etc/redis/redis.conf --appendonly yes



* 模拟安装redis集群

```shell







```







### 阿里云服务器与docke虚拟网段冲突问题

```shell
touch /etc/docker/daemon.json

#编辑{"bip": "192.168.1.5/24"}
#重启docke
systemctl restart docker
```

<img src="https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202301091013822.png" alt="image-20230109101351717" style="zoom:67%;" />

































