[Shiro安全框架【快速入门】就这一篇！](https://zhuanlan.zhihu.com/p/54176956)
<a name="JhcU7"></a>
## 做什么
Apache Shiro是一个Java 的安全(权限)框架。<br />使用 Shiro 官方给了许多令人信服的原因，因为 Shiro 具有以下几个特点：

- **易于使用**——易用性是项目的最终目标。应用程序安全非常令人困惑和沮丧,被认为是“不可避免的灾难”。如果你让它简化到新手都可以使用它,它就将不再是一种痛苦了。
- **全面**——没有其他安全框架的宽度范围可以同Apache Shiro一样,它可以成为你的“一站式”为您的安全需求提供保障。
- **灵活**——Apache Shiro可以在任何应用程序环境中工作。虽然在网络工作、EJB和IoC环境中可能并不需要它。但Shiro的授权也没有任何规范,甚至没有许多依赖关系。
- **Web支持**——Apache Shiro拥有令人兴奋的web应用程序支持,允许您基于应用程序的url创建灵活的安全策略和网络协议(例如REST),同时还提供一组JSP库控制页面输出。
- **低耦合**——Shiro干净的API和设计模式使它容易与许多其他框架和应用程序集成。你会看到Shiro无缝地集成Spring这样的框架, 以及Grails, Wicket, Tapestry, Mule, Apache Camel, Vaadin…等。
- **被广泛支持**——Apache Shiro是Apache软件基金会的一部分。项目开发和用户组都有友好的网民愿意帮助。这样的商业公司如果需要Katasoft还提供专业的支持和服务。
- 官网: 

[https://shiro.apache.org/](https://shiro.apache.org/)

- 官方文档十分钟快速入门：

[https://shiro.apache.org/10-minute-tutorial.html](https://shiro.apache.org/10-minute-tutorial.html)

- 下载地址:

[GitHub - apache/shiro: Apache Shiro](https://github.com/apache/shiro)
<a name="OgcVA"></a>
## **Apache Shiro Features 特性**
Apache Shiro是一个全面的、蕴含丰富功能的安全框架。下图为描述Shiro功能的框架图：<br />![](https://cdn.nlark.com/yuque/0/2023/jpeg/28163149/1689524564845-41f9e615-7e57-4645-a9c8-0037b4c08f30.jpeg#averageHue=%2394ab64&clientId=u7fdc72db-4ab8-4&from=paste&id=u63d22728&originHeight=253&originWidth=491&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u4fd57c49-4186-4547-9574-11ad83e1a27&title=)<br />Authentication(认证)，Authorization(授权)，Session Management(会话管理)，Cryptography(加密)被 Shiro 框架的开发团队称之为应用安全的四大基石。那么就让我们来看看它们吧：

- **Authentication(认证) : **用户身份识别，通常用于用户登录
- **Authorization(授权)：**访问控制。比如：某个用户是否具有某个操作的使用权限
- **Session Management(会话管理)： **特定用于用户的会话管理，甚至在非WEB或EJB应用程序
- **Cryptography(加密)：**在对数据源使用加密算法的同时，保证易于使用。

还有其他的功能来支持和加强这些不同应用环境下安全领域的关注点。特别是对以下的功能支持：

- **Web支持：**Shiro的Web支持API有助于保护Web应用程序。
- **缓存：**缓存是Apache Shiro API中的第一级，以确保安全操作保持快速和高效。
- **并发性：**Apache Shiro支持具有并发功能的多线程应用程序。
- **测试：**存在测试支持，可帮助您编写单元测试和集成测试，并确保代码按预期得到保障。
- **“运行方式”：**允许用户承担另一个用户的身份(如果允许)的功能，有时在管理方案中很有用。
- **“记住我”：**记住用户在会话中的身份，所以用户只需要强制登录即可。
:::success
**注意：** Shiro不会去维护用户、维护权限，这些需要我们自己去设计/提供，然后通过相应的接口注入给Shiro
:::
<a name="YEe4k"></a>
## High-Level Overview 高级概述
在概念层，shiro架构包含三个主要理念：Subject(当前用户)，SecurityManager(管理所有的用户)，Reamls(权限信息认证)。<br />![](https://cdn.nlark.com/yuque/0/2023/jpeg/28163149/1690771368185-e058d30d-772f-4ac6-a21f-874355e4e11b.jpeg#averageHue=%23f5f5f5&clientId=u06099888-6df4-4&from=paste&id=u97f28d12&originHeight=220&originWidth=410&originalType=url&ratio=1.25&rotation=0&showTitle=false&status=done&style=none&taskId=u0d56b3e2-d102-4e93-8c18-6e6f58b2cd2&title=)

- **Subject : **当前用户，Subject可以是一个人，也可以是第三方服务，守护进程账户，时钟守护任务或者其他-当前和软件交互的任何事件。
- **SecurityManager: **管理所有的Subject,SecurityManager是shiro架构的核心，配合内部安全组件共同组成安全伞
- **Realms： **用于进行权限信息的验证，**我们自己实现，**Realm 本质上是一个特定的安全 DAO：它封装与数据源连接的细节，得到Shiro 所需的相关的数据。在配置 Shiro 的时候，你必须指定至少一个Realm 来实现认证（authentication）和/或授权（authorization）。

我们需要实现Realms的Authentication 和 Authorization。其中 Authentication 是用来验证用户身份，Authorization 是授权访问控制，用于对用户进行的操作授权，证明该用户是否允许进行当前操作，如访问某个链接，某个资源文件等。
<a name="WgGCX"></a>
## shiro认证过程
![](https://cdn.nlark.com/yuque/0/2023/jpeg/28163149/1690771683255-cda0c6d4-6783-4150-8b2b-407e021b4056.jpeg#averageHue=%23f2ece8&clientId=u06099888-6df4-4&from=paste&id=uf386d3cd&originHeight=283&originWidth=629&originalType=url&ratio=1.25&rotation=0&showTitle=false&status=done&style=none&taskId=ud76989fc-6d20-4743-b3ed-76f9646d435&title=)跟上图展示了 Shiro 认证的一个重要的过程，为了加深我们的印象，我们来自己动手来写一个例子，来验证一下，首先我们新建一个Maven工程，然后在pom.xml中引入相关依赖：
```xml
<dependency>
    <groupId>org.apache.shiro</groupId>
    <artifactId>shiro-core</artifactId>
    <version>1.4.0</version>
</dependency>
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
</dependency>
```
新建一个【AuthenticationTest】测试类：
```java
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.mgt.DefaultSecurityManager;
import org.apache.shiro.realm.SimpleAccountRealm;
import org.apache.shiro.subject.Subject;
import org.junit.Before;
import org.junit.Test;

public class AuthenticationTest {

    SimpleAccountRealm simpleAccountRealm = new SimpleAccountRealm();

    @Before // 在方法开始前添加一个用户
    public void addUser() {
        simpleAccountRealm.addAccount("wmyskxz", "123456");
    }

    @Test
    public void testAuthentication() {

        // 1.构建SecurityManager环境
        DefaultSecurityManager defaultSecurityManager = new DefaultSecurityManager();
        defaultSecurityManager.setRealm(simpleAccountRealm);

        // 2.主体提交认证请求
        SecurityUtils.setSecurityManager(defaultSecurityManager); // 设置SecurityManager环境
        Subject subject = SecurityUtils.getSubject(); // 获取当前主体

        UsernamePasswordToken token = new UsernamePasswordToken("wmyskxz", "123456");
        subject.login(token); // 登录

        // subject.isAuthenticated()方法返回一个boolean值,用于判断用户是否认证成功
        System.out.println("isAuthenticated:" + subject.isAuthenticated()); // 输出true

        subject.logout(); // 登出

        System.out.println("isAuthenticated:" + subject.isAuthenticated()); // 输出false
    }
}
```
运行之后可以看到预想中的效果，先输出isAuthenticated:true表示登录认证成功，然后再输出isAuthenticated:false表示认证失败退出登录，再来一张图加深一下印象：<br />![](https://cdn.nlark.com/yuque/0/2023/jpeg/28163149/1690772317988-f2ab7a9a-d3c5-42f4-af47-543cc23c213c.jpeg#averageHue=%23c5cacb&clientId=u06099888-6df4-4&from=paste&id=u26192012&originHeight=314&originWidth=483&originalType=url&ratio=1.25&rotation=0&showTitle=false&status=done&style=none&taskId=uff427ec0-839e-4eb4-81ff-7f707a5a0d6&title=)

流程如下：

1. 首先调用 Subject.login(token) 进行登录，其会自动委托给 Security Manager，调用之前必须通过 SecurityUtils.setSecurityManager() 设置；
2. SecurityManager 负责真正的身份验证逻辑；它会委托给 Authenticator 进行身份验证；
3. Authenticator 才是真正的身份验证者，**Shiro API 中核心的身份认证入口点，此处可以自定义插入自己的实现；**
4. Authenticator 可能会委托给相应的 AuthenticationStrategy 进行多 Realm 身份验证，默认 ModularRealmAuthenticator 会调用 AuthenticationStrategy 进行多 Realm 身份验证；
5. Authenticator 会把相应的 token 传入 Realm，从 Realm 获取身份验证信息，如果没有返回 / 抛出异常表示身份验证失败了。此处可以配置多个 Realm，将按照相应的顺序及策略进行访问。
<a name="uPFoz"></a>
## **Shiro 授权过程**
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1690772336810-44bea562-b528-4d40-93a7-123f57da6af6.webp#averageHue=%23f2efea&clientId=u06099888-6df4-4&from=paste&id=u42a5945a&originHeight=294&originWidth=624&originalType=url&ratio=1.25&rotation=0&showTitle=false&status=done&style=none&taskId=ua821b02a-db46-414d-af0d-997ee13de0a&title=)<br />跟认证过程大致相似，下面我们仍然通过代码来熟悉一下过程（引入包类似这里节约篇幅就不贴出来了）：
```java
public class AuthenticationTest {

    SimpleAccountRealm simpleAccountRealm = new SimpleAccountRealm();

    @Before // 在方法开始前添加一个用户,让它具备admin和user两个角色
    public void addUser() {
        simpleAccountRealm.addAccount("wmyskxz", "123456", "admin", "user");
    }

    @Test
    public void testAuthentication() {

        // 1.构建SecurityManager环境
        DefaultSecurityManager defaultSecurityManager = new DefaultSecurityManager();
        defaultSecurityManager.setRealm(simpleAccountRealm);

        // 2.主体提交认证请求
        SecurityUtils.setSecurityManager(defaultSecurityManager); // 设置SecurityManager环境
        Subject subject = SecurityUtils.getSubject(); // 获取当前主体

        UsernamePasswordToken token = new UsernamePasswordToken("wmyskxz", "123456");
        subject.login(token); // 登录

        // subject.isAuthenticated()方法返回一个boolean值,用于判断用户是否认证成功
        System.out.println("isAuthenticated:" + subject.isAuthenticated()); // 输出true
        // 判断subject是否具有admin和user两个角色权限,如没有则会报错
        subject.checkRoles("admin","user");
        //        subject.checkRole("xxx"); // 报错
    }
}
```
运行测试，能够正确看到效果。
<a name="h6GMl"></a>
## **自定义 Realm**
从上面我们了解到实际进行权限信息验证的是我们的 Realm，Shiro 框架内部默认提供了两种实现，一种是查询.ini文件的IniRealm，另一种是查询数据库的JdbcRealm，这两种来说都相对简单，感兴趣的可以去[【这里】](https://link.zhihu.com/?target=http%3A//how2j.cn/k/shiro/shiro-tutorial/1720.html%23nowhere)瞄两眼，我们着重就来介绍介绍自定义实现的 Realm 吧。<br />有了上面的对认证和授权的理解，我们先在合适的包下创建一个【MyRealm】类，继承 Shirot 框架的 AuthorizingRealm 类，并实现默认的两个方法：
```java
import org.apache.shiro.authc.*;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;

import java.util.*;

public class MyRealm extends AuthorizingRealm {

    /**
     * 模拟数据库数据
     */
    Map<String, String> userMap = new HashMap<>(16);

    {
        userMap.put("wmyskxz", "123456");
        super.setName("myRealm"); // 设置自定义Realm的名称，取什么无所谓..
    }

    /**
     * 授权
     *
     * @param principalCollection
     * @return
     */
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        String userName = (String) principalCollection.getPrimaryPrincipal();
        // 从数据库获取角色和权限数据
        Set<String> roles = getRolesByUserName(userName);
        Set<String> permissions = getPermissionsByUserName(userName);

        SimpleAuthorizationInfo simpleAuthorizationInfo = new SimpleAuthorizationInfo();
        simpleAuthorizationInfo.setStringPermissions(permissions);
        simpleAuthorizationInfo.setRoles(roles);
        return simpleAuthorizationInfo;
    }

    /**
     * 模拟从数据库中获取权限数据
     *
     * @param userName
     * @return
     */
    private Set<String> getPermissionsByUserName(String userName) {
        Set<String> permissions = new HashSet<>();
        permissions.add("user:delete");
        permissions.add("user:add");
        return permissions;
    }

    /**
     * 模拟从数据库中获取角色数据
     *
     * @param userName
     * @return
     */
    private Set<String> getRolesByUserName(String userName) {
        Set<String> roles = new HashSet<>();
        roles.add("admin");
        roles.add("user");
        return roles;
    }

    /**
     * 认证
     *
     * @param authenticationToken 主体传过来的认证信息
     * @return
     * @throws AuthenticationException
     */
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        // 1.从主体传过来的认证信息中，获得用户名
        String userName = (String) authenticationToken.getPrincipal();

        // 2.通过用户名到数据库中获取凭证
        String password = getPasswordByUserName(userName);
        if (password == null) {
            return null;
        }
        SimpleAuthenticationInfo authenticationInfo = new SimpleAuthenticationInfo("wmyskxz", password, "myRealm");
        return authenticationInfo;
    }

    /**
     * 模拟从数据库取凭证的过程
     *
     * @param userName
     * @return
     */
    private String getPasswordByUserName(String userName) {
        return userMap.get(userName);
    }
}
```
然后我们编写测试类，来验证是否正确：
```java
import com.wmyskxz.demo.realm.MyRealm;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.mgt.DefaultSecurityManager;
import org.apache.shiro.subject.Subject;
import org.junit.Test;

public class AuthenticationTest {

    @Test
    public void testAuthentication() {

        MyRealm myRealm = new MyRealm(); // 实现自己的 Realm 实例

        // 1.构建SecurityManager环境
        DefaultSecurityManager defaultSecurityManager = new DefaultSecurityManager();
        defaultSecurityManager.setRealm(myRealm);

        // 2.主体提交认证请求
        SecurityUtils.setSecurityManager(defaultSecurityManager); // 设置SecurityManager环境
        Subject subject = SecurityUtils.getSubject(); // 获取当前主体

        UsernamePasswordToken token = new UsernamePasswordToken("wmyskxz", "123456");
        subject.login(token); // 登录

        // subject.isAuthenticated()方法返回一个boolean值,用于判断用户是否认证成功
        System.out.println("isAuthenticated:" + subject.isAuthenticated()); // 输出true
        // 判断subject是否具有admin和user两个角色权限,如没有则会报错
        subject.checkRoles("admin", "user");
//        subject.checkRole("xxx"); // 报错
        // 判断subject是否具有user:add权限
        subject.checkPermission("user:add");
    }
}
```
 运行测试，完美。
<a name="OEHlz"></a>
## shiro加密
在之前的学习中，我们在数据库中保存的密码都是明文的，一旦数据库数据泄露，那就会造成不可估算的损失，所以我们通常都会使用非对称加密，简单理解也就是**不可逆**的加密，而 md5 加密算法就是符合这样的一种算法。![](https://cdn.nlark.com/yuque/0/2023/jpeg/28163149/1690773250719-82e047f3-55d3-4ae6-8ea6-2575b480ce98.jpeg#averageHue=%233b3b35&clientId=u06099888-6df4-4&from=paste&id=ufa6d43a6&originHeight=288&originWidth=787&originalType=url&ratio=1.25&rotation=0&showTitle=false&status=done&style=none&taskId=u3eefc85a-6d06-456d-9ed1-43a6ad32b5e&title=)<br />如上面的 123456 用 Md5 加密后，得到的字符串：**e10adc3949ba59abbe56e057f20f883e**，就无法通过计算还原回 123456，我们把这个加密的字符串保存在数据库中，等下次用户登录时我们把密码通过同样的算法加密后再从数据库中取出这个字符串进行比较，就能够知道密码是否正确了，这样既保留了密码验证的功能又大大增加了安全性，**但是问题是：虽然无法直接通过计算反推回密码，但是我们仍然可以通过计算一些简单的密码加密后的 Md5 值进行比较，推算出原来的密码**

比如我的密码是 123456，你的密码也是，通过 md5 加密之后的字符串一致，所以你也就能知道我的密码了，如果我们把常用的一些密码都做 md5 加密得到一本字典，那么就可以得到相当一部分的人密码，这也就相当于“破解”了一样，所以其实也没有我们想象中的那么“安全”
<a name="oGSCJ"></a>
### 加盐+多次加密
既然相同的密码 md5 一样，那么我们就让我们的原始密码再**加一个随机数**，然后再进行 md5 加密，这个随机数就是我们说的**盐(salt)**，这样处理下来就能得到不同的 Md5 值，当然我们需要把这个随机数盐也保存进数据库中，以便我们进行验证。<br />另外我们可以通过**多次加密**的方法，即使黑客通过一定的技术手段拿到了我们的密码 md5 值，但它并不知道我们到底加密了多少次，所以这也使得破解工作变得艰难。<br />在 Shiro 框架中，对于这样的操作提供了简单的代码实现：
```java
String password = "123456";
String salt = new SecureRandomNumberGenerator().nextBytes().toString();
int times = 2;  // 加密次数：2
String alogrithmName = "md5";   // 加密算法

String encodePassword = new SimpleHash(alogrithmName, password, salt, times).toString();

System.out.printf("原始密码是 %s , 盐是： %s, 运算次数是： %d, 运算出来的密文是：%s ",password,salt,times,encodePassword);
```
输出：
```java
原始密码是 JAYHLZ , 盐是： X6h20yPSubJRg+1VuTlcLA==, 运算次数是： 520, 运算出来的密文是：a31bc37e872529c00fc8bfbe59076f1c 
```
<a name="ZxTSA"></a>
## SpingBoot简单示例
通过上面的学习，我们现在来着手搭建一个简单的使用 Shiro 进行权限验证授权的一个简单系统
<a name="CPnQw"></a>
### pom.xml
```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <scope>runtime</scope>
</dependency>
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-test</artifactId>
  <scope>test</scope>
</dependency>
<dependency>
  <groupId>org.apache.shiro</groupId>
  <artifactId>shiro-spring</artifactId>
  <version>1.4.0</version>
</dependency>
```
<a name="vSb3N"></a>
### application.properties文件
```properties
#thymeleaf 配置
spring.thymeleaf.mode=HTML5
spring.thymeleaf.encoding=UTF-8
spring.thymeleaf.servlet.content-type=text/html
#缓存设置为false, 这样修改之后马上生效，便于调试
spring.thymeleaf.cache=false

#数据库
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/testdb?useUnicode=true&characterEncoding=utf-8&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=123456
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.jpa.properties.hibernate.hbm2ddl.auto=update
#显示SQL语句
spring.jpa.show-sql=true
#不加下面这句则不会默认创建MyISAM引擎的数据库
spring.jpa.database-platform=org.hibernate.dialect.MySQL5InnoDBDialect
#自己重写的配置类，默认使用utf8编码
spring.jpa.properties.hibernate.dialect=com.wmyskxz.demo.shiro.config.MySQLConfig
```
<a name="cNmkE"></a>
### 实体类
```java
@Entity
    public class UserInfo {
        @Id
        @GeneratedValue
        private Long id; // 主键.
        @Column(unique = true)
        private String username; // 登录账户,唯一.
        private String name; // 名称(匿名或真实姓名),用于UI显示
        private String password; // 密码.
        private String salt; // 加密密码的盐
        @JsonIgnoreProperties(value = {"userInfos"})
        @ManyToMany(fetch = FetchType.EAGER) // 立即从数据库中进行加载数据
        @JoinTable(name = "SysUserRole", joinColumns = @JoinColumn(name = "uid"), inverseJoinColumns = @JoinColumn(name = "roleId"))
        private List<SysRole> roles; // 一个用户具有多个角色

        /** getter and setter */
    }
```
```java
@Entity
public class SysRole {
    @Id
    @GeneratedValue
    private Long id; // 主键.
    private String name; // 角色名称,如 admin/user
    private String description; // 角色描述,用于UI显示

    // 角色 -- 权限关系：多对多
    @JsonIgnoreProperties(value = {"roles"})
    @ManyToMany(fetch = FetchType.EAGER)
    @JoinTable(name = "SysRolePermission", joinColumns = {@JoinColumn(name = "roleId")}, inverseJoinColumns = {@JoinColumn(name = "permissionId")})
    private List<SysPermission> permissions;

    // 用户 -- 角色关系：多对多
    @JsonIgnoreProperties(value = {"roles"})
    @ManyToMany
    @JoinTable(name = "SysUserRole", joinColumns = {@JoinColumn(name = "roleId")}, inverseJoinColumns = {@JoinColumn(name = "uid")})
    private List<UserInfo> userInfos;// 一个角色对应多个用户

    /** getter and setter */
}
```
```java
@Entity
public class SysPermission {
    @Id
    @GeneratedValue
    private Long id; // 主键.
    private String name; // 权限名称,如 user:select
    private String description; // 权限描述,用于UI显示
    private String url; // 权限地址.
    @JsonIgnoreProperties(value = {"permissions"})
    @ManyToMany
    @JoinTable(name = "SysRolePermission", joinColumns = {@JoinColumn(name = "permissionId")}, inverseJoinColumns = {@JoinColumn(name = "roleId")})
    private List<SysRole> roles; // 一个权限可以被多个角色使用

    /** getter and setter */
}
```
:::success
**注意：**这里有一个坑，还缠了我蛮久感觉，就是当我们想要使用RESTful风格返回给前台JSON数据的时候，这里有一个关于多对多无限循环的坑，比如当我们想要返回给前台一个用户信息时，由于一个用户拥有多个角色，一个角色又拥有多个权限，而权限跟角色也是多对多的关系，也就是造成了 查用户→查角色→查权限→查角色→查用户… 这样的无限循环，导致传输错误，所以我们根据这样的逻辑在每一个实体类返回JSON时使用了一个@JsonIgnoreProperties注解，来排除自己对自己无线引用的过程，也就是打断这样的无限循环。
:::
根据以上的代码会自动生成user_info（用户信息表）、sys_role（角色表）、sys_permission（权限表）、sys_user_role（用户角色表）、sys_role_permission（角色权限表）这五张表，为了方便测试我们给这五张表插入一些初始化数据：
:::success
注：spring jpa 可以通过代码生成数据库
:::
```java
INSERT INTO `user_info` (`id`,`name`,`password`,`salt`,`username`) VALUES (1, '管理员','951cd60dec2104024949d2e0b2af45ae', 'xbNIxrQfn6COSYn1/GdloA==', 'wmyskxz');
INSERT INTO `sys_permission` (`id`,`description`,`name`,`url`) VALUES (1,'查询用户','userInfo:view','/userList');
INSERT INTO `sys_permission` (`id`,`description`,`name`,`url`) VALUES (2,'增加用户','userInfo:add','/userAdd');
INSERT INTO `sys_permission` (`id`,`description`,`name`,`url`) VALUES (3,'删除用户','userInfo:delete','/userDelete');
INSERT INTO `sys_role` (`id`,`description`,`name`) VALUES (1,'管理员','admin');
INSERT INTO `sys_role_permission` (`permission_id`,`role_id`) VALUES (1,1);
INSERT INTO `sys_role_permission` (`permission_id`,`role_id`) VALUES (2,1);
INSERT INTO `sys_user_role` (`role_id`,`uid`) VALUES (1,1);
```
<a name="ZIvTb"></a>
### 配置 Shiro
<a name="WrMx2"></a>
#### config
```java
public class MySQLConfig extends MySQL5InnoDBDialect {
    @Override
    public String getTableTypeString() {
        return "ENGINE=InnoDB DEFAULT CHARSET=utf8";
    }
}
```
这个文件关联的是配置文件中最后一个配置，是让 Hibernate 默认创建 InnoDB 引擎并默认使用 utf-8 编码
```java
public class MyShiroRealm extends AuthorizingRealm {
    @Resource
    private UserInfoService userInfoService;

    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        // 能进入这里说明用户已经通过验证了
        UserInfo userInfo = (UserInfo) principalCollection.getPrimaryPrincipal();
        SimpleAuthorizationInfo simpleAuthorizationInfo = new SimpleAuthorizationInfo();
        for (SysRole role : userInfo.getRoles()) {
            simpleAuthorizationInfo.addRole(role.getName());
            for (SysPermission permission : role.getPermissions()) {
                simpleAuthorizationInfo.addStringPermission(permission.getName());
            }
        }
        return simpleAuthorizationInfo;
    }

    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        // 获取用户输入的账户
        String username = (String) authenticationToken.getPrincipal();
        System.out.println(authenticationToken.getPrincipal());
        // 通过username从数据库中查找 UserInfo 对象
        // 实际项目中，这里可以根据实际情况做缓存，如果不做，Shiro自己也是有时间间隔机制，2分钟内不会重复执行该方法
        UserInfo userInfo = userInfoService.findByUsername(username);
        if (null == userInfo) {
            return null;
        }

        SimpleAuthenticationInfo simpleAuthenticationInfo = new SimpleAuthenticationInfo(
                userInfo, // 用户名
                userInfo.getPassword(), // 密码
                ByteSource.Util.bytes(userInfo.getSalt()), // salt=username+salt
                getName() // realm name
        );
        return simpleAuthenticationInfo;
    }
}
```
自定义的 Realm ，方法跟上面的认证授权过程一致
```java
@Configuration
public class ShiroConfig {
    @Bean
    public ShiroFilterFactoryBean shirFilter(SecurityManager securityManager) {
        System.out.println("ShiroConfiguration.shirFilter()");
        ShiroFilterFactoryBean shiroFilterFactoryBean = new ShiroFilterFactoryBean();
        shiroFilterFactoryBean.setSecurityManager(securityManager);
        // 拦截器.
        Map<String, String> filterChainDefinitionMap = new LinkedHashMap<String, String>();
        // 配置不会被拦截的链接 顺序判断
        filterChainDefinitionMap.put("/static/**", "anon");
        // 配置退出 过滤器,其中的具体的退出代码Shiro已经替我们实现了
        filterChainDefinitionMap.put("/logout", "logout");
        // <!-- 过滤链定义，从上向下顺序执行，一般将/**放在最为下边 -->:这是一个坑呢，一不小心代码就不好使了;
        // <!-- authc:所有url都必须认证通过才可以访问; anon:所有url都都可以匿名访问-->
        filterChainDefinitionMap.put("/**", "authc");
        // 如果不设置默认会自动寻找Web工程根目录下的"/login.jsp"页面
        shiroFilterFactoryBean.setLoginUrl("/login");
        // 登录成功后要跳转的链接
        shiroFilterFactoryBean.setSuccessUrl("/index");

        //未授权界面;
        shiroFilterFactoryBean.setUnauthorizedUrl("/403");
        shiroFilterFactoryBean.setFilterChainDefinitionMap(filterChainDefinitionMap);
        return shiroFilterFactoryBean;
    }

    /**
     * 凭证匹配器
     * （由于我们的密码校验交给Shiro的SimpleAuthenticationInfo进行处理了）
     *
     * @return
     */
    @Bean
    public HashedCredentialsMatcher hashedCredentialsMatcher() {
        HashedCredentialsMatcher hashedCredentialsMatcher = new HashedCredentialsMatcher();
        hashedCredentialsMatcher.setHashAlgorithmName("md5"); // 散列算法:这里使用MD5算法;
        hashedCredentialsMatcher.setHashIterations(2); // 散列的次数，比如散列两次，相当于 md5(md5(""));
        return hashedCredentialsMatcher;
    }

    @Bean
    public MyShiroRealm myShiroRealm() {
        MyShiroRealm myShiroRealm = new MyShiroRealm();
        myShiroRealm.setCredentialsMatcher(hashedCredentialsMatcher());
        return myShiroRealm;
    }


    @Bean
    public SecurityManager securityManager() {
        DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
        securityManager.setRealm(myShiroRealm());
        return securityManager;
    }

    /**
     * 开启shiro aop注解支持.
     * 使用代理方式;所以需要开启代码支持;
     *
     * @param securityManager
     * @return
     */
    @Bean
    public AuthorizationAttributeSourceAdvisor authorizationAttributeSourceAdvisor(SecurityManager securityManager) {
        AuthorizationAttributeSourceAdvisor authorizationAttributeSourceAdvisor = new AuthorizationAttributeSourceAdvisor();
        authorizationAttributeSourceAdvisor.setSecurityManager(securityManager);
        return authorizationAttributeSourceAdvisor;
    }

    @Bean(name = "simpleMappingExceptionResolver")
    public SimpleMappingExceptionResolver
    createSimpleMappingExceptionResolver() {
        SimpleMappingExceptionResolver r = new SimpleMappingExceptionResolver();
        Properties mappings = new Properties();
        mappings.setProperty("DatabaseException", "databaseError"); // 数据库异常处理
        mappings.setProperty("UnauthorizedException", "403");
        r.setExceptionMappings(mappings);  // None by default
        r.setDefaultErrorView("error");    // No default
        r.setExceptionAttribute("ex");     // Default is "exception"
        //r.setWarnLogCategory("example.MvcLogger");     // No default
        return r;
    }
}
```
Apache Shiro 的核心通过 Filter 来实现，就好像 SpringMvc 通过 DispachServlet 来主控制一样。 既然是使用 Filter 一般也就能猜到，是通过URL规则来进行过滤和权限校验，所以我们需要定义一系列关于URL的规则和访问权限。<br />Filter Chain定义说明：

- 1、一个URL可以配置多个Filter，使用逗号分隔
- 2、当设置多个过滤器时，全部验证通过，才视为通过
- 3、部分过滤器可指定参数，如perms，roles

Shiro内置的FilterChain，因表格显示问题，麻烦移步简书观看完整版..

- anon:所有url都都可以匿名访问
- authc: 需要认证才能进行访问
- user:配置记住我或认证通过可以访问
<a name="QBwMF"></a>
### DAO 层和 Service 层
新建【dao】包，在下面创建【UserInfoDao】接口：
```java
public interface UserInfoDao extends JpaRepository<UserInfo, Long> {
    /** 通过username查找用户信息*/
    public UserInfo findByUsername(String username);
}
```
新建【service】包，创建【UserInfoService】接口：
```java
public interface UserInfoService {
    /** 通过username查找用户信息；*/
    public UserInfo findByUsername(String username);
}
```
并在该包下再新建一个【impl】包，新建【UserInfoServiceImpl】实现类：
```java
@Service
public class UserInfoServiceImpl implements UserInfoService {

    @Resource
    UserInfoDao userInfoDao;

    @Override
    public UserInfo findByUsername(String username) {
        return userInfoDao.findByUsername(username);
    }
}
```
<a name="vz8fJ"></a>
### controller层
新建【controller】包，然后在下面创建以下文件：
```java
@Controller
public class HomeController {

    @RequestMapping({"/","/index"})
    public String index(){
        return"/index";
    }

    @RequestMapping("/login")
    public String login(HttpServletRequest request, Map<String, Object> map) throws Exception{
        System.out.println("HomeController.login()");
        // 登录失败从request中获取shiro处理的异常信息。
        // shiroLoginFailure:就是shiro异常类的全类名.
        String exception = (String) request.getAttribute("shiroLoginFailure");
        System.out.println("exception=" + exception);
        String msg = "";
        if (exception != null) {
            if (UnknownAccountException.class.getName().equals(exception)) {
                System.out.println("UnknownAccountException -- > 账号不存在：");
                msg = "UnknownAccountException -- > 账号不存在：";
            } else if (IncorrectCredentialsException.class.getName().equals(exception)) {
                System.out.println("IncorrectCredentialsException -- > 密码不正确：");
                msg = "IncorrectCredentialsException -- > 密码不正确：";
            } else if ("kaptchaValidateFailed".equals(exception)) {
                System.out.println("kaptchaValidateFailed -- > 验证码错误");
                msg = "kaptchaValidateFailed -- > 验证码错误";
            } else {
                msg = "else >> "+exception;
                System.out.println("else -- >" + exception);
            }
        }
        map.put("msg", msg);
        // 此方法不处理登录成功,由shiro进行处理
        return "/login";
    }

    @RequestMapping("/403")
    public String unauthorizedRole(){
        System.out.println("------没有权限-------");
        return "403";
    }
}
```
这里边的地址对应我们在设置 Shiro 时设置的地址<br />**UserInfoController**
```java
@RestController
public class UserInfoController {

    @Resource
    UserInfoService userInfoService;

    /**
     * 按username账户从数据库中取出用户信息
     *
     * @param username 账户
     * @return
     */
    @GetMapping("/userList")
    @RequiresPermissions("userInfo:view") // 权限管理.
    public UserInfo findUserInfoByUsername(@RequestParam String username) {
        return userInfoService.findByUsername(username);
    }

    /**
     * 简单模拟从数据库添加用户信息成功
     *
     * @return
     */
    @PostMapping("/userAdd")
    @RequiresPermissions("userInfo:add")
    public String addUserInfo() {
        return "addUserInfo success!";
    }

    /**
     * 简单模拟从数据库删除用户成功
     *
     * @return
     */
    @DeleteMapping("/userDelete")
    @RequiresPermissions("userInfo:delete")
    public String deleteUserInfo() {
        return "deleteUserInfo success!";
    }
}

```
<a name="l4o8Q"></a>
### 准备页面
新建三个页面用来测试：
```html
<!DOCTYPE html>
<head>
  <meta charset="UTF-8">
  <title>首页</title>
</head>
<body>
  index - 首页
</body>
</html>
```
```html
<!DOCTYPE html>
<html xmlns:th="http://www.w3.org/1999/xhtml">
  <head>
    <meta charset="UTF-8">
    <title>登录页</title>
  </head>
  <body>
    错误信息：<h4 th:text="${msg}"></h4>
    <form action="" method="post">
      <p>账号：<input type="text" name="username" value="wmyskxz"/></p>
      <p>密码：<input type="text" name="password" value="123456"/></p>
      <p><input type="submit" value="登录"/></p>
    </form>
  </body>
</html>
```
```html
<!DOCTYPE html>
<head>
  <meta charset="UTF-8">
  <title>403错误页</title>
</head>
<body>
  错误页面
</body>
</html>
```
<a name="GWyK4"></a>
### 测试

1. 编写好程序后就可以启动，首先访问http://localhost:8080/userList?username=wmyskxz页面，由于没有登录就会跳转到我们配置好的http://localhost:8080/login页面。登陆之后就会看到正确返回的JSON数据，上面这些操作时候触发MyShiroRealm.doGetAuthenticationInfo()这个方法，也就是登录认证的方法。
2. 登录之后，我们还能访问http://localhost:8080/userAdd页面，因为我们在数据库中提前配置好了权限，能够看到正确返回的数据，但是我们访问http://localhost:8080/userDelete时，就会返回错误页面.

<br /> <br /> <br /> 
