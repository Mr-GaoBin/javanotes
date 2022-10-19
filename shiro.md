# shiro

### 导入shiro依赖

```xml
        <!--shiro-spring包含shiro-core和shiro-web-->
        <dependency>
            <groupId>org.apache.shiro</groupId>
            <artifactId>shiro-spring-boot-starter</artifactId>
            <version>1.4.1</version>
        </dependency>
```

### 配置

```

```

### ShiroRealm

```
Authentication：认证、鉴权

Authorization：授权
```

### PrincipalCollection身份[集合

```
public interface PrincipalCollection extends Iterable, Serializable {
	Object getPrimaryPrincipal(); //得到主要的身份
	<T> T oneByType(Class<T> type); //根据身份类型获取第一个
	<T> Collection<T> byType(Class<T> type); //根据身份类型获取一组
	List asList(); //转换为List
	Set asSet(); //转换为Set
	Collection fromRealm(String realmName); //根据Realm名字获取
	Set<String> getRealmNames(); //获取所有身份验证通过的Realm名字
	boolean isEmpty(); //判断是否为空
}
```

###  shiro默认过滤器

shiro提供和多个默认的过滤器，可以用这些过滤器来配置过滤指定url的访问权限。	

配置缩写	对应的过滤器	功能
anon	AnonymousFilter	指定url可以匿名访问
authc	FormAuthenticationFilter	指定url需要form表单登录，默认会从请求中获取username、password,rememberMe等参数并尝试登录，如果登录不了就会跳转到loginUrl配置的路径。我们也可以用这个过滤器做默认的登录逻辑，但是一般都是我们自己在控制器写登录逻辑的，自己写的话出错返回的信息都可以定制嘛。
authcBasic	BasicHttpAuthenticationFilter	指定url需要basic登录
logout	LogoutFilter	登出过滤器，配置指定url就可以实现退出功能，非常方便
noSessionCreation	NoSessionCreationFilter	禁止创建会话
perms	PermissionsAuthorizationFilter	需要指定权限才能访问
port	PortFilter	需要指定端口才能访问
rest	HttpMethodPermissionFilter	将http请求方法转化成相应的动词来构造一个权限字符串，这个感觉意义不大，有兴趣自己看源码的注释
roles	RolesAuthorizationFilter	需要指定角色才能访问
ssl	SslFilter	需要https请求才能访问
user	UserFilter	需要已登录或“记住我”的用户才能访问

|      |      |      |
| :--: | ---- | ---- |
|      |      |      |
|      |      |      |
|      |      |      |
|      |      |      |
|      |      |      |
|      |      |      |
|      |      |      |
|      |      |      |
|      |      |      |
|      |      |      |
|      |      |      |





shiro常用的权限控制注解，可以在控制器类上使用

注解	功能
@RequiresGuest	只有游客可以访问
@RequiresAuthentication	需要登录才能访问
@RequiresUser	已登录的用户或“记住我”的用户能访问
@RequiresRoles	已登录的用户需具有指定的角色才能访问
@RequiresPermissions	已登录的用户需具有指定的权限才能访问
