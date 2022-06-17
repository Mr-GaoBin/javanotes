# linux

1.一切皆文件(文件：读写，执行，权限（用户，用户组）)

2."/" : 所有文件都挂载在/下

```apl
bin (binaries)存放二进制可执行文件
sbin (super user binaries)存放二进制可执行文件，只有root才能访问
etc (etcetera)存放系统配置文件
usr (unix shared resources)用于存放共享的系统资源
home 存放用户文件的根目录
root 超级用户目录
dev (devices)用于存放设备文件
lib (library)存放跟文件系统中的程序运行所需要的共享库及内核模块
mnt (mount)系统管理员安装临时文件系统的安装点
boot 存放用于系统引导时使用的各种文件
tmp (temporary)用于存放各种临时文件
var (variable)用于存放运行时需要改变数据的文件
```

3."|": 管道符号

```apl
使用管道操作符“|”可以把一个命令的标准输出传送到另一个命令的标准输入中，连续的|意味着第一个命令的输出为第二个命令的输入，第二个命令的输入为第一个命令的输出，依次类推。
```

>### 主机参数

```shell
# #超级管理员
# @普通用户
[当前用户@主机名称 所在位置]
#查看主机名称
hostname
#设置主机名称（需重启有可能失效）
hostname 自定义名称
#root用户设置主机名称（需重启，永久生效）
hostnamectl set-hostname mycomputer
```

​	

>### 开关机命令

```shell
lscpu     #cpu信息
hostname  #本机用户名
cat /etc/sellinux/config
cat /etc/redhat-release   #查看centos版本信息
sync	 #将数据由内存同步到硬盘中。
shutdown #关机指令，你可以man shutdown 来看一下帮助文档。例如你可以运行如下命令关机:
shutdown -h 10 #这个命令告诉大家，计算机将在10分钟后关机
shutdown -h now #立马关机
shutdown -h 20:25 #系统会在今天20:25关机
shutdown -h +10 #十分钟后关机
shutdown -r now #系统立马重启
shutdown -r +10 #系统十分钟后重启
reboot #就是重启，等同于shutdown -r now
halt #关闭系统，等同于shutdown -h now和poweroff
最后总结一下,不管是重启系统还是关闭系统，首先要运行sync命令,把内存中的数据写到磁盘中。
```

>### CD 命令 切换目录

```
reboot                                         --重启服务器
clear                                          -- 清空
ls /									    --查看根目录下所有文件
cd  dir									    -- 进入指定文件夹
cd..									    -- 向前退一级
cd ~									    -- 进入家目录，不同身份进入文件夹也不同root/home
mkdir dir                                      -- 创建文件夹
mv 文件名 文件夹名							   -- 移动指定文件到指定文件夹
```

>### pwd显示当前用户所在目录

```
[root@kuangshen ~]# pwd
/ root
[root@kuangshen ~]# cd /bin
[root@kuangshen bin]# pwd
/bin
[root@kuangshen bin]# cd /usr/1oca1
[root@kuangshen 1oca1]# pwd
/usr/1oca1
```

>### 移除文件夹

```
cp 源文件 目标文件                              -- 拷贝文件(文件夹需加 -r)
				---> cp -r /sunny/java/jdk-8u202-linux-x64.rpm /sunny/mysql
rm  dir										-- 删除指定空目录
rm -p dir 									-- 递归删除多个目录
rm -rf /  									-- 删除系统所有文件，删库跑路
-f 忽略不存在的文件，不警告
-r 递归删除目录
-i 删除询问是否删除
```

>### 查找文件

```
find/ -文件名                                   -- 根据文件名称查找文件

-------------------------------查询jdk安装位置---------------------------------
1.首先确定系统上面的确有jdk,否则一切都是白搭
	--> java -version
2. 查找java命令的位置
	--> which java
3.查找列出该链接所指向的原始文件或目录
	--> ls -l /usr/bin/java
4.查找目录
	--> ls -l /etc/alternatives/java
```

>### Ln 链接

```
touch dir1                         --创建文件dir1
ehco "内容" >>dir      			-- 给dir写入内容            
ln dir1 dir2					 -- 创建硬链接dir2
ln -s dir1 dir3					 -- 创建软链接dir3
cat dir							-- 查看dir
```

>### vim编辑器

```
vim 文件          				    --编辑指定文件    
i  									-- 进入编辑模式
esc									-- 退出编辑模式
:									-- 底线命令模式
w									--保存
q									--退出   <--!建议q,w一起使用:保存并退出-->
```

>## ps

