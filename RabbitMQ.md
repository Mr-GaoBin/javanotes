# RabbitMQ

erlang环境安装

```
变量名:ERLANG_HOME
变量值:erlang安装地址(C:\Program Files\erl-24.2)
```

```
系统变量path
变量值:%ERLANG_HOME%\bin
```

```
erl 				-- 查看版本号erlang安装成功
```

RabbitMQ安装

```
sbin  				-- 打开RabbitMQ的sbin目录
```

```
rabbitmq-plugins enable rabbitmq_management
					-- 安装插件
```

```
rabbitmqctl status   -- 检测版本号
```

```
rabbitmq-server.bat  or net start rabbitmq
					-- 点击启动服务
```

```
默认用户名和密码都是guest
```

```
http://localhost:15672/ 
```

>rabbitmq支持消息的模式

1.简单模式Simple

AMQP协议，基于HTTP协议

2.工作模式

work

3.发布订阅模式

fanout

4.路由模式

direct

5.主题模式

6.参数模式

headers	
