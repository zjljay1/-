:::info
**Sa-Token** 是一个轻量级 Java 权限认证框架，主要解决：**登录认证**、**权限认证**、**单点登录**、**OAuth2.0**、**分布式Session会话**、**微服务网关鉴权** 等一系列权限相关问题。
:::
<a name="vFLOz"></a>
## springboot集成Sa-Token

- 导入依赖
```xml
        <!-- Sa-Token 权限认证, 在线文档：http://sa-token.dev33.cn/ -->
        <dependency>
            <groupId>cn.dev33</groupId>
            <artifactId>sa-token-spring-boot-starter</artifactId>
            <version>1.30.0</version>
        </dependency>
        <dependency>
            <groupId>cn.dev33</groupId>
            <artifactId>sa-token-jwt</artifactId>
            <version>1.30.0</version>
        </dependency>

```

- 配置
```yaml
server:
# 端口
port: 8081

############## Sa-Token 配置 (文档: https://sa-token.cc) ##############
sa-token: 
  # token名称 (同时也是cookie名称)
  token-name: satoken
  # token有效期，单位s 默认30天, -1代表永不过期 
  timeout: 2592000
  # token临时有效期 (指定时间内无操作就视为token过期) 单位: 秒
  activity-timeout: -1
  # 是否允许同一账号并发登录 (为true时允许一起登录, 为false时新登录挤掉旧登录) 
  is-concurrent: true
  # 在多人登录同一账号时，是否共用一个token (为true时所有登录共用一个token, 为false时每次登录新建一个token) 
  is-share: true
  # token风格
  token-style: uuid
  # 是否输出操作日志 
  is-log: false
```
注解鉴权
```java
@Configuration
public class SaTokenConfigure implements WebMvcConfigurer {
    // 注册 Sa-Token 拦截器，打开注解式鉴权功能
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        // 注册 Sa-Token 拦截器，打开注解式鉴权功能
//        registry.addInterceptor(new SaInterceptor()).addPathPatterns("/**");
        registry.addInterceptor(new SaAnnotationInterceptor()).addPathPatterns("/**");
    }
}

```

- 权限认证
```java

import cn.dev33.satoken.stp.StpInterface;
import com.programminglearningplatformonline.mapper.UserMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import com.alibaba.fastjson.JSONObject;

import java.util.ArrayList;
import java.util.List;

/**
* 自定义权限验证接口扩展
*/
@Component    // 保证此类被SpringBoot扫描，完成Sa-Token的自定义权限验证扩展
    public class StpInterfaceImpl implements StpInterface {

        @Autowired
        UserMapper userMapper;

        /**
* 返回一个账号所拥有的权限码集合
* @return
*/
        @Override
        public List<String> getPermissionList(Object loginId, String loginType) {
            String permission = userMapper.getPermissions((String) loginId);
            if(!permission.equals("")){
                return toListString(permission);
            }
            return null;
        }

        /**
* 返回一个账号所拥有的角色标识集合 (权限与角色可分开校验)
*/
        @Override
        public List<String> getRoleList(Object loginId, String loginType) {
            String role = userMapper.getRole(((String) loginId));
            if(!role.equals("")){
                return toListString(role);
            }
            return null;
        }

        /**
* 将JSON字符串转成List<String>
* @param argument JSON化的数组字符串
* @return Java能识别的List<String>
*/
        public List<String> toListString(String argument){
            Object[] array = JSONObject.parseArray(argument).toArray();
            List<String> result = new ArrayList<String>();
            for (Object item : array) {
                result.add(item.toString());
            }
            return result;
        }
    }

```

- 异常拦截
```java

import cn.dev33.satoken.exception.*;
import com.programminglearningplatformonline.utils.common.Result;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;


@RestControllerAdvice
public class SaTokenExceptionHandler {
    @ExceptionHandler(NotLoginException.class)
    public Result handlerNotLoginException(NotLoginException nle) {
        // 不同异常返回不同状态码
        String message = "";
        if (nle.getType().equals(NotLoginException.NOT_TOKEN)) {
            message = "未提供Token";
        } else if (nle.getType().equals(NotLoginException.INVALID_TOKEN)) {
            return Result.fail(401, "未提供有效的Token");
        } else if (nle.getType().equals(NotLoginException.TOKEN_TIMEOUT)) {
            message = "登录信息已过期，请重新登录";
        } else if (nle.getType().equals(NotLoginException.BE_REPLACED)) {
            message = "您的账户已在另一台设备上登录，如非本人操作，请立即修改密码";
        } else if (nle.getType().equals(NotLoginException.KICK_OUT)) {
            message = "已被系统强制下线";
        } else {
            message = "当前会话未登录";
        }
        // 返回给前端
        return Result.fail(403, message);
    }

    @ExceptionHandler
    public Result handlerNotRoleException(NotRoleException e) {
        return  Result.fail(401, "无此角色：" + e.getRole());
    }

    @ExceptionHandler
    public Result handlerNotPermissionException(NotPermissionException e) {
        return  Result.fail(401, "无此权限：" + e.getCode());
    }

    @ExceptionHandler
    public  Result handlerDisableLoginException(DisableLoginException e) {
        return  Result.fail(401, "账户被封禁：" + e.getDisableTime() + "秒后解封");
    }

    @ExceptionHandler
    public  Result handlerNotSafeException(NotSafeException e) {
        return Result.fail(401, "二级认证异常：" + e.getMessage());
    }
    
}
```
