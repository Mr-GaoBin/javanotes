# Spring Cloud Alibaba

## NAcos服务注册与发现

* shell命令

```apl
##单机启动
startup.sh -m standalone
```

* yaml配置

```apl
#服务注册与发现
@EnableDiscoveryClient

#配置
spring:
  application:
    name: nacos-discovery-consumer # 服务名称
  cloud:
    nacos:
      discovery:
        server-addr: 127.0.0.1:8848 #注册中心地址
```



## 服务调用

>#### LoadBalancerClient与@LoadBalancer不能单独使用，二者会起冲突

* RestTemplate

>指定api地址调用

```java
#注入RestTemplate接口
 @Autowired
 private RestTemplate restTemplate;

restTemplate.getForObject("http://localhost:8080/NaCosProvidre?str="+str,String.class);
```



* LoadBalancerClient

>服务名称获取api地址

```java
  #注入LoadBalancerClient接口
  @Autowired
  private LoadBalancerClient loadBalancerClient;

      ServiceInstance serviceInstance = loadBalancerClient.choose("nacos-discovery-providre");
      URI url = serviceInstance.getUri();
	  restTemplate.getForObject(url+"/NaCosProvidre?str="+str,String.class);
```



* LoadBalanced

>直用服务名称调用

bean

```java
@Configuration
public class resttempmlateconfig {
    @Bean
    @LoadBalanced
    public RestTemplate createRestTemplate(){
        return new RestTemplate();
    }
}
```

controller

```java
restTemplate.getForObject("http://nacos-discovery-providre/NaCosProvidre?str="+str,String.class);
```



* OpenFengn

>通过服务名称将服务注入到本地直接调用

service

```java
@Component
@FeignClient(value = "nacos-discovery-providre")
public interface FeignServicec {
    @GetMapping("/NaCosProvidre/{name}")
    public String sendStr(@RequestParam("name") String str);
}
```

controller

```java
注入接口直接调用  
@Autowired
private FeignServicec feignServicec;
```



* webflux

>
>
>

```
```





## Nacos配置持久化

>application.properties

```properties
#配置mysql数据源
spring.datasource.platform=mysql
db.num=1
db.url.0=jdbc:mysql://127.0.0.1:3306/nacos?characterEncoding=utf8&serverTimezone=UTC&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
db.user=root
db.password=root
```



>nacos-mysql.sql
>
>​	-->在主机创建nacos-database,执行nacos-mysql.sql文件





## Nacos负载均衡



>nacos封装了ribben，只需在服务调用bean方法上添加@LoadBalanced注解，在客户端进行轮询转发，轮询算法，可在nacos注册中心配置轮询权重，也可在程序中实现IRole接口自定义算法

![image-20220630093235321](https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202206300932398.png)



## Sentienl熔断降级

> 源码地址:[ https://github.com/alibaba/Sentinel ](https://github.com/alibaba/Sentinel ) 
>
>下载地址:[https://github.com/alibaba/Sentinel/releases](https://github.com/alibaba/Sentinel/releases)



* 根据资源名称限流

```



```



* 根据URL限流



* 自适应限流

>





* 热点限流

>









* 黑白名单控制



* sentinel规则持久化

file持久化

```yaml
spring:
	sentinel:
		datasource:
        	ds1:
          		file:
                    file: classpath:rule.json
                    data-type: json
                    rule-type: FLOW
```

rule.json

```json
[
  {
    "resource": "fileRule",
    "count": 1,
    "grade": 1,
    "limitApp": "default",
    "strategy": 0,
    "controlBehavior": 0
  }
]
```



nacos持久化

```yaml
spring:
	sentinel:
		datasource:
   			ds2:
          		nacos:
                    server-addr: localhost:8848
                    dataId: sentinel-core-exampl
                    groupId: DEFAULT_GROUP
                    data-type: json
                    rule-type: flow
```



## NAcos配置



* 配置文件优先级(由高到低)

>bootstrap.properties -> bootstrap.yml -> application.properties -> application.yml

```yaml
#类上添加可实现动态刷新配置文件
@RefreshScope
```



* 从Nacos获取配置中心的配置文件

```yaml
# 应用名称
spring:
  config:
    import:
      - optional:nacos:nacos-config-client.yml
```



* 从classpath下获取配置文件

```yaml
spring:
  config:
    import:
    # 导入classpath下default目录下的default.properties配置文件
    - classpath:/default/default.properties
    # 导入classpath下service目录下的service.yml配置文件
    - classpath:/service/service.yml
```



* 从系统目录下配置配置文件

```yaml
spring:
  config:
    import:
    # 导入系统目录/Users/yuqiyu/Downloads下的system.properties配置文件
    - optional:/Users/yuqiyu/Downloads/system.properties
```





## getaway网关

* 配置

```yaml
# 应用名称
spring:
  application:
    name: nacos-discovery-gateway-server
  cloud:
    gateway:
      routes:
        - id: nacos-discovery-providre
          uri: lb://nacos-discovery-providre
          predicates:
            - Path=/NaCosProvidre/*
          filters:
            - PrefixPath=/app
```





## seata实现分布式事务

>Seata是一款开源的分布式事务解决方案，致力于在微服务架构下提供高性能和简单
>易用的分布式事务服务。

