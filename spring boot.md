# spring boot

### 一，启动注解

```java
@SpringBootApplication
查看源码可发现，@SpringBootApplication是一个复合注解，包含了@SpringBootConfiguration，@EnableAutoConfiguration，@ComponentScan这三个注解
@SpringBootConfiguration 注解，继承@Configuration注解，主要用于加载配置文件
@SpringBootConfiguration继承自@Configuration，二者功能也一致，标注当前类是配置类， 并会将当前类内声明的一个或多个以@Bean注解标记的方法的实例纳入到spring容器中，并且实例名就是方法名。

@EnableAutoConfiguration 注解，开启自动配置功能
@EnableAutoConfiguration可以帮助SpringBoot应用将所有符合条件的@Configuration配置都加载到当前SpringBoot创建并使用的IoC容器。借助于Spring框架原有的一个工具类：SpringFactoriesLoader的支持，@EnableAutoConfiguration可以智能的自动配置功效才得以大功告成

@ComponentScan 注解，主要用于组件扫描和自动装配
@ComponentScan的功能其实就是自动扫描并加载符合条件的组件或bean定义，最终将这些bean定义加载到容器中。我们可以通过basePackages等属性指定@ComponentScan自动扫描的范围，如果不指定，则默认Spring框架实现从声明@ComponentScan所在类的package进行扫描，默认情况下是不指定的，所以SpringBoot的启动类最好放在root package下。
```

### 

















































## 集成Swagger

### 依赖

```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.2.2</version>
</dependency>

<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.2.2</version>
</dependency>
```

### 配置

```java
@Confi guration 
@Enab1 eSwagger2
//开启Swagger2 
public class SwaggerConfig {

    
    
    
    
    
    
}

//构造器





```

### 注解说明

```java
@Api：用在类上，说明该类的作用。
@ApiOperation：注解来给API增加方法说明。
@ApiImplicitParams : 用在方法上包含一组参数说明。
@ApiImplicitParam：用来注解来给方法入参增加说明。
@ApiResponses：用于表示一组响应
@ApiResponse：用在@ApiResponses中，一般用于表达一个错误的响应信息
@ApiModel：描述一个Model的信息（一般用在请求参数无法使用@ApiImplicitParam注解进行描述的时候）
@ApiModelProperty：描述一个model的属性
```

### 下载Swagger UI组件 

去官网下载Zip包，或者在github上下载也可以，需要将dist文件夹下的所有文件的复制到webapp目录下

原理就是在系统加载的时候，Swagger配置类去扫描所有添加注释的接口，并且储存起来通过下面地址进行访问，返回JSON数据，在前端界面显示出来。 启动项目后，访问*http://localhost:8099/swagger-ui.html*