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

>### shiro提供和多个默认的过滤器，可以用这些过滤器来配置过滤指定url的访问权限。	
>
>| 配置缩写          | 对应的过滤器                   |                             功能                             |
>| ----------------- | ------------------------------ | :----------------------------------------------------------: |
>| anon              | AnonymousFilter                |                     指定url可以匿名访问                      |
>| authc             | FormAuthenticationFilter       | 指定url需要form表单登录，默认会从请求中获取username、password,rememberMe等参数并尝试登录，如果登录不了就会跳转到loginUrl配置的路径。我们也可以用这个过滤器做默认的登录逻辑，但是一般都是我们自己在控制器写登录逻辑的，自己写的话出错返回的信息都可以定制。 |
>| authcBasic        | BasicHttpAuthenticationFilter  |                     指定url需要basic登录                     |
>| logout            | LogoutFilter                   |     登出过滤器，配置指定url就可以实现退出功能，非常方便      |
>| noSessionCreation | NoSessionCreationFilter        |                         禁止创建会话                         |
>| perms             | PermissionsAuthorizationFilter |                     需要指定权限才能访问                     |
>| port              | PortFilter                     |                     需要指定端口才能访问                     |
>| rest              | HttpMethodPermissionFilter     |      将http请求方法转化成相应的动词来构造一个权限字符串      |
>| roles             | RolesAuthorizationFilter       |                     需要指定角色才能访问                     |
>| ssl               | SslFilter                      |                    需要https请求才能访问                     |
>| user              | UserFilter                     |              需要已登录或“记住我”的用户才能访问              |
>
>

>### shiro常用的权限控制注解，可以在控制器类上使用
>
>|          注解           |                 功能                 |
>| :---------------------: | :----------------------------------: |
>|     @RequiresGuest      |           只有游客可以访问           |
>| @RequiresAuthentication |           需要登录才能访问           |
>|      @RequiresUser      |  已登录的用户或“记住我”的用户能访问  |
>|     @RequiresRoles      | 已登录的用户需具有指定的角色才能访问 |
>|  @RequiresPermissions   | 已登录的用户需具有指定的权限才能访问 |
>
>



>### shiro过滤器链
>
>![image-20221021094854416](https://mapstore-1307680469.cos.ap-chongqing.myqcloud.com/img/202210210949545.png)



>### AccessControlFilter
>
>### 访问控制过滤器。继承PathMatchingFilter过滤器，重写onPreHandle方法，又分出了两个抽象方法来控制
>
>```java
>//isAccessAllowed 是否允许访问
>//onAccessDenied 是否拒绝访问
>
>protected void saveRequestAndRedirectToLogin(ServletRequest request, ServletResponse response) throws IOException {
>    saveRequest(request);
>    redirectToLogin(request, response);
>}
>
>protected void saveRequest(ServletRequest request) {
>    WebUtils.saveRequest(request);
>}
>
>protected void redirectToLogin(ServletRequest request, ServletResponse response) throws IOException {
>    String loginUrl = getLoginUrl();
>    WebUtils.issueRedirect(request, response, loginUrl);
>}
>```
>
>其中redirectToLogin提供了调整到登录页面的逻辑与实现，为后面的过滤器发现未登录跳转到登录页面提供了基础。
>
>这个过滤器，我们可以灵活运用。



>### AuthenticationFilter
>
>### 继承AccessControlFilter，重写了isAccessAllowed方法，通过判断用户是否已经完成登录来判断用户是否允许继续后面的逻辑判断。这里可以看出，从这个过滤器开始，后续的判断会与用户的登录状态相关，直接继承这些过滤器，我们不需要再自己手动去判断用户是否已经登录。并且提供了登录成功之后跳转的方法。
>
>```java
>public abstract class AuthenticationFilter extends AccessControlFilter {
>    public void setSuccessUrl(String successUrl) {
>        this.successUrl = successUrl;
>    }
>
>    protected boolean isAccessAllowed(ServletRequest request, ServletResponse response, Object mappedValue) {
>        Subject subject = getSubject(request, response);
>        return subject.isAuthenticated();
>    }
>}
>```



>### AuthenticatingFilter
>
>### 继承AuthenticationFilter，提供了自动登录、是否登录请求等方法。
>
>```---------------------apl
>executeLogin 执行登录
>onLoginSuccess 登录成功跳转
>onLoginFailure 登录失败跳转
>createToken 创建登录的身份token
>isAccessAllowed 是否允许被访问
>isLoginRequest 是否登录请求
>```
