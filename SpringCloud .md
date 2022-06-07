# SpringCloud

## 服务注册中心

>###  CAP 理论
>
>C（Consistency）：数据一致性。大家都知道，分布式系统中，数据会有副本。由于网络或者机器故障等因素，可能有些副本数据写入正确，有些却写入错误或者失败，这样就导致了数据的不一致了。而满足数据一致性规则，就是保证所有数据都要同步。
>A（Availability）：可用性。我们需要获取什么数据时，都能够正常的获取到想要的数据（当然，允许可接受范围内的网络延迟），也就是说，要保证任何时候请求数据都能够正常响应。
>P（Partition Tolerance）：分区容错性。当网络通信发生故障时，集群仍然可用，不会因为某个节点挂了或者存在问题，而影响整个系统的正常运作。
>
>### CAP 理论"三选一"
>
>**对于一个大规模分布式数据系统来说， CAP 三要素不可兼得**， 同一个系统至多只能实现其中的两个， 而必须放宽第3 个要素来保证其他两个要素被满足。



>### eureka 的 AP 原则
>
>​	自我保护机制
>
>### Zookeeper 的 CP 原则
>
>但是 zookeeper 也有个缺陷，刚刚提到了 leader 节点，当 master 节点因为网络故障与其他节点失去联系时，剩余节点会重新进行 leader 选举。问题在于，选举 leader 的时间太长，30 ~ 120s, 且选举期间整个 zookeeper 集群都是不可用的，这就导致在选举期间注册服务瘫痪。





| eureka    |      |      |      |
| --------- | ---- | ---- | ---- |
| Zookeeper |      |      |      |
| Consul    |      |      |      |
| Nacos     |      |      |      |





## 负载均衡

>### Ribbon
>
>### LoadBlancer
>
>​	负载均衡LB，在微服务或分布式中经常用的一种应用
>
>​	负载均衡简单的说就是将用户的请求平摊的分配到多个服务上，从而达到系统的HA (高可用)
>​	常见的负载均衡软件有Nginx, LVS，apache+tomcat 等等
>​	dubbo，SpringCloud中均给我们提供 了负载均衡，SpringCloud的负载均衡算法可以自定义 .
>
>负载均衡简单分类:











## 服务调用

>### Feige
>
>
>
>### openFeige
>
>### 	

## 服务降级

```java
public @interface HystrixCommand {
            // HystrixCommand 命令所属的组的名称：默认注解方法类的名称
            String groupKey() default "";
            // HystrixCommand 命令的key值，默认值为注解方法的名称
            String commandKey() default "";
            // 线程池名称，默认定义为groupKey
            String threadPoolKey() default "";
            // 定义回退方法的名称, 此方法必须和hystrix的执行方法在相同类中
            String fallbackMethod() default "";
            // 配置hystrix命令的参数
            HystrixProperty[] commandProperties() default {};
            // 配置hystrix依赖的线程池的参数
             HystrixProperty[] threadPoolProperties() default {};
            // 如果hystrix方法抛出的异常包括RUNTIME_EXCEPTION，则会被封装HystrixRuntimeException异常。
    	    // 我们也可以通过此方法定义哪些需要忽略的异常
            Class<? extends Throwable>[] ignoreExceptions() default {};
            // 定义执行hystrix observable的命令的模式，类型详细见ObservableExecutionMode
            ObservableExecutionMode observableExecutionMode() default ObservableExecutionMode.EAGER;
            // 如果hystrix方法抛出的异常包括RUNTIME_EXCEPTION，则会被封装HystrixRuntimeException异常。
    	    // 此方法定义需要抛出的异常
            HystrixException[] raiseHystrixExceptions() default {};
            // 定义回调方法：但是defaultFallback不能传入参数，返回参数和hystrix的命令兼容
            String defaultFallback() default "";
        }
```



>* ### Hystrixc
>
>  feign.hystrix.enabled=true
>
>  fallbackMethod：标记的是捕获异常时需要执行的方法，方法名称跟value值要一样，我这里是errMethod
>
>* ### Resilience4j
>
>* ### Sentienl

## 服务网关

> * ### Zuul
>
> * ### Zuul2
>
> * ### Geteway

## 服务配置

>* ### Config
>
>* ### Nacos

## 服务总线

>* ### Bus
>
>* ### Nacos



# Spring Cloud Alibaba

```
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-dependencies</artifactId>
            <version>2.2.7.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

## 版本管理规范

```
项目的版本号格式为 x.x.x 的形式，其中 x 的数值类型为数字，从 0 开始取值，且不限于 0~9 这个范围。项目处于孵化器阶段时，第一位版本号固定使用 0，即版本号为 0.x.x 的格式。

由于 Spring Boot 1 和 Spring Boot 2 在 Actuator 模块的接口和注解有很大的变更，且 spring-cloud-commons 从 1.x.x 版本升级到 2.0.0 版本也有较大的变更，因此我们采取跟 SpringBoot 版本号一致的版本:

1.5.x 版本适用于 Spring Boot 1.5.x
2.0.x 版本适用于 Spring Boot 2.0.x
2.1.x 版本适用于 Spring Boot 2.1.x
2.2.x 版本适用于 Spring Boot 2.2.x
2021.x 版本适用于 Spring Boot 2.4.x
```

## 社区相关开源

```
```

