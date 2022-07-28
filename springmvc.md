# springmvc

Spring Boot整合Spring MVC只需在pom.xml中引入

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

配置Spring MVC(application.yml)

```yaml
server:
  port: 8080 # web服务端口号
  servlet:
    multipart:
      enabled: true # 启用文件上传
      location: # 上传文件临时保存位置
      max-file-size: 50MB # 单个文件上传最大大小限制
      max-request-size: 100MB # 单次请求主体最大大小限制
  mvc:
    format:
      date: yyyy-MM-dd # 日期格式
      date-time: yyyy-MM-dd HH:mm:ss # 日期时间格式
      time: HH:mm:ss # 时间格式
    servlet:
      path: / # servlet路径
      static-path-pattern: # 匹配静态资源路径
    view:
      prefix: # view前缀
      suffix: # view后缀，如：.jsp
```

Spring Boot中MVC配置相关类和接口：

>1. WebMvcConfigurer 接口
>2. WebMvcConfigurerAdapter WebMvcConfigurer 的实现类(废弃)
>3. WebMvcConfigurationSupport MVC的基本实现并包含了WebMvcConfigurer接口中的方法
>4. WebMvcAutoConfiguration MVC的自动装在类并部分包含了 WebMvcConfigurer 接口中的方法



### 拦截器

拦载器一般用于做登录效验，权限认证等统一操作。

```java
@Override
public void addInterceptors(InterceptorRegistry registry) {
    registry.addInterceptor(自定义拦载器)
            .addPathPatterns(拦载的路径);
}
```

自定义拦载器继承 HandlerInterceptor 接口





























