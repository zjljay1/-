<a name="Ys6mJ"></a>
## 实战
<a name="gag3b"></a>
## 设计用户操作日志表: sys_oper_log
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1685973276386-98f2447e-ee8f-4ded-8152-0f5deef803bc.webp#averageHue=%23faf9f8&clientId=u24e77238-6461-4&from=paste&id=u3927e874&originHeight=273&originWidth=1400&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u69d415a5-8107-47a6-8d58-e272aad3969&title=)<br />对应实体类为** SysOperLog.java**
```java
import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.annotation.TableId;
import io.swagger.annotations.ApiModelProperty;
import lombok.Data;
import lombok.EqualsAndHashCode;
import lombok.experimental.Accessors;
import java.io.Serializable;
import java.util.Date;

/**
* <p>
* 操作日志记录
* </p>
*/
@Data
    @EqualsAndHashCode(callSuper = false)
    @Accessors(chain = true)
    public class SysOperLog implements Serializable {

        private static final long serialVersionUID = 1L;

        @TableId(value = "id", type = IdType.AUTO)
        @ApiModelProperty("主键Id")
        private Integer id;

        @ApiModelProperty("模块标题")
        private String title;

        @ApiModelProperty("参数")
        private String optParam;

        @ApiModelProperty("业务类型（0其它 1新增 2修改 3删除）")
        private Integer businessType;

        @ApiModelProperty("路径名称")
        private String uri;

        @ApiModelProperty("操作状态（0正常 1异常）")
        private Integer status;

        @ApiModelProperty("错误消息")
        private String errorMsg;

        @ApiModelProperty("操作时间")
        private Date operTime;
    }
```
<a name="r7kW5"></a>
## 引入依赖
```yaml
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>fastjson</artifactId>
  <version>2.0.9</version>
  </dependency>
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```
<a name="AtGtp"></a>
## 自定义用户操作日志注解
**MyLog.java**
```java
import java.lang.annotation.*;

@Target({ElementType.PARAMETER, ElementType.METHOD})
    @Retention(RetentionPolicy.RUNTIME)
    @Documented
    public @interface MyLog {
        // 自定义模块名，eg:登录
        String title() default "";
        // 方法传入的参数
        String optParam() default "";
        // 操作类型，eg:INSERT, UPDATE...
        BusinessType businessType() default BusinessType.OTHER;  
    }
```
**BusinessType.java** --- 操作类型枚举类
```java
public enum BusinessType {
    // 其它
    OTHER,
    // 查找
    SELECT,
    // 新增
    INSERT,
    // 修改
    UPDATE,
    // 删除
    DELETE,
}
```
<a name="XuANf"></a>
## 自定义用户操作日志切面
**LogAspect.java**
```java
import com.alibaba.fastjson.JSONObject;
import iot.sixiang.license.entity.SysOperLog;
import iot.sixiang.license.handler.IotLicenseException;
import iot.sixiang.license.jwt.UserUtils;
import iot.sixiang.license.service.SysOperLogService;
import lombok.extern.slf4j.Slf4j;
import org.apache.commons.lang3.StringUtils;
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.Signature;
import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.AfterThrowing;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Pointcut;
import org.aspectj.lang.reflect.MethodSignature;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import java.lang.reflect.Method;
import java.util.Date;
import java.util.HashMap;
import java.util.Map;

@Aspect
@Component
@Slf4j
public class LogAspect {
    /**
     * 该Service及其实现类相关代码请自行实现，只是一个简单的插入数据库操作
     */
    @Autowired
    private SysOperLogService sysOperLogService;

    /**
     * @annotation(MyLog类的路径) 在idea中，右键自定义的MyLog类-> 点击Copy Reference
     */
    @Pointcut("@annotation(xxx.xxx.xxx.MyLog)")
    public void logPointCut() {
        log.info("------>配置织入点");
    }

    /**
     * 处理完请求后执行
     *
     * @param joinPoint 切点
     */
    @AfterReturning(pointcut = "logPointCut()")
    public void doAfterReturning(JoinPoint joinPoint) {
        handleLog(joinPoint, null);
    }

    /**
     * 拦截异常操作
     *
     * @param joinPoint 切点
     * @param e         异常
     */
    @AfterThrowing(value = "logPointCut()", throwing = "e")
    public void doAfterThrowing(JoinPoint joinPoint, Exception e) {
        handleLog(joinPoint, e);
    }

    private void handleLog(final JoinPoint joinPoint, final Exception e) {

        // 获得MyLog注解
        MyLog controllerLog = getAnnotationLog(joinPoint);
        if (controllerLog == null) {
            return;
        }
        SysOperLog operLog = new SysOperLog();
        // 操作状态（0正常 1异常）
        operLog.setStatus(0);
        // 操作时间
        operLog.setOperTime(new Date());
        if (e != null) {
            operLog.setStatus(1);
            // IotLicenseException为本系统自定义的异常类，读者若要获取异常信息，请根据自身情况变通
            operLog.setErrorMsg(StringUtils.substring(((IotLicenseException) e).getMsg(), 0, 2000));
        }

        // UserUtils.getUri();获取方法上的路径 如：/login，本文实现方法如下：
        // 1、在拦截器中 String uri = request.getRequestURI();
        // 2、用ThreadLocal存放uri，UserUtils.setUri(uri);
        // 3、UserUtils.getUri();
        String uri = UserUtils.getUri();
        operLog.setUri(uri);
        // 处理注解上的参数
        getControllerMethodDescription(joinPoint, controllerLog, operLog);
        // 保存数据库
        sysOperLogService.addOperlog(operLog.getTitle(), operLog.getBusinessType(), operLog.getUri(), operLog.getStatus(), operLog.getOptParam(), operLog.getErrorMsg(), operLog.getOperTime());
    }

    /**
     * 是否存在注解，如果存在就获取，不存在则返回null
     * @param joinPoint
     * @return
     */
    private MyLog getAnnotationLog(JoinPoint joinPoint) {
    Signature signature = joinPoint.getSignature();
    MethodSignature methodSignature = (MethodSignature) signature;
    Method method = methodSignature.getMethod();
    if (method != null) {
    return method.getAnnotation(MyLog.class);
    }
    return null;
    }

    /**
    * 获取Controller层上MyLog注解中对方法的描述信息
    * @param joinPoint 切点
    * @param myLog 自定义的注解
    * @param operLog 操作日志实体类
    */
    private void getControllerMethodDescription(JoinPoint joinPoint, MyLog myLog, SysOperLog operLog) {
    // 设置业务类型（0其它 1新增 2修改 3删除）
    operLog.setBusinessType(myLog.businessType().ordinal());
    // 设置模块标题，eg:登录
    operLog.setTitle(myLog.title());
    // 对方法上的参数进行处理，处理完：userName=xxx,password=xxx
    String optParam = getAnnotationValue(joinPoint, myLog.optParam());
    operLog.setOptParam(optParam);

    }

    /**
    * 对方法上的参数进行处理
    * @param joinPoint
    * @param name
    * @return
    */
    private String getAnnotationValue(JoinPoint joinPoint, String name) {
    String paramName = name;
    // 获取方法中所有的参数
    Map<String, Object> params = getParams(joinPoint);
    // 参数是否是动态的:#{paramName}
    if (paramName.matches("^#\\{\\D*\\}")) {
    // 获取参数名,去掉#{ }
    paramName = paramName.replace("#{", "").replace("}", "");
    // 是否是复杂的参数类型:对象.参数名
    if (paramName.contains(".")) {
    String[] split = paramName.split("\\.");
    // 获取方法中对象的内容
    Object object = getValue(params, split[0]);
    // 转换为JsonObject
    JSONObject jsonObject = (JSONObject) JSONObject.toJSON(object);
    // 获取值
    Object o = jsonObject.get(split[1]);
    return String.valueOf(o);
    } else {// 简单的动态参数直接返回
    StringBuilder str = new StringBuilder();
    String[] paraNames = paramName.split(",");
    for (String paraName : paraNames) {

    String val = String.valueOf(getValue(params, paraName));
    // 组装成 userName=xxx,password=xxx,
    str.append(paraName).append("=").append(val).append(",");
    }
    // 去掉末尾的,
    if (str.toString().endsWith(",")) {
    String substring = str.substring(0, str.length() - 1);
    return substring;
    } else {
    return str.toString();
    }
    }
    }
    // 非动态参数直接返回
    return name;
    }

    /**
    * 获取方法上的所有参数，返回Map类型, eg: 键："userName",值:xxx  键："password",值:xxx
    * @param joinPoint
    * @return
    */
    public Map<String, Object> getParams(JoinPoint joinPoint) {
    Map<String, Object> params = new HashMap<>(8);
    // 通过切点获取方法所有参数值["zhangsan", "123456"]
    Object[] args = joinPoint.getArgs();
    // 通过切点获取方法所有参数名 eg:["userName", "password"]
    MethodSignature signature = (MethodSignature) joinPoint.getSignature();
    String[] names = signature.getParameterNames();
    for (int i = 0; i < args.length; i++) {
    params.put(names[i], args[i]);
    }
    return params;
    }

    /**
    * 从map中获取键为paramName的值，不存在放回null
    * @param map
    * @param paramName
    * @return
    */
    private Object getValue(Map<String, Object> map, String paramName) {
    for (Map.Entry<String, Object> entry : map.entrySet()) {
    if (entry.getKey().equals(paramName)) {
    return entry.getValue();
    }
    }
    return null;
    }
    }

```
**LogAspect.java**
```java
import com.alibaba.fastjson.JSONObject;
import iot.sixiang.license.entity.SysOperLog;
import iot.sixiang.license.handler.IotLicenseException;
import iot.sixiang.license.jwt.UserUtils;
import iot.sixiang.license.service.SysOperLogService;
import lombok.extern.slf4j.Slf4j;
import org.apache.commons.lang3.StringUtils;
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.Signature;
import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.AfterThrowing;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Pointcut;
import org.aspectj.lang.reflect.MethodSignature;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import java.lang.reflect.Method;
import java.util.Date;
import java.util.HashMap;
import java.util.Map;

@Aspect
    @Component
    @Slf4j
    public class LogAspect {
        /**
* 该Service及其实现类相关代码请自行实现，只是一个简单的插入数据库操作
*/
        @Autowired
        private SysOperLogService sysOperLogService;

        /**
* @annotation(MyLog类的路径) 在idea中，右键自定义的MyLog类-> 点击Copy Reference
*/
        @Pointcut("@annotation(xxx.xxx.xxx.MyLog)")
        public void logPointCut() {
            log.info("------>配置织入点");
        }

        /**
* 处理完请求后执行
*
* @param joinPoint 切点
*/
        @AfterReturning(pointcut = "logPointCut()")
        public void doAfterReturning(JoinPoint joinPoint) {
            handleLog(joinPoint, null);
        }

        /**
* 拦截异常操作
*
* @param joinPoint 切点
* @param e         异常
*/
        @AfterThrowing(value = "logPointCut()", throwing = "e")
        public void doAfterThrowing(JoinPoint joinPoint, Exception e) {
            handleLog(joinPoint, e);
        }

        private void handleLog(final JoinPoint joinPoint, final Exception e) {

            // 获得MyLog注解
            MyLog controllerLog = getAnnotationLog(joinPoint);
            if (controllerLog == null) {
                return;
            }
            SysOperLog operLog = new SysOperLog();
            // 操作状态（0正常 1异常）
            operLog.setStatus(0);
            // 操作时间
            operLog.setOperTime(new Date());
            if (e != null) {
                operLog.setStatus(1);
                // IotLicenseException为本系统自定义的异常类，读者若要获取异常信息，请根据自身情况变通
                operLog.setErrorMsg(StringUtils.substring(((IotLicenseException) e).getMsg(), 0, 2000));
            }

            // UserUtils.getUri();获取方法上的路径 如：/login，本文实现方法如下：
            // 1、在拦截器中 String uri = request.getRequestURI();
            // 2、用ThreadLocal存放uri，UserUtils.setUri(uri);
            // 3、UserUtils.getUri();
            String uri = UserUtils.getUri();
            operLog.setUri(uri);
            // 处理注解上的参数
            getControllerMethodDescription(joinPoint, controllerLog, operLog);
            // 保存数据库
            sysOperLogService.addOperlog(operLog.getTitle(), operLog.getBusinessType(), operLog.getUri(), operLog.getStatus(), operLog.getOptParam(), operLog.getErrorMsg(), operLog.getOperTime());
        }

        /**
* 是否存在注解，如果存在就获取，不存在则返回null
* @param joinPoint
* @return
*/
        private MyLog getAnnotationLog(JoinPoint joinPoint) {
        Signature signature = joinPoint.getSignature();
        MethodSignature methodSignature = (MethodSignature) signature;
        Method method = methodSignature.getMethod();
        if (method != null) {
        return method.getAnnotation(MyLog.class);
        }
        return null;
        }

        /**
        * 获取Controller层上MyLog注解中对方法的描述信息
        * @param joinPoint 切点
        * @param myLog 自定义的注解
        * @param operLog 操作日志实体类
        */
        private void getControllerMethodDescription(JoinPoint joinPoint, MyLog myLog, SysOperLog operLog) {
        // 设置业务类型（0其它 1新增 2修改 3删除）
        operLog.setBusinessType(myLog.businessType().ordinal());
        // 设置模块标题，eg:登录
        operLog.setTitle(myLog.title());
        // 对方法上的参数进行处理，处理完：userName=xxx,password=xxx
        String optParam = getAnnotationValue(joinPoint, myLog.optParam());
        operLog.setOptParam(optParam);

        }

        /**
        * 对方法上的参数进行处理
        * @param joinPoint
        * @param name
        * @return
        */
        private String getAnnotationValue(JoinPoint joinPoint, String name) {
        String paramName = name;
        // 获取方法中所有的参数
        Map<String, Object> params = getParams(joinPoint);
        // 参数是否是动态的:#{paramName}
        if (paramName.matches("^#\\{\\D*\\}")) {
        // 获取参数名,去掉#{ }
        paramName = paramName.replace("#{", "").replace("}", "");
        // 是否是复杂的参数类型:对象.参数名
        if (paramName.contains(".")) {
        String[] split = paramName.split("\\.");
        // 获取方法中对象的内容
        Object object = getValue(params, split[0]);
        // 转换为JsonObject
        JSONObject jsonObject = (JSONObject) JSONObject.toJSON(object);
        // 获取值
        Object o = jsonObject.get(split[1]);
        return String.valueOf(o);
        } else {// 简单的动态参数直接返回
        StringBuilder str = new StringBuilder();
        String[] paraNames = paramName.split(",");
        for (String paraName : paraNames) {

        String val = String.valueOf(getValue(params, paraName));
        // 组装成 userName=xxx,password=xxx,
        str.append(paraName).append("=").append(val).append(",");
        }
        // 去掉末尾的,
        if (str.toString().endsWith(",")) {
        String substring = str.substring(0, str.length() - 1);
        return substring;
        } else {
        return str.toString();
        }
        }
        }
        // 非动态参数直接返回
        return name;
        }

        /**
        * 获取方法上的所有参数，返回Map类型, eg: 键："userName",值:xxx  键："password",值:xxx
        * @param joinPoint
        * @return
        */
        public Map<String, Object> getParams(JoinPoint joinPoint) {
        Map<String, Object> params = new HashMap<>(8);
        // 通过切点获取方法所有参数值["zhangsan", "123456"]
        Object[] args = joinPoint.getArgs();
        // 通过切点获取方法所有参数名 eg:["userName", "password"]
        MethodSignature signature = (MethodSignature) joinPoint.getSignature();
        String[] names = signature.getParameterNames();
        for (int i = 0; i < args.length; i++) {
        params.put(names[i], args[i]);
        }
        return params;
        }

        /**
        * 从map中获取键为paramName的值，不存在放回null
        * @param map
        * @param paramName
        * @return
        */
        private Object getValue(Map<String, Object> map, String paramName) {
        for (Map.Entry<String, Object> entry : map.entrySet()) {
        if (entry.getKey().equals(paramName)) {
        return entry.getValue();
        }
        }
        return null;
        }
        }
```
<a name="OBEQU"></a>
## MyLog 注解的使用
```java
@GetMapping("login")
@MyLog(title = "登录", optParam = "#{userName},#{password}", businessType = BusinessType.OTHER)
public DataResult login(@RequestParam("userName") String userName, @RequestParam("password") String password) {
 ...
}
```
<a name="kKa8M"></a>
## 最终效果
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1685974290625-5083dac3-e478-468b-94a3-4b325a5054f6.webp#averageHue=%23f0f2ec&clientId=u24e77238-6461-4&from=paste&id=ue5970bae&originHeight=232&originWidth=1409&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=ue708773d-3a5a-4584-b9cd-df69f496da8&title=)

 
