

# spring

>简化企业应用开发
>
>轻量级，免费开源，非入侵式容器框架

## ioc

### 代理模式

```apl
动态代理的本质就是使用反射机制实现
```



>可以使真实角色的操作更加纯粹!不用去关注一些公共的业务
>公共也就就交给代理角色实现了业务的分工
>公共业务发生扩展的时候，方便集中管理

静态代理

>缺点：一个真实角色就会产生-个代理角色;代码量会翻倍~开发效率会变低

动态代理

>优点：
>
>动态代理和静态代理用色一 样
>动态代理的代理类是动态生成的，不是我们直接写好的
>动态代理分为两大类:基于接口的动态代理，基于类的动态代理

**控制反转( Inversion of Control )**:件设计的一个重要思想：**依赖倒置原则（Dependency Inversion Principle ）**

**控制反转（Inversion of Control）** 就是依赖倒置原则的一种代码设计的思路,具体采用的方法就是所谓的**依赖注入（Dependency Injection）**

### 获取spring容器

>ClassPathXmlApplicationContext是用来加载xml配置文件的Context，而AnnotationConfigApplicationContext是用来加载注解配置的Context。虽然直接父类不同，但是都有一个共同的祖先类AbstractApplicationContext。以及同样有一个组合对象BeanFactory提供容器作用。

通过注解获取Bean

```
ApplicationContext applicationContext1 = new AnnotationConfigApplicationContext(Springconfig.class);
```

通过配置文件获取Bean

```
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("bean.xml");
User user = applicationContext.getBean("user", User.class);
```

## springBean生命周期

```
1. 实例化->Instantiation
2. 属性赋值->Populate
3. 初始化->Initialization
4. 销毁->Destruction

实例化 -> 属性赋值 -> 初始化 -> 销毁
```

## aop







## spring常用注解

### 二、Controller 相关注解

```java
@Controller
控制器，处理http请求。

@RestController 复合注解,@RestController注解相当于@ResponseBody+@Controller合在一起的作用,RestController使用的效果是将方法返回的对象直接在浏览器上展示成json格式.

@RequestBody
通过HttpMessageConverter读取Request Body并反序列化为Object（泛指）对象

@RequestMapping
@RequestMapping 是 Spring Web 应用程序中最常被用到的注解之一。这个注解会将 HTTP 请求映射到 MVC 和 REST 控制器的处理方法上

@GetMapping用于将HTTP get请求映射到特定处理程序的方法注解
注解简写：@RequestMapping(value = "/say",method = RequestMethod.GET)等价于：@GetMapping(value = "/say")

@PostMapping 
@PutMapping
@DeleteMapping
```

### 三、取请求参数值

```java
@PathVariable:获取url中的数据    请求示例：http://localhost:8080/User/getUser/123

@RequestHeader 把Request请求header部分的值绑定到方法的参数上
    
@CookieValue 把Request header中关于cookie的值绑定到方法的参数上
```

### 四、注入bean相关

```java
@Repository
DAO层注解，DAO层中接口继承JpaRepository<T,ID extends Serializable>,需要在build.gradle中引入相关jpa的一个jar自动加载。

@Service
@Service是@Component注解的一个特例，作用在类上
@Service注解作用域默认为单例
使用注解配置和类路径扫描时，被@Service注解标注的类会被Spring扫描并注册为Bean
@Service用于标注服务层组件,表示定义一个bean
@Service使用时没有传参数，Bean名称默认为当前类的类名，首字母小写
@Service(“serviceBeanId”)或@Service(value=”serviceBeanId”)使用时传参数，使用value作为Bean名字
    
@Scope作用域注解  默认是单例模式，即scope="singleton"
@Scope作用在类上和方法上，用来配置 spring bean 的作用域，它标识 bean 的作用域
另外scope还有prototype、request、session、global session作用域。scope="prototype"多例
1.singleton单例模式,全局有且仅有一个实例
2.prototype原型模式，每次获取Bean的时候会有一个新的实例

@Entity实体类注解
@Table(name ="数据库表名")，这个注解也注释在实体类上，对应数据库中相应的表。
@Id、@Column注解用于标注实体类中的字段，pk字段标注为@Id，其余@Column。

@Bean产生一个bean的方法
@Bean明确地指示了一种方法，产生一个bean的方法，并且交给Spring容器管理。支持别名@Bean("xx-name")

@Autowired 自动导入
@Autowired注解作用在构造函数、方法、方法参数、类字段以及注解上
@Autowired注解可以实现Bean的自动注入
    
@Nulllable字段可以为空
@Qualifier
如果@Autowired自动装配的环境比较复杂，自动装配无法通过一一个注解[ @Autowired]完成的时候、我们可以
使用@Qualifier(value="xx" )去配置@Autowired的使用，指定一个唯一 -的bean对象注入!

@Component
把普通pojo实例化到spring容器中，相当于配置文件中的

虽然有了@Autowired,但是我们还是要写一堆bean的配置文件,相当麻烦,而@Component就是告诉spring,我是pojo类,把我注册到容器中,spring会自动提取相关信息,不用写xml配置文件了
```