```
ps -a                    			   -- 显示当前终端运行的所有进程信息
ps -x								 -- 以用户的信息显示进程
ps -u    			                   -- 显示后台运行进程的参数
ps -aux|grep               			   -- 查找文中符合条件的字符串
ps -ef                  			   -- 查看父进程信息
ps -p								 -- 查看父id
ps -u								 -- 查看用户组
pstree -pu						      -- 进程数
kill -9 进程id						--强制结束进程
```

>### 环境变量命令

```
vim /etc/profile					 --编辑环境变量
cat /etc/profile					 --查看环境变量
```

>### 环境安装

```
安装软件的三种方式:
- rpm
- tar
- yum(docker)
```

## 环境安装

### java环境安装

```
java -version    					  -- 检测当前系统是发存在java环境
rpm -qa|grep jdk	`		          -- 检测jdk版本信息
rpm -e --nodeps  jdk                   -- 强制删除 
rpm -ivh jdk-8u202-linux-x64.rpm	   -- 安装(.rpm)
tar -zxvf name						 --解压(.tar)
semanage port -l | grep ssh
```

yum安装

```
yum -y list java*					-- 查看可安装的jdk版本
yum install -y jdk版本			   -- 选择合适jdk安装
```

手动安装

```
# 解压到安装目录
tar -zxvf /usr/local/jdk-8u321-linux-x64.tar.gz
#默认解压到/usr/java 拷贝文件夹到/usr/local/java
cp -r /usr/java /usr/local
# 重命名
mv /usr/local/jdk-8u321-linux-x64 /usr/local/java/jdk1.8
#配置环境变量
vim /etc/profile
插入java环境变量
export JAVA_HOME=/usr/local/jdk/jdk1.8
export CLASSPATH=$:CLASSPATH:$JAVA_HOME/lib/
export PATH=$PATH:$JAVA_HOME/bin
# 使环境变量生效
source /ect/profile
# 查看是否安装成功
java -version
```

修改/etc/profile失败补救方法（[Linux -- 04 -- 修改/etc/profile失败补救方法_Gene Xu的博客-CSDN博客](https://blog.csdn.net/Goodbye_Youth/article/details/84375225)）

```
一、修改 /etc/profile
由于相关命令都没法使用，因此我们无法通过 vim 来修改配置文件，但我们可以通过绝对命令来修改：/bin/vim /etc/profile，然后保存退出
二、重置 PATH
正常修改完配置文件后，我们需要通过 source /etc/profile 命令来重新加载配置文件，但此时 source 命令同样用不了，因此我们需要重置 PATH
PATH的初始值为: /usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin，将 PATH 的值重置为该值即可
export PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin
export PATH=/usr/bin:/usr/sbin:/bin:/sbin:/usr/X11R6/bin
三、重新加载配置文件
经过上面两步之后，相关命令又可以正常使用了
此时我们再使用 source /etc/profile 命令来重新加载配置文件，至此就大功告成了
```

### nginx安装

常用命令

```apl
wget  nginx-1.20.2.tar.gz				   -- 下载nginx
tar -zxvf  nginx-1.20.2.tar.gz              -- 解压
whereis nginx                               -- 检测本机是否有nginx 


./configure
make								    -- 配置nginx
make install

cd sbin
./nginx                                     -- 启动nginx服务器
./nginc -s quit							 -- 安全退出
./nginc -s stop                             -- 停止nginx服务器
./nginc -s reload                           -- 重新加载配置文件
ps -aux|grep nginx                          -- 查看nginx进程

```

配置文件usr/local/nginx/conf/nginx.conf

```apl
    server {
        upstream upname{
        #配置负载均衡    #weight 加权轮询
			server 162.14.78.63:8080 weight=1
			server 162.14.78.63:8081 weight=2
        }
    
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
             #配置代理
            proxy_pass http://upname
        }
    }
```

### mysql环境安装

```shell
#解压
tar -zxvf mysql5.5.tar.gz
#重命名
mv  mysql-5.6.50-linux-glibc2.12-x86_64 mysql
mv mysql /usr/local
#配置用户和组
useradd -r -s /sbin/nologin mysql
#更改/usr/loca1/mysq1目录权限，更改文件拥有者与所属组都必须为mysql
chown -R mysql.mysql /usr/local/mysql
#mysql安装目录下初始化
/usr/local/mysql/scripts/mysql_install_db --user=mysql
#移除mariadb-libs库文件
#CentOS7操作系统默认安装mariadb-libs库,库会影响mysql的初始化
yum remove mariadb-libs(y/n):y
#配置全局命令
cp support-files/mysql.server /etc/init.d/mysql
#启动
service MySQL start
#设置账户密码
bin/mysqladmin -u root password 'root' 
#登录
bin/mysql -uroot -p
password:enter
#查看现有用户,密码及允许连接的主机
 SELECT User, Password, Host FROM user;
