[Spring Boot读取配置文件常用方式_@autowired private environment environment;_嘉禾嘉宁papa的博客-CSDN博客](https://blog.csdn.net/Alian_1223/article/details/118891954)
<a name="oMSXr"></a>
## 简介
实际开发过程中，我们经常要获取配置文件的值，来实现我们程序的可配置化，今天我们就来看看Spring Boot里获取配置文件值的几种方式。
<a name="knLAM"></a>
## 依赖和配置
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.5.2</version>
    <relativePath/>
  </parent>

  <groupId>com.alian</groupId>
  <artifactId>properties</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <name>properties</name>
  <description>Spring Boot读取配置文件的值</description>

  <properties>
    <java.version>1.8</java.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter</artifactId>
      <version>${parent.version}</version>
    </dependency>

    <!--@PropertySource使用到-->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-configuration-processor</artifactId>
      <version>${parent.version}</version>
      <optional>true</optional>
    </dependency>

    <!--本项目也可以不引用-->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
      <version>${parent.version}</version>
    </dependency>

    <!--@Data或日志使用到-->
    <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <version>1.16.14</version>
    </dependency>
  </dependencies>

  <build>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>
</project>

```

- application.yaml配置
```yaml
#如果引用了
#<dependency>
#    <groupId>org.springframework.boot</groupId>
#    <artifactId>spring-boot-starter-web</artifactId>
#    <version>${parent.version}</version>
#</dependency>
#则配置项目名和端口
server
	port:8897
server
	servlet
		context-path: /properties

```
<a name="VCAph"></a>
## 实践
<a name="qBb7K"></a>
### @Value方式获取
```java
@Vlaue
private String configValue;
```
注意：@Value是org.springframework.beans.factory.annotation.Value<br />我们在配置文件application.yaml中新增如下配置，用于测试@Value方式获取配置文件的值
```yaml
#定义@Value的变量测试
#请求地址
value:
	request:
		url: https://openapi.alipay.com/gateway.do
#请求应用id
value:
	request:
		app.id: 2019052960295228
#请求签名key
value:
	request:
		sign:
			key: FFFFEA911121494981C5FEBA599C3A99
#请邱加密key
value:
	request:
		encrypted:
			key: EEEE9CA14DE545768A0BD9F1435EE507

```
@Value测试源程序<br />**ValueService.java**
```java
package com.alian.properties.service;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Service;

import javax.annotation.PostConstruct;

@Service
public class ValueService {

        /**
* 请求地址
*/
        @Value("${value.request.url}")
        private String url;

        /**
* 请求应用id
*/
        @Value("${value.request.app.id}")
        private String appId;

        /**
* 请求签名key
*/
        @Value("${value.request.sign.key}")
        private String signKey;

        /**
* 请求加密key
*/
        @Value("${value.request.encrypted.key}")
        private String encryptedKey;

        /**
* 超时时间（默认值设置，当没有获取到配置值时，用冒号分隔，写在冒号后面）
*/
        @Value("${value.request.timeout:20}")
        private int timeOut;

        @PostConstruct
        public void testValue() {
            System.out.println("-------------------@Value测试开始-------------------");
            System.out.println("@Value测试获取的请求地址：" + url);
            System.out.println("@Value测试获取的请求应用id：" + appId);
            System.out.println("@Value测试获取的请求签名key：" + signKey);
            System.out.println("@Value测试获取的请求加密key：" + encryptedKey);
            //如果配置文件未设置该key的值，则使用默认值
            System.out.println("@Value测试获取的默认值设置：" + timeOut);
            System.out.println("-------------------@Value测试结束-------------------");
        }
    }

```
```java
-------------------@Value测试开始-------------------
@Value测试获取的请求地址：https://openapi.alipay.com/gateway.do
@Value测试获取的请求应用id：2019052960295228
@Value测试获取的请求签名key：FFFFEA911121494981C5FEBA599C3A99
@Value测试获取的请求加密key：EEEE9CA14DE545768A0BD9F1435EE507
@Value测试获取的默认值设置：20
-------------------@Value测试结束-------------------

```
注意：<br />使用@Value方式获取配置文件的值，如果配置项的key不存在，也没有设置默认值，则程序直接报错<br />使用@Value方式默认值的设置方法：配置项的key后面加冒号然后写默认值如：${配置项的key：默认值}<br />使用@Value方式如果是配置文件里配置项太多，并且使用的地方过多的时候，维护和管理不太方便
<a name="Nobvk"></a>
### Environment对象获取
使用很简单，直接使用spring的注解@Autowired引入即可
```java
@Autowired
private Environment environment;
```
注意：Environment 是org.springframework.core.env.Environment<br />我们继续在配置文件application.yaml中新增如下配置，用于测试Environment 方式获取配置文件的值
```yaml
#定义Environment的变量测试
#系统组
envir:
	system:
		group: Alian
#系统组
envir:
	system:
		level: 1
#系统名称
envir:
	system:
		name: administrator
#系统密码
envir:
	system:
		password: e6fa5927cc37437ac6cbe5e988288f80

```
```java
package com.alian.properties.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.core.env.Environment;
import org.springframework.stereotype.Service;

import javax.annotation.PostConstruct;

@Service
public class EnvironmentService {

    @Autowired
    private Environment environment;

    @PostConstruct
    public void testEnvironment() {
        System.out.println("-------------------Environment测试开始-------------------");
        System.out.println("Environment测试获取的系统组：" + environment.getProperty("envir.system.group"));
        System.out.println("Environment测试获取的系统级别：" + environment.getProperty("envir.system.level"));
        System.out.println("Environment测试获取的系统名：" + environment.getProperty("envir.system.name"));
        System.out.println("Environment测试获取的系统密码：" + environment.getProperty("envir.system.password"));
        //如果配置文件未设置该key的值，则使用默认值
        System.out.println("Environment测试获取的默认值设置：" + environment.getProperty("envir.system.init", "未设置初始化参数"));
        System.out.println("-------------------Environment测试结束-------------------");
    }
}
```
```java
-------------------Environment测试开始-------------------
Environment测试获取的系统组：Alian
Environment测试获取的系统级别：1
Environment测试获取的系统名：administrator
Environment测试获取的系统密码：e6fa5927cc37437ac6cbe5e988288f80
Environment测试获取的默认值设置：未设置初始化参数
-------------------Environment测试结束-------------------
```
注意：

- 使用Environment对象获取配置文件的值，最好使用带默认值的方法：getProperty(“配置项key”,“默认值”)，避免null值
- 使用Environment对象还可以获取到一些系统的启动信息，当然如果配置项过多也会有维护管理方面的问题
<a name="jfXK1"></a>
### @ConfigurationProperties方式获取
为了更契合java的面向对象，我们采用自动配置的方式映射配置文件属性，配置完成后直接当做java对象即可使用。我们继续在配置文件application.yaml中新增如下配置，用于测试@ConfigurationProperties方式获取配置文件的值
```yaml
##定义Properties的变量测试
#作者
app:
	author-name: Alian

#博客网站
app:
 	web-url: https://blog.csdn.net/Alian_1223

#小程序应用id
app:
	micro-applet:
		app-id: wx4etd7e3803c6c555

#小程序应用secretId
app:
	micro-applet:
		secret-id: e6fa5627cc537ac8cbe5e988288f80

#小程序超时时间
app:
	micro-applet:
		expire-time: 3600

```
废话不多少，直接新建一个配置类AppProperties.java

```java
package com.alian.properties.config;

import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

@Data
@Component
@ConfigurationProperties(value = "app")
public class AppProperties {

    /**
     * 作者名称
     */
    private String authorName = "";

    /**
     * 博客网站
     */
    private String webUrl = "https://blog.csdn.net/Alian_1223";

    /**
     * 小程序配置
     */
    private MicroApplet microApplet;

    @Data
    public static class MicroApplet {
        /**
         * 应用id
         */
        private String appId = "";
        /**
         * secretId
         */
        private String secretId = "";
        /**
         * 过期时间
         */
        private int expireTime = 30;
    }
}

```
注意：

- @ConfigurationProperties(value = “app”)表示的配置文件里属性的前缀都是app开头
- 配置类上记得加上@Data和@Component注解（或者在启动类上加上@EnableConfigurationProperties(value = AppProperties.class)）
- 如果有内部类对象，记得加上@Data，不然无法映射数据
- properties类型文件映射规则，短横线(-)后面的首个字母会变成大写，同时注意有内部类时的写法<br /> 

使用方法也很简单，直接使用spring的注解@Autowired引入即可
```java
@Autowired
    private AppProperties appProperties;
```
```java
package com.alian.properties.service;

import com.alian.properties.config.AppProperties;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import javax.annotation.PostConstruct;

@Service
public class PropertiesService {

    @Autowired
    private AppProperties appProperties;

    @PostConstruct
    public void testProperties() {
        System.out.println("-------------------Properties测试开始-------------------");
        System.out.println("Properties测试获取的作者：" + appProperties.getAuthorName());
        System.out.println("Properties测试获取的博客地址：" + appProperties.getWebUrl());
        System.out.println("Properties测试获取的小程序应用id：" + appProperties.getMicroApplet().getAppId());
        System.out.println("Properties测试获取的小程序SecretId：" + appProperties.getMicroApplet().getSecretId());
        System.out.println("Properties测试获取的小程序超时时间：" + appProperties.getMicroApplet().getExpireTime());
        System.out.println("-------------------Properties测试结束-------------------");
    }
}

```
```java
-------------------Properties测试开始-------------------
Properties测试获取的作者：Alian
Properties测试获取的博客地址：https://blog.csdn.net/Alian_1223
Properties测试获取的小程序应用id：wx4etd7e3803c6c555
Properties测试获取的小程序SecretId：e6fa5627cc57437ac8cbe5e988288f80
Properties测试获取的小程序超时时间：3600
-------------------Properties测试结束-------------------

```
注意：

- 类型转换少，配置类可以直接定义常规类型
- 配置分类方便，一个地方维护，不用一个key到处写
- 更符合面向对象的写法
- Spring Boot注解读取application.properties或者application-{profile}.properties文件时默认编码是ISO_8859_1，读取yaml配置文件时使用的是UTF-8的编码方式，如果有中文配置请使用.yml格式，或者使用我接下来的读取方式。
