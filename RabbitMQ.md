# rabbitmq

### windows环境安装

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
#安装web插件
rabbitmq-plugins enable rabbitmq_management
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



### rabbitmq支持消息的模式

#### 1.简单模式Simple

AMQP协议，基于HTTP协议

做最简单的事情，一个生产者对应一个消费者，[RabbitMQ](https://so.csdn.net/so/search?q=RabbitMQ&spm=1001.2101.3001.7020)相当于一个消息代理，负责将A的消息转发给B
**应用场景**：将发送的电子邮件放到[消息队列](https://so.csdn.net/so/search?q=消息队列&spm=1001.2101.3001.7020)，然后邮件服务在队列中获取邮件并发送给收件人

<img src="https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202301101709568.png" alt="image-20230110170915476"  />





#### 2.工作模式work

在多个消费者之间分配任务（竞争的消费者模式），一个生产者对应多个消费者，一般适用于执行资源密集型任务，单个消费者处理不过来，需要多个消费者进行处理
应用场景：一个订单的处理需要10s，有多个订单可以同时放到消息队列，然后让多个消费者同时处理，这样就是并行了，而不是单个消费者的串行情况

![image-20230110170945443](https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202301101709500.png)



#### 3.发布订阅模式fanout

一次向许多消费者发送消息，一个生产者发送的消息会被多个消费者获取，也就是将消息将广播到所有的消费者中。
应用场景：更新商品库存后需要通知多个缓存和多个数据库，这里的结构应该是：

一个fanout类型交换机扇出两个个消息队列，分别为缓存消息队列、数据库消息队列
一个缓存消息队列对应着多个缓存消费者
一个数据库消息队列对应着多个数据库消费者

![image-20230110171112159](https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202301101711212.png)





#### 4.路由模式direct

有选择地（Routing key）接收消息，发送消息到交换机并且要指定路由key ，消费者将队列绑定到交换机时需要指定路由key，仅消费指定路由key的消息
应用场景：如在商品库存中增加了1台iphone12，iphone12促销活动消费者指定routing key为iphone12，只有此促销活动会接收到消息，其它促销活动不关心也不会消费此routing key的消息

![image-20230110171315411](https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202301101713471.png)

#### 5.主题模式

根据主题（Topics）来接收消息，将路由key和某模式进行匹配，此时队列需要绑定在一个模式上，`#`匹配一个词或多个词，`*`只匹配一个词。
**应用场景**：同上，iphone促销活动可以接收主题为iphone的消息，如iphone12、iphone13等

![image-20230110171901268](https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202301101719327.png)







#### 6.参数模式headers	

如果我们需要在远程计算机上运行功能并等待结果就可以使用RPC，具体流程可以看图。
**应用场景**：需要等待接口返回数据，如订单支付

<img src="https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202301101719724.png" alt="image-20230110171924655" style="zoom:80%;" />





### 各端口作用

| 端口       | 作用                                                 |
| ---------- | ---------------------------------------------------- |
| 15672      | 管理界面ui使用的端口                                 |
| 15671      | 管理监听端口                                         |
| 5672，5671 | AMQP 0-9-1 without and with TLSclient端通信口        |
| 4369       | （epmd)epmd代表 Erlang端口映射守护进程，erlang发现口 |
| 25672      | ( Erlang distribution） server间内部通信口           |

​	

### 常用命令

```shell
#启动容器
docker run -d -p 5672:5672 -p 15672:15672 --name rabbitmq -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=admin rabbitmq:3.8.16
#开启插件
docker exec -it rabbitmq rabbitmq-plugins enable rabbitmq_management
#列出rabbitmq插件的启用和禁用状态
rabbitmq-plugins list
#访问rabbitmq web管理页时会出现无法访问，这是因为没有开启插件
rabbitmq-plugins enable rabbitmq_management
#禁用rabbitmq-managementweb管理页插件
rabbitmq-plugins disable rabbitmq_management

#rabbitmq有一个默认的用户名和密码，guest和guest,但为了安全考虑，该用户名和密码只允许本地访问，如果是远程操作的话，需要创建新的用户名和密码
docker run -di --name rabbitmq -p 5672:5672 -p 15672:15672 -v `pwd`/rabbitmq:/var/lib/rabbitmq --hostname myRabbit -e RABBITMQ_DEFAULT_VHOST=myvhost -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=admin rabbitmq:3.7.14-management
top：
-i：表示运行容器
-d：在run后面加上-d参数,则会创建一个守护式容器在后台运行（这样创建容器后不会自动登录容器，如果只加-it两个参数，创建后就会自动进去容器）。
-di：后台运行容器；
--name：指定容器名；
-p：指定服务运行的端口（5672：应用访问端口；15672：控制台Web端口号）；
-v：映射目录或文件（文件共享），格式 宿主机目录：容器目录
--hostname：主机名（RabbitMQ的一个重要注意事项是它根据所谓的 “节点名称” 存储数据，默认为主机名）；
-e 指定环境变量；（RABBITMQ_DEFAULT_VHOST：默认虚拟机名；RABBITMQ_DEFAULT_USER：默认的用户名；RABBITMQ_DEFAULT_PASS：默认用户名的密码）

#安装RabbitMQ并安装延时队列插件
官网下载对应版本
#从主机将文件拷贝至容器内rabbitmq:/plugins
docker cp /usrupload/rabbitmq_delayed_message_exchange-3.8.17.8f537ac.ez rabbitmq:/plugins
#在容器内plugins目录下，查看插件是否上传成功
ls -l|grep delay
#在容器内plugins目录下，启动延时队列插件
rabbitmq-plugins enable rabbitmq_delayed_message_exchange
```



### 源码学习

### RabbitAdmin 与 RabbitTemplate 使用

>## RabbitAdmin
>
>* 该类封装了对 RabbitMQ 的管理操作
>
>```java
>@Bean
>public RabbitAdmin rabbitAdmin(ConnectionFactory connectionFactory){
>    return new RabbitAdmin(connectionFactory);
>}
> 
>@Autowired
>private RabbitAdmin rabbitAdmin;
>```
>
>
>
>* Exchange 操作
>
>1. 创建四种类型的 Exchange，均为持久化，不自动删除
>2. rabbitAdmin.declareExchange(new DirectExchange("direct.exchange",true,false));
>3. rabbitAdmin.declareExchange(new TopicExchange("topic.exchange",true,false));
>4. rabbitAdmin.declareExchange(new FanoutExchange("fanout.exchange",true,false));
>5. rabbitAdmin.declareExchange(new HeadersExchange("header.exchange",true,false));
>6. 删除 Exchange
>7. rabbitAdmin.deleteExchange("header.exchange");
>
>
>
>* Queue 操作
>
>1. 定义队列，均为持久化
>2. rabbitAdmin.declareQueue(new Queue("debug",true));
>3. rabbitAdmin.declareQueue(new Queue("info",true));
>4. rabbitAdmin.declareQueue(new Queue("error",true));
>5. //删除队列      rabbitAdmin.deleteQueue("debug");
>6. //将队列中的消息全消费掉
>7. rabbitAdmin.purgeQueue("info",false);
>
>- Binding 绑定
>
>```java
>//绑定队列到交换器，通过路由键
>rabbitAdmin.declareBinding(new Binding("debug",Binding.DestinationType.QUEUE,
>        "direct.exchange","key.1",new HashMap()));
> 
>rabbitAdmin.declareBinding(new Binding("info",Binding.DestinationType.QUEUE,
>        "direct.exchange","key.2",new HashMap()));
> 
>rabbitAdmin.declareBinding(new Binding("error",Binding.DestinationType.QUEUE,
>        "direct.exchange","key.3",new HashMap()));
> 
>//进行解绑
>rabbitAdmin.removeBinding(BindingBuilder.bind(new Queue("info")).
>        to(new TopicExchange("direct.exchange")).with("key.2"));
> 
>//使用BindingBuilder进行绑定
>rabbitAdmin.declareBinding(BindingBuilder.bind(new Queue("info")).
>        to(new TopicExchange("topic.exchange")).with("key.#"));
> 
>//声明topic类型的exchange
>rabbitAdmin.declareExchange(new TopicExchange("exchange1",true,false));
>rabbitAdmin.declareExchange(new TopicExchange("exchange2",true,false));
> 
>//exchange与exchange绑定
>rabbitAdmin.declareBinding(new Binding("exchange1",Binding.DestinationType.EXCHANGE,
>        "exchange2","key.4",new HashMap()));
>```



>## RabbitTemplate
>
>* Spring AMQP 提供了 RabbitTemplate 来简化 RabbitMQ 发送和接收消息操作
>
>```java
>@Autowired
>private RabbitAdmin rabbitAdmin;
>```
>
>#### 发送消息
>
>- send （自定义消息 Message）
>
>1. Message message = new Message("hello".getBytes(),new MessageProperties());
>2. // 发送消息到默认的交换器，默认的路由键
>3. rabbitTemplate.send(message);
>4. // 发送消息到指定的交换器，指定的路由键
>5. rabbitTemplate.send("direct.exchange","key.1",message);
>6. // 发送消息到指定的交换器，指定的路由键
>7. rabbitTemplate.send("direct.exchange","key.1",message,new CorrelationData(UUID.randomUUID().toString()));
>
>* convertAndSend（自动 Java 对象包装成 Message 对象，Java 对象需要实现 Serializable 序列化接口）
>
>```java
>User user = new User("linyuan");
>// 发送消息到默认的交换器，默认的路由键
>rabbitTemplate.convertAndSend(user);
>// 发送消息到指定的交换器，指定的路由键，设置消息 ID
>rabbitTemplate.convertAndSend("direct.exchange","key.1",user,new CorrelationData(UUID.randomUUID().toString()));
>// 发送消息到指定的交换器，指定的路由键，在消息转换完成后，通过 MessagePostProcessor 来添加属性
>rabbitTemplate.convertAndSend("direct.exchange","key.1",user,mes -> {
>    mes.getMessageProperties().setDeliveryMode(MessageDeliveryMode.NON_PERSISTENT);
>        return mes;
>});
>```
>
>#### 接收消息
>
>- receive（返回 Message 对象）
>
>1. // 接收来自指定队列的消息，并设置超时时间
>
>2. Message msg = rabbitTemplate.receive("debug",2000l);
>
>   
>
>- receiveAndConvert（将返回 Message 转换成 Java 对象）
>
>```java
>User user = (User) rabbitTemplate.receiveAndConvert();
>```
>
>

>## AmqpTemplate	
>
>AmqpTemplate
>
>  就像Spring Framework和其它一些项目提供了一些高度抽象，Spring AMQP提供的‘template’扮演者关键的角色。定义者主要操作的接口是AmqpTemplate。这些操作包含了发送和接收消息的一般操作。换种说法，它们不是某个实现所专有的，所以AMQP存在于名称里。这个接口的实现与AMQP协议的实现紧密关联。不像JMS，本身是一个接口级别的API，而AMQP是一个wire-level的协议。这个协议的实现必须提供他们自己的客户端类库，所以模板接口的每一个实现必须依赖一个特定的客户端类库。到目前为止，仅仅存在一个实现：RabbitTemplate。在下面的例子中，你将看到AmqpTemplate的使用，但是当你看到例子的配置，或者一些代码template的初始化或者设定的时候，你会看到实现类型RabbitTemplate。