+------+-------------------------------------------+-----------+
| User | Password                                  | Host      |
+------+-------------------------------------------+-----------+
| root | *81F5E21E35407D884A6CD4A731AEBFB6AF209E1B | localhost |
| root |                                           | sunny     |
| root |                                           | 127.0.0.1 |
| root |                                           | ::1       |
|      |                                           | localhost |
|      |                                           | sunny     |
+------+-------------------------------------------+-----------+
#授权，允许所有访问
GRANT ALL PRIVILEGES ON *.* TO 'root' @'%'IDENTIFIED BY 'root' WITH GRANT OPTION;
#查看用户是否授权
select*from mysql.user where user='root'\G;
#远程登陆 
#-P  端口
#-p  用户密码
mysql -h 162.14.78.63 -P 3306 -u root -proot
#查看是否区分大小写
show variables like "%case%";
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| lower_case_file_system | OFF   |
| lower_case_table_names | 0     |
+------------------------+-------+
#lower_case_table_names=1（说明是不区分大小写的）
#lower_case_table_names=0（说明区分大小写的）
#拷贝配置文件到/etc/my.cnf
cp usr/local/mysq/my.cnf /etc/my.cnf
#编辑配置文件，添加配置
vim /etc/my.cnf
lower_case_table_names = 1
#重启MySQL
service mysql restart 
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| lower_case_file_system | OFF   |
| lower_case_table_names | 1     |
+------------------------+-------+
```

卸载mysql

```shell
 #过滤MySQL线程
 rpm -qa | grep -i mysql
 #MySQL运行状态
 service mysql status
 #停止MySQL服务
 service mysql stop
 #查找关于MySQL的文件
 find / -name mysql
```

### tomcat环境安装

>Linux提供了不同的工具来通过不同类型的协议(例如HTTP，FTP，HTTPS等)下载文件。wget是用于通过命令行界面下载文件的最受欢迎的工具。 Wget受Linux，BSD，Windows，MacOSX支持。 Wget具有丰富的功能集，其中一些可以列出
>
>- Resume downloads
>
>  继续下载
>
>- Multiple file download single command
>
>  多文件下载单个命令
>
>- Proxy support
>
>  代理支持
>
>- Unattended download
>
>  无人值守下载

下载tomcat压缩包

```
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.60/bin/apache-tomcat-9.0.60.tar.gz        
```

解压tomcat

```
tar -zxvf apache-tomcat-9.0.8.tar.gz 
```

启动tomcat

```
cd apache-tomcat-9.0.60									-- 进入解压后的tomcat文件架
cd bin												   -- 进入目录目录
./startup.sh								            -- 启动
./startup.sh										   -- 停止
```

查看防火墙当前开启了那些端口

```
firewall-cmd --state									 -- 防火墙状态
systemctl start firewalld								 -- 开启防火墙
firewall-cmd --zone=public --add-port=6379/tcp --permanent  --开启端口
systemctl restart firewal1d.service                        -- 重启防火墙
firewall-cmd --reload									-- 重启防火墙
firewall-cmd --list-ports								--查看防火墙当前开启了那些端口


lsof -i:端口号                                             -- 当前被占用的端口
kill -9 PID                                                -- 杀掉进程
```

### 用户和组

```shell
#选项:
-Ccomment指定一段往释性描述。
-d目录指定用户主目录，如果此目录不存在，则同时使用-m选项， 可以创建主目录。
-g用户组指定用户所属的用户组。
-G用户组，用户组指定用户所属的附加组。
-m使用者目录如不存在则自动建立。
-s Shell文件指定用户的登录Shell。
-u用户号指定用户的用户号，如果同时有-o选项，则可以重复使用其他用户的标识号。
#用户名:
指定新账号的登录名
#查看所有用户组
cat /etc/group
#查看指定用户组信息
id usernane
#添加用户
useradd -m usernane
#删除用户
userdel -r usernane
#修改用户
usermod usernane
#切换用户
su usernane
#给当前用户设置密码
passwd
#给指定用户设置密码
passwd usernane
#锁定用户
passwd -l usernane
#置空密码拒绝登录
passwd -d usernane
```

## 宝塔面板

一键安装宝塔面板

```
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh
```

获取宝塔Linux面板登陆地址账号和密码

```
/etc/init.d/bt default
```

清除登陆限制

```
rm -f /www/server/panel/data/*.login
```

修改密码

```
cd /www/server/panel && python tools.py panel testpasswd
```

