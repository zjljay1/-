[SpringBoot中AOP的使用_springboot aop_彤彤的小跟班的博客-CSDN博客](https://blog.csdn.net/weixin_45583303/article/details/118565966?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522168109116616782425151451%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=168109116616782425151451&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-4-118565966-null-null.142^v82^insert_down38,201^v4^add_ask,239^v2^insert_chatgpt&utm_term=springboot%20apo&spm=1018.2226.3001.4187)
<a name="ukE5Q"></a>
## 提前预知
本片博客主要介绍spring中aop的使用，用过spring框架的都知道，aop是spring框架的两大核心功能之一，还有一个就是ioc，下面我们就springboot中如何引入aop来做一下探讨<br />引入AOP依赖包后，一般来说并不需要去做其他配置，使用过Spring注解配置方式的人会问是否需要在程序主类中增加@EnableAspectJAutoProxy来启用，实际并不需要。<br />因为在AOP的默认配置属性中，spring.aop.auto属性默认是开启的，也就是说只要引入了AOP依赖后，默认已经增加了@EnableAspectJAutoProxy<br />Springboot中有关AOP相关的自动配置包为：AopAutoConfiguration<br />java版本：1.8，SpringBoot版本：2.5.14<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1687485565858-784a1c44-8ac7-4874-94cf-cf813ca57dda.webp#averageHue=%23f8f8f8&clientId=ud4f000d7-4293-4&from=paste&id=u326fd056&originHeight=556&originWidth=764&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=ua9f5fd39-1cd8-477e-b1a7-8e7caf6f45c&title=)
<a name="lS2jO"></a>
## 相关注解
1.@Component ：将当前类注入到Spring容器内。<br />2.@Aspect ：表明当前类是一个切面类。<br />3.@Before ：前置通知，在被切入方法执行之前执行。<br />4.@After ：后置通知，在被切入方法执行之后执行。<br />5.@AfterRuturning ：返回通知，在被切入方法返回结果之后执行。使用此注解的方法，可以使用returning = "XXX"返回被切入方法的返回值，XXX即为被切入方法的返回值，本例中是controller类中方法的返回值。
```java
//返回通知
@AfterReturning(returning = "ret", pointcut = "log()")
    public void doAfterReturning(Object ret) throws Throwable {
    // 处理完请求，返回内容
    System.out.println("返回通知：方法的返回值 : " + ret);
}

```
6.@AfterThrowing ：异常通知，在被切入方法抛出异常之后执行。在此注解中可以使用throwing = "XXX"获取异常信息，获取的异常信息可以在方法中打印出来，举例如下。
```java
//异常通知
@AfterThrowing(throwing = "ex", pointcut = "log()")
    public void throwss(JoinPoint jp, Exception ex) {
    System.out.println("异常通知：方法异常时执行.....");
    System.out.println("产生异常的方法：" + jp);
    System.out.println("异常种类：" + ex);
}

```
7.@Around ：环绕通知，围绕着被切入方法执行。参数必须为ProceedingJoinPoint，pjp.proceed相应于执行被切面的方法。环绕通知一般单独使用，环绕通知可以替代上面的四种通知，后面单独介绍。<br />8.@Pointcut ：切入点，PointCut（切入点）表达式有很多种，其中execution用于使用切面的连接点。<br />注意：

- 返回通知和异常通知只能有一个会被执行，因为发生异常执行异常通知，然后就不会继续向下执行，自然后置通知也就不会被执行，反之亦然。
- 后置通知一定会执行
<a name="vjm3I"></a>
## 相关概念

- **Joinpoint(连接点)**：所谓连接点是指那些被拦截到的点，在 spring 中，这些点指的是方法，因为 spring 只支持方法类型的连接点，通俗的说就是被增强类中的所有方法。注意： 除了环绕通知外，其他的四个通知注解中，加或者不加参数JoinPoint都可以，如果有用到JoinPoint的地方就加，用不到就可以不加。JoinPoint里包含了类名、被切面的方法名，参数等属性。
- **PointCut(切入点)**：所谓切入点是指我们要对哪些 Joinpoint 进行拦截的定义，通俗的说就是被增强类中的被增强的方法，因为被增强类中并不是所有的方法都被增强了。
- **Advice(通知/增强)**：所谓通知是指拦截到 Joinpoint （被增强的方法）之后所要做的事情就是通知，通俗的说就是对被增强的方法进行增强的代码
- **Aspect(切面)**：是切入点和通知（引介）的结合，通俗的说就是建立切入点和通知方法在创建时的对应关系
<a name="XWBS0"></a>
## 切入点表达式详解：@PointCut(表达式)
PointCut：切入点，指哪些方法需要被执行AOP，PointCut表达式可以有一下几种方式：
<a name="GNcSe"></a>
### execution表达式
表达式语法：访问修饰符 返回值 包名.包名.包名…类名.方法名(参数列表)
```java
public void com.aismall.testaop.controller.HelloController.helloAop()
```
```java
void com.aismall.testaop.controller.HelloController.helloAop()
```
```java
* com.aismall.testaop.controller.HelloController.helloAop()
```
```java
*  *.*.*.*.HelloController.helloAop()
```
```java
* *...HelloController.helloAop()
```
```java
* *..*.*()
```
表达式中的参数列表，可以直接写数据类型：

- 基本类型直接写名称 ：例如，int
- 引用类型写包名.类名的方式 ：例如，java.lang.String
- 可以使用通配符*表示任意类型，但是必须有参数
- 可以使用…表示有无参数均可，有参数可以是任意类型

全通配写法：* *..*.*(...)
<a name="WqGsG"></a>
### within表达式
是用来指定类型的，指定类型中的所有方法将被拦截<br />表达式：包名.包名.包名…类名<br />标准的表达式写法范例：
```java
com.aismall.testaop.controller.HelloController
```
举例：匹配HelloController类对应对象的所有方法，并且只能是HelloController类生成的对象，不能是它的子类生成的对象。
```java
within(com.aismall.testaop.controller.HelloController)
```
也可以使用通配符*：匹配com.aismall包及其子包下面的所有类的所有方法。
```java
within(com.aismall…*)
```
<a name="dS8tp"></a>
### this(type)
SpringAOP是基于代理的，this就代表代理对象，语法是this(type)，当生成的代理对象可以转化为type指定的类型时表示匹配。<br />this(com.aismall.testaop.controller.HelloController)匹配生成的代理对象是HelloController类型的所有方法的外部调用
<a name="K4raC"></a>
### target
SpringAOP是基于代理的，target表示被代理的目标对象，当被代理的目标对象可以转换为指定的类型时则表示匹配。<br />target(com.aismall.testaop.controller.HelloController) 匹配所有被代理的目标对象能够转化成HelloController类型的所有方法的外部调用。
<a name="Uh5KV"></a>
### args:
args用来匹配方法参数

- args() 匹配不带参数的方法
- args(java.lang.String) 匹配方法参数是String类型的
- args(…) 带任意参数的方法
- args(java.lang.String,…) 匹配第一个参数是String类型的，其他参数任意。最后一个参数是String的同理。

<a name="BRQRv"></a>
## 注解
@within和@target针对类的注解<br />@annotation针对方法的注解
<a name="gBRap"></a>
### @annotation:
带有相应注解的方法，比如对标有@Transactional注解的方法进行增强
```java
@annotation(org.springframework.transaction.annotation.Transactional)
```
<a name="vyH32"></a>
### @args:
参数带有相应标注的任意方法，比如@Transactional
```java
@args(org.springframework.transaction.annotation.Transactional)
```
<a name="QjU60"></a>
## 逻辑运算符
表达式可由多个切点函数通过逻辑运算组成

- 基本使用：使用log()方法相当于直接使用上面的表达式
```java
//PointCut表达式
@Pointcut("execution(public * com.aismall.testaop.controller.HelloController.*(..))")
//PointCut签名
public void log(){
}
```

- PointCut中的运算符：PointCut中可以使用&&、||、!运算
```java
@Pointcut("execution(public * com.aismall.testaop.controller.HelloController.*(..))")
    public void cutController(){
    }

@Pointcut("execution(public * com.aismall.testaop.service.UserService.*(..))")
    public void cutService(){
}

//使用 && 运算符，则cutAll()的作用等同于cutController 和 cutService 之和
@Pointcut("cutController() && cutService()")
    public void cutAll(){
}

```
<a name="iRzAN"></a>
## 实战环节
使用aop来完成全局请求日志处理<br /> ![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681173890880-00850e51-7929-4b3f-b8d1-bcc60c78d56e.png#averageHue=%23faf9f7&clientId=u48d95ea4-2662-4&from=paste&height=513&id=ufed0fdf3&originHeight=705&originWidth=578&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=33430&status=done&style=none&taskId=u409e05c9-ed8e-4be2-a193-debfaac48ba&title=&width=420.3636363636364)<br />第一步：使用Spring Initializr快速创建一个springboot web项目，名称为testaop<br />第二步：引入aop相关的依赖
```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
  <groupId>org.projectlombok</groupId>
  <artifactId>lombok</artifactId>
  <optional>true</optional>
</dependency>
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-test</artifactId>
  <scope>test</scope>
</dependency>

<!--        aop-->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```
创建个controller
```java
	@RestController
    public class HelloController {
        @RequestMapping("/helloAop")
        public Object hello(){
            return "hello aop";
        }
        @RequestMapping("/helloError")
        public Object helloError(){
            return 1/0;
        }
    }

```
创建一个aspect切面类4
```java
@Aspect
@Component
public class MyAop {
    //切入点:待增强的方法
    @Pointcut("execution(public * com.aismall.testaop.controller.HelloController.hello(..)) " +
            "|| execution(public * com.aismall.testaop.controller.HelloController.helloError(..))")
    //切入点签名
    public void log(){
        System.out.println("pointCut签名。。。");
    }
    //前置通知
    @Before("log()")
    public void deBefore(JoinPoint jp) throws Throwable {
        // 接收到请求，记录请求内容
        ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
        HttpServletRequest request = attributes.getRequest();
        // 记录下请求内容
        System.out.println("URL : " + request.getRequestURL().toString());
        System.out.println("HTTP_METHOD : " + request.getMethod());
        System.out.println("CLASS_METHOD : " + jp);
        System.out.println("ARGS : " + Arrays.toString(jp.getArgs()));
        System.out.println("前置通知：方法执行前执行...");
    }
    //返回通知
    @AfterReturning(returning = "ret", pointcut = "log()")
    public void doAfterReturning(Object ret) throws Throwable {
        // 处理完请求，返回内容
        System.out.println("返回通知：方法的返回值 : " + ret);
    }

    //异常通知
    @AfterThrowing(throwing = "ex", pointcut = "log()")
    public void throwss(JoinPoint jp, Exception ex) {
        System.out.println("异常通知：方法异常时执行.....");
        System.out.println("产生异常的方法：" + jp);
        System.out.println("异常种类：" + ex);
    }

    //后置通知
    @After("log()")
    public void after(JoinPoint jp){
        System.out.println("后置通知：最后且一定执行.....");
    }
}

```
启动项目

- 请求链接：http://localhost:8080/helloAop
- 控制台返回的结果：
```java
URL : http://localhost:8080/helloAop
HTTP_METHOD : GET
CLASS_METHOD : execution(Object com.aismall.testaop.controller.HelloController.hello())
ARGS : []
前置通知：方法执行前执行...
返回通知：方法的返回值 : hello aop
后置通知：最后且一定执行.....
```

- 请求链接：http://localhost:8080/helloError
- 控制台返回的结果（部分）：
```java
URL : http://localhost:8080/helloError
HTTP_METHOD : GET
CLASS_METHOD : execution(Object com.aismall.testaop.controller.HelloController.helloError())
ARGS : []
前置通知：方法执行前执行...
异常通知：方法异常时执行.....
产生异常的方法：execution(Object com.aismall.testaop.controller.HelloController.helloError())
异常种类：java.lang.ArithmeticException: / by zero
后置通知：最后且一定执行.....
```

- 分析：返回通知和异常通知只会执行一个，后置通知一定会执行。
<a name="XvNzm"></a>
## 实战环节二
下面就再演示一个@annotation方式吧<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681174049504-5c56ad3c-b36d-4a5a-b2a7-4f4156ac9241.png#averageHue=%23fcfafa&clientId=u48d95ea4-2662-4&from=paste&id=u5e86719b&originHeight=459&originWidth=577&originalType=url&ratio=1.25&rotation=0&showTitle=false&size=31022&status=done&style=none&taskId=ud871b4ca-eea0-4413-be27-fbf181d0dfb&title=)<br />编写一个自定义注解
```java

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

//表示此注解可以标注在类和方法上
@Target({ElementType.METHOD, ElementType.TYPE})
//运行时生效
@Retention(RetentionPolicy.RUNTIME)
public @interface MyLogAnnotation {
    //定义一个变量，可以接收参数
    String desc() default " ";
}
```
在HelloController类中添加一个方法
```java
@RequestMapping("helloAnnotation")
//标有这个注解的方法会被增强
@MyLogAnnotation(desc = "测试注解类型:helloAnnotation")
public Object helloAnnotation() {
    return "hello annotation";
}
```
切面类
```java
@Aspect
@Component
public class MyAopAnnotation {
    //切入点：增强标有MyLogAnnotation注解的方法
    @Pointcut(value="@annotation(com.aismall.testaop.MyAnnotation.MyLogAnnotation)")
    //切入点签名
    public void logAnnotation(){
        System.out.println("pointCut签名。。。");
    }
    //前置通知
    //注意：获取注解中的属性时，@annotation()中的参数要和deBefore方法中的参数写法一样，即myLogAnnotation
    @Before("logAnnotation() && @annotation(myLogAnnotation)")
    public void deBefore(JoinPoint jp, MyLogAnnotation myLogAnnotation) throws Throwable {
     	System.out.println("前置通知：方法执行前执行...");
        // 接收到请求，记录请求内容
        ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
        HttpServletRequest request = attributes.getRequest();
        // 获取注解中的属性
        System.out.println("deBefore===========>" + myLogAnnotation.desc());
        // 记录下请求内容
        System.out.println("URL : " + request.getRequestURL().toString());
    }
    //返回通知
    @AfterReturning(returning = "ret", pointcut = "logAnnotation() && @annotation(myLogAnnotation)")
    public void doAfterReturning(Object ret, MyLogAnnotation myLogAnnotation) throws Throwable {
        // 获取注解中的属性
        System.out.println("doAfterReturning===========>" + myLogAnnotation.desc());
        // 处理完请求，返回内容
        System.out.println("返回通知：方法的返回值 : " + ret);
    }

    //异常通知
    @AfterThrowing(throwing = "ex", pointcut = "logAnnotation() && @annotation(myLogAnnotation)")
    public void throwss(JoinPoint jp,Exception ex, MyLogAnnotation myLogAnnotation){
        // 获取注解中的属性
        System.out.println("throwss===========>" + myLogAnnotation.desc());
        System.out.println("异常通知：方法异常时执行.....");
        System.out.println("产生异常的方法："+jp);
        System.out.println("异常种类："+ex);
    }

    //后置通知
    @After("logAnnotation() && @annotation(myLogAnnotation)")
    public void after(JoinPoint jp, MyLogAnnotation myLogAnnotation){
        // 获取注解中的属性
        System.out.println("after===========>" + myLogAnnotation.desc());
        System.out.println("后置通知：最后且一定执行.....");
    }
}
```
启动项目

- 请求链接：http://localhost:8080/helloAnnotation
- 控制台返回的结果
```java
前置通知：方法执行前执行...
deBefore===========>测试注解类型:helloAnnotation
URL : http://localhost:8080/helloAnnotation
doAfterReturning===========>测试注解类型:helloAnnotation
返回通知：方法的返回值 : hello annotation
after===========>测试注解类型:helloAnnotation
后置通知：最后且一定执行.....
```
<a name="rGx0T"></a>
## 环绕通知
spring的通知（Advice）中 一共有五种通知，之前已经介绍了四种，为什么不把环绕通知和它们放在一起说，因为环绕通知可以把前面的四种通知都表示出来，而且环绕通知一般单独使用<br />环绕通知的使用：

- Spring框架为我们提供了一个接口：ProceedingJoinPoint，该接口有一个方法proceed()，此方法就相当于明确调用切入点方法。
- 该接口作为环绕通知的方法参数，在程序执行时，spring框架会为我们提供该接口的实现类供我们使用。
- 增强代码写在调用proceed()方法之前为前置通知，之后为返回通知，写在catch中为异常通知，写在finally中为后置通知

![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681174256763-3cffd03e-6b27-4a0a-98d4-90eb67fe89c7.png#averageHue=%23fcfbfa&clientId=u48d95ea4-2662-4&from=paste&id=u56ebe625&originHeight=402&originWidth=604&originalType=url&ratio=1.25&rotation=0&showTitle=false&size=26843&status=done&style=none&taskId=u09e3f2a4-4072-47fd-97ad-ab27851b661&title=)<br />在HelloController类中添加一个方法
```java
@RequestMapping("/helloAround")
public Object helloAround(){
    System.out.println("helloAround执行了。。。");
    return "hello around";
}
```
切面类
```java
@Aspect
@Component
public class MyAroundAop {
    //切入点：待增强的方法
    @Pointcut("execution(public * com.aismall.testaop.controller.HelloController.helloAround(..))")
    //切入点签名
    public void logAround() {
        System.out.println("pointCut签名。。。");
    }

    //环绕通知,环绕增强，相当于MethodInterceptor
    @Around("logAround()")
    public Object aroundAdvice(ProceedingJoinPoint pjp) {
        Object rtValue = null;
        try {
            Object[] args = pjp.getArgs();//得到方法执行所需的参数
            System.out.println("通知类中的aroundAdvice方法执行了。。前置");
            rtValue = pjp.proceed(args);//明确调用切入点方法（切入点方法）
            System.out.println("通知类中的aroundAdvice方法执行了。。返回");
            System.out.println("返回通知："+rtValue);
            return rtValue;
        } catch (Throwable e) {
            System.out.println("通知类中的aroundAdvice方法执行了。。异常");
            throw new RuntimeException(e);
        } finally {
            System.out.println("通知类中的aroundAdvice方法执行了。。后置");
        }
    }
}
```
启动项目

- 请求链接：http://localhost:8080/helloAround
- 控制台返回的结果
```java
通知类中的aroundAdvice方法执行了。。前置
helloAround执行了。。。
通知类中的aroundAdvice方法执行了。。返回
返回通知：hello around
通知类中的aroundAdvice方法执行了。。后置
```
<a name="CLPf6"></a>
### 案例演示二：注解类型
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681174317846-4f1b5298-2fec-4d9b-96e9-ade79b7f25dc.png#averageHue=%23fcf9f8&clientId=u48d95ea4-2662-4&from=paste&id=uaa01f639&originHeight=358&originWidth=468&originalType=url&ratio=1.25&rotation=0&showTitle=false&size=24557&status=done&style=none&taskId=u582845bf-2b2d-4222-8614-b4e7298d2ab&title=)<br />编写一个自定义注解
```java
//表示此注解可以标注在类和方法上
@Target({ElementType.METHOD, ElementType.TYPE})
//运行时生效
@Retention(RetentionPolicy.RUNTIME)
public @interface MyLogAnnotation2 {
    //定义一个变量，可以接收参数
    String desc() default " ";
}
```
在HelloController类中添加一个方法
```java
@RequestMapping("/helloAroundAnnotation")
@MyLogAnnotation2(desc = "测试注解类型:helloAroundAnnotation")
public Object helloAroundAnnotation(){
    System.out.println("helloAroundAnnotation执行了。。。");
    return "hello aroundAnnotation";
}
```
切面类
```java
@Aspect
@Component
public class MyAroundAopAnnotation {
    //切入点：待增强的方法
    @Pointcut("execution(public * com.aismall.testaop.controller.HelloController.helloAroundAnnotation(..))")
    //切入点签名
    public void logAroundAnnotation() {
        System.out.println("pointCut签名。。。");
    }

    //环绕通知,环绕增强，相当于MethodInterceptor
    @Around("logAroundAnnotation() && @annotation(myLogAnnotation2)")
    public Object aroundAnnotationAdvice(ProceedingJoinPoint pjp, MyLogAnnotation2 myLogAnnotation2) {
        Object rtValue = null;
        try {
            // 获取注解中的属性
            System.out.println("aroundAnnotationAdvice===========>" + myLogAnnotation2.desc());
            Object[] args = pjp.getArgs();//得到方法执行所需的参数
            System.out.println("通知类中的aroundAnnotationAdvice方法执行了。。前置");
            rtValue = pjp.proceed(args);//明确调用切入点方法（切入点方法）
            System.out.println("通知类中的aroundAnnotationAdvice方法执行了。。返回");
            System.out.println("返回通知："+rtValue);
            return rtValue;
        } catch (Throwable e) {
            System.out.println("通知类中的aroundAnnotationAdvice方法执行了。。异常");
            throw new RuntimeException(e);
        } finally {
            System.out.println("通知类中的aroundAnnotationAdvice方法执行了。。后置");
        }
    }
}
```
启动项目

- 请求链接：http://localhost:8080/helloAroundAnnotation
- 控制台返回的结果：
```java
aroundAnnotationAdvice===========>测试注解类型:helloAroundAnnotation
通知类中的aroundAnnotationAdvice方法执行了。。前置
helloAroundAnnotation执行了。。。
通知类中的aroundAnnotationAdvice方法执行了。。返回
返回通知：hello aroundAnnotation
通知类中的aroundAnnotationAdvice方法执行了。。后置
```