### 五、导入配置文件

```java
@PropertySource注解
引入单个properties文件：

@PropertySource(value = {"classpath : xxxx/xxx.properties"})

引入多个properties文件：

@PropertySource(value = {"classpath : xxxx/xxx.properties"，"classpath : xxxx.properties"})

@ImportResource导入xml配置文件
可以额外分为两种模式 相对路径classpath，绝对路径（真实路径）file

注意：单文件可以不写value或locations，value和locations都可用

相对路径（classpath）

引入单个xml配置文件：@ImportSource("classpath : xxx/xxxx.xml")

引入多个xml配置文件：@ImportSource(locations={"classpath : xxxx.xml" , "classpath : yyyy.xml"})

绝对路径（file）

引入单个xml配置文件：@ImportSource(locations= {"file : d:/hellxz/dubbo.xml"})

引入多个xml配置文件：@ImportSource(locations= {"file : d:/hellxz/application.xml" , "file : d:/hellxz/dubbo.xml"})

取值：使用@Value注解取配置文件中的值

@Value("${properties中的键}")
private String xxx;

@Import 导入额外的配置信息
功能类似XML配置的，用来导入配置类，可以导入带有@Configuration注解的配置类或实现了ImportSelector/ImportBeanDefinitionRegistrar。
```

### 六、事务注解 @Transactional

```
编程式事务管理： 编程式事务管理使用TransactionTemplate或者直接使用底层的PlatformTransactionManager。对于编程式事务管理，spring推荐使用TransactionTemplate。
声明式事务管理： 建立在AOP之上的。其本质是对方法前后进行拦截，然后在目标方法开始之前创建或者加入一个事务，在执行完目标方法之后根据执行情况提交或者回滚事务，通过@Transactional就可以进行事务操作，更快捷而且简单。
```

### 七、全局异常处理

```
@ControllerAdvice 注解定义全局异常处理类
@ExceptionHandler 注解声明异常处理方法
```

## Spring 与 Mybatis 中的 @Repository 与 @Mapper

```java
@mapper
使用 Mybatis 有 XML 文件或者注解的两种使用方式，如果是使用 XML 文件的方式，我们需要在配置文件中指定 XML 的位置，这里只研究注解开发的方式。
在 Spring 程序中，Mybatis 需要找到对应的 mapper，在编译的时候动态生成代理类，实现数据库查询功能，所以我们需要在接口上添加 @Mapper 注解

@Repository
@Repository 用于声明 dao 层的 bean，如果我们要真正地使用 @Repository 来进行开发，那是基于代码的开发，简单来说就是手写 JDBC,
和 @Service、@Controller 一样，我们将 @Repository 添加到对应的实现类上

@MapperScan
@MapperScan 注解，可以在启动类上添加该注解，自动扫描包路径下的所有接口。
```

>1. @Mapper 一定要有，否则 Mybatis 找不到 mapper。
>2. @Repository 可有可无，可以消去依赖注入的报错信息。
>3. @MapperScan 可以替代 @Mapper。

