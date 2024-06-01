<a name="aLKrQ"></a>
# Redis集成
Redis是我们Java开发中，使用频次非常高的一个nosql数据库，数据以key-value键值对的形式存储在内存中。redis的常用使用场景，可以做缓存，分布式锁，自增序列等，使用redis的方式和我们使用数据库的方式差不多，首先我们要在自己的本机电脑或者服务器上安装一个redis的服务器，通过我们的java客户端在程序中进行集成，然后通过客户端完成对redis的增删改查操作。redis的Java客户端类型还是很多的，常见的有**jedis, redission,lettuce**等，所以我们在集成的时候，我们可以选择直接集成这些原生客户端。但是在springBoot中更常见的方式是集成**spring-data-redis**，这是spring提供的一个专门用来操作redis的项目，封装了对redis的常用操作，里边主要封装了jedis和lettuce两个客户端。相当于是在他们的基础上加了一层门面。
<a name="XDsCz"></a>
## springboot集成
<a name="fA6UH"></a>
### 添加依赖
```xml
<!-- 集成redis依赖  -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>

```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.lsqingfeng.springboot</groupId>
    <artifactId>springboot-learning</artifactId>
    <version>1.0.0</version>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.6.2</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.22</version>
            <scope>provided</scope>
        </dependency>

        <!-- mybatis-plus 所需依赖  -->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.5.1</version>
        </dependency>

        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-generator</artifactId>
            <version>3.5.1</version>
        </dependency>

        <dependency>
            <groupId>org.freemarker</groupId>
            <artifactId>freemarker</artifactId>
            <version>2.3.31</version>
        </dependency>

        <!-- 开发热启动 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
        </dependency>

        <!-- MySQL连接 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>

        <!-- 集成redis依赖  -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
    </dependencies>


</project>

```
这里我们直接引入了spring-boot-starter-data-redis这个springBoot本身就已经提供好了的starter, 我们可以点击去看一下这个starter中包含了哪些依赖：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1690889134190-67bd2272-7a44-45f4-89f3-100afcc0ff28.png#averageHue=%23fdfcfc&clientId=u38ce344b-096a-4&from=paste&height=409&id=ud26a25fc&originHeight=562&originWidth=653&originalType=binary&ratio=1.375&rotation=0&showTitle=false&size=63914&status=done&style=none&taskId=uc056b09f-8a93-4ca2-893c-4bbbbdec310&title=&width=474.90909090909093)<br />可以发现，里面包含了spring-data-redis和 lettuce-core两个核心包，这就是为什么说我们的spring-boot-starter-data-redis默认使用的就是lettuce这个客户端了。<br />如果我们想要使用jedis客户端怎么办呢？就需要排除lettuce这个依赖，再引入jedis的相关依赖就可以了。

那么为什么我们只需要通过引入不同的依赖就能让spring-data-redis可以自由切换客户端呢，这其实就涉及到了springBoot的自动化配置原理。我们可以给大家简单讲解一下。

springBoot这个框架之所以可以通过各种starter无缝融合其他技术的一大主要原因就是springBoot本身的自动化配置功能。所谓自动化配置就是springBoot本身已经预先设置好了一些常用框架的整合类。然后通过类似于ConditionOn这样的条件判断注解，去辨别你的项目中是否有相关的类（或配置）了，进而进行相关配置的初始化。

springBoot预设的自动化配置类都位于spring-boot-autoconfigure这个包中，只要我们搭建了springBoot的项目，这个包就会被引入进来。<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1690889268539-ead291a1-90f6-44ba-aee9-34c851b44c6a.webp#averageHue=%23f8f8dc&clientId=u38ce344b-096a-4&from=paste&id=ua8d9642a&originHeight=1566&originWidth=1052&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u4beaebf5-45dd-4c44-b7c8-6c81965b721&title=)而这个包下就有一个RedisAutoConfiguration这个类，顾名思义就是Redis的自动化配置。在这个类中，会引入LettuceConnectionConfiguration 和 JedisConnectionConfiguration 两个配置类，分别对应lettuce和jedis两个客户端。<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1690889328831-f3fff488-6c75-4c9d-ba5b-3bec50250e0e.webp#averageHue=%23f9f7f6&clientId=u38ce344b-096a-4&from=paste&id=u0db81b5d&originHeight=712&originWidth=1136&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u234f5323-f118-4685-83fe-25daad07fbf&title=)而这个两个类上都是用了ConditionOn注解来进行判断是否加载。<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1690889380628-e23bcc4b-03a7-46e0-8608-afb81f965a57.webp#averageHue=%23f6f4f3&clientId=u38ce344b-096a-4&from=paste&id=u529b9c3f&originHeight=756&originWidth=1102&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u36726de9-b153-47c6-a0dd-4303db7a5ea&title=)<br /> jedis如下；<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1690889393977-c8964169-01b8-4d65-872c-576507ed73bf.webp#averageHue=%23f4f2f0&clientId=u38ce344b-096a-4&from=paste&id=u1b16cbf9&originHeight=520&originWidth=1110&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=ua4711d25-242a-4469-95a6-d32c512b915&title=)<br /> 而由于我们的项目自动引入了lettuce-core，而没有引入jedis相关依赖，所以LettuceConnectionConfiguration这个类的判断成立会被加载，而Jedis的判断不成立，所以不会加载。进而lettuce的配置生效，所以我们在使用的使用， 默认就是lettuce的客户端。
<a name="yhWjK"></a>
### 添加配置
然后我们需要配置连接redis所需的账号密码等信息，这里大家要提前安装好redis,保证我们的本机程序可以连接到我们的redis， 如果不知道redis如何安装，可以参考文章: [Linux系统安装redis6.0.5]<br />[Linux系统安装redis6.0.5_redis6.0.5安装_一缕82年的清风的博客-CSDN博客](https://blog.csdn.net/lsqingfeng/article/details/107359076?ops_request_misc=%7B%22request%5Fid%22%3A%22164663221016780366526405%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fblog.%22%7D&request_id=164663221016780366526405&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_ecpm_v1~rank_v31_ecpm-1-107359076.nonecase&utm_term=redis&spm=1018.2226.3001.4450&ydreferer=aHR0cHM6Ly9saW5rLmp1ZWppbi5jbi8%3D)<br />常规配置如下： 在application.yml配置文件中配置 redis的连接信息
```yaml
spring:
  redis:
    host: localhost
    port: 6379
    password: 123456
    database: 0
```
如果有其他配置放到一起：
```yaml
server:
  port: 19191

spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/springboot_learning?serverTimezone=Asia/Shanghai&characterEncoding=utf-8
    username: root
    password: root
  redis:
    host: localhost
    port: 6379
    password: 123456
    database: 0
    lettuce:
      pool:
        max-idle: 16
        max-active: 32
        min-idle: 8
  devtools:
    restart:
      enable: true


third:
  weather:
    url: http://www.baidu.com
    port: 8080
    username: test
    cities:
      - 北京
      - 上海
      - 广州
    list[0]: aaa
    list[1]: bbb
    list[2]: ccc

```
这样我们就可以直接在项目当中操作redis了。如果使用的是集群，那么使用如下配置方式：
```yaml
spring:
  redis:
    password: 123456
    cluster:
      nodes: 10.255.144.115:7001,10.255.144.115:7002,10.255.144.115:7003,10.255.144.115:7004,10.255.144.115:7005,10.255.144.115:7006
      max-redirects: 3
```
但是有的时候我们想要给我们的redis客户端配置上连接池。就像我们连接mysql的时候，也会配置连接池一样，目的就是增加对于数据连接的管理，提升访问的效率，也保证了对资源的合理利用。那么我们如何配置连接池呢，这里大家一定要注意了，很多网上的文章中，介绍的方法可能由于版本太低，都不是特别的准确。 比如很多人使用spring.redis.pool来配置，这个是不对的（不清楚是不是老版本是这样的配置的，但是在springboot-starter-data-redis中这种写法不对）。首先是配置文件，由于我们使用的lettuce客户端，所以配置的时候，在spring.redis下加上lettuce再加上pool来配置，具体如下；
```yaml
spring:
  redis:
    host: 10.255.144.111
    port: 6379
    password: 123456
    database: 0
    lettuce:
      pool:
        max-idle: 16
        max-active: 32
        min-idle: 8
```
**如果使用的是jedis,就把lettuce换成jedis（同时要注意依赖也是要换的）。**<br />但是仅仅这在配置文件中加入，其实连接池是不会生效的。这里大家一定要注意，很多同学在配置文件上加上了这段就以为连接池已经配置好了，其实并没有，还少了最关键的一步，就是要导入一个依赖，不导入的话，这么配置也没有用。
```xml
<dependency>
  <groupId>org.apache.commons</groupId>
  <artifactId>commons-pool2</artifactId>
</dependency>
```
之后，连接池才会生效。我们可以做一个对比。 在导包前后，观察RedisTemplate对象的值就可以看出来。<br />导入之前：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1690889544377-41e5e69a-edd6-4449-a54c-0721430ecd63.webp#averageHue=%23f9f6f6&clientId=u38ce344b-096a-4&from=paste&id=u0dbc88de&originHeight=992&originWidth=1166&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u0429863e-4dbc-49e4-9ce8-edc1e110965&title=)导入之后：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1690889552896-ddcc93c6-957b-4c98-aea8-a4f2ec59e336.webp#averageHue=%23faf8f8&clientId=u38ce344b-096a-4&from=paste&id=u830bc43b&originHeight=1084&originWidth=1220&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=ubea2e692-0d3b-4589-bcb4-936e64e4390&title=)<br /> 导入之后，我们的连接池信息才有值，这也印证了我们上面的结论。<br />具体的配置信息我们可以看一下源代码，源码中使用RedisProperties 这个类来接收redis的配置参数。 ![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1690889707914-709875eb-4dda-4435-ba08-0ec1b76d30c5.webp#averageHue=%23fbfaf9&clientId=u38ce344b-096a-4&from=paste&id=u475b66f0&originHeight=906&originWidth=1076&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u2326788a-2bb2-41bd-bc79-a351b482f29&title=)
<a name="mz7Me"></a>
### 项目中使用
我们的配置工作准备就绪以后，我们就可以在项目中操作redis了，操作的话，使用spring-data-redis中为我们提供的 RedisTemplate 这个类，就可以操作了。我们先举个简单的例子，插入一个键值对（值为string）。
```java
package com.lsqingfeng.springboot.controller;

import com.lsqingfeng.springboot.base.Result;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

/**
 * @className: RedisController
 * @description:
 * @author: sh.Liu
 * @date: 2022-03-08 14:28
 */
@RestController
@RequestMapping("redis")
public class RedisController {

    private final RedisTemplate redisTemplate;

    public RedisController(RedisTemplate redisTemplate) {
        this.redisTemplate = redisTemplate;
    }

    @GetMapping("save")
    public Result save(String key, String value){
        redisTemplate.opsForValue().set(key, value);
        return Result.success();
    }

}
```
<a name="cuJcM"></a>
## 工具类封装
我们在前面的代码中已经通过RedisTemplate成功操作了redis服务器，比如set一个字符串，我们可以使用：
```java
redisTemplate.opsForValue().set(key, value);
```
来put一个String类型的键值对。而redis中可以支持 string, list, hash,set, zset五种数据格式，这五种数据格式的常用操作，都在RedisTemplate这个类中进行了封装。 操作string类型就是用opsForValue,操作list类型是用listOps, 操作set类型是用setOps等等。我们可以通过查看RedisTemplate这个类中的源码来了解大致有哪些功能。<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1690890089193-377b6dff-ea81-4c12-936c-be216332d736.webp#averageHue=%23f3f2f1&clientId=u38ce344b-096a-4&from=paste&id=u36650c30&originHeight=724&originWidth=1142&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u6f88097a-34ae-40d7-9720-204c3416827&title=)而这些功能都在这一个类中，使用起来其实并不是很方便，所有一般情况下，我们都是单独封装一个工具类，来把常用的一些方法进行抽象。操作的时候，直接通过工具类来操作
```java
 
package com.lsqingfeng.springboot.utils;
 
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Component;
 
import java.util.List;
import java.util.Map;
import java.util.Set;
import java.util.concurrent.TimeUnit;
 
/**
 * @className: RedisUtil
 * @description:
 * @author: sh.Liu
 * @date: 2022-03-09 14:07
 */
@Component
public class RedisUtil {
 
    @Autowired
    private RedisTemplate redisTemplate;
    /**
     * 给一个指定的 key 值附加过期时间
     *
     * @param key
     * @param time
     * @return
     */
    public boolean expire(String key, long time) {
        return redisTemplate.expire(key, time, TimeUnit.SECONDS);
    }
    /**
     * 根据key 获取过期时间
     *
     * @param key
     * @return
     */
    public long getTime(String key) {
        return redisTemplate.getExpire(key, TimeUnit.SECONDS);
    }
    /**
     * 根据key 获取过期时间
     *
     * @param key
     * @return
     */
    public boolean hasKey(String key) {
        return redisTemplate.hasKey(key);
    }
    /**
     * 移除指定key 的过期时间
     *
     * @param key
     * @return
     */
    public boolean persist(String key) {
        return redisTemplate.boundValueOps(key).persist();
    }
 
    //- - - - - - - - - - - - - - - - - - - - -  String类型 - - - - - - - - - - - - - - - - - - - -
 
    /**
     * 根据key获取值
     *
     * @param key 键
     * @return 值
     */
    public Object get(String key) {
        return key == null ? null : redisTemplate.opsForValue().get(key);
    }
 
    /**
     * 将值放入缓存
     *
     * @param key   键
     * @param value 值
     * @return true成功 false 失败
     */
    public void set(String key, String value) {
        redisTemplate.opsForValue().set(key, value);
    }
 
    /**
     * 将值放入缓存并设置时间
     *
     * @param key   键
     * @param value 值
     * @param time  时间(秒) -1为无期限
     * @return true成功 false 失败
     */
    public void set(String key, String value, long time) {
        if (time > 0) {
            redisTemplate.opsForValue().set(key, value, time, TimeUnit.SECONDS);
        } else {
            redisTemplate.opsForValue().set(key, value);
        }
    }
 
    /**
     * 批量添加 key (重复的键会覆盖)
     *
     * @param keyAndValue
     */
    public void batchSet(Map<String, String> keyAndValue) {
        redisTemplate.opsForValue().multiSet(keyAndValue);
    }
 
    /**
     * 批量添加 key-value 只有在键不存在时,才添加
     * map 中只要有一个key存在,则全部不添加
     *
     * @param keyAndValue
     */
    public void batchSetIfAbsent(Map<String, String> keyAndValue) {
        redisTemplate.opsForValue().multiSetIfAbsent(keyAndValue);
    }
 
    /**
     * 对一个 key-value 的值进行加减操作,
     * 如果该 key 不存在 将创建一个key 并赋值该 number
     * 如果 key 存在,但 value 不是长整型 ,将报错
     *
     * @param key
     * @param number
     */
    public Long increment(String key, long number) {
        return redisTemplate.opsForValue().increment(key, number);
    }
 
    /**
     * 对一个 key-value 的值进行加减操作,
     * 如果该 key 不存在 将创建一个key 并赋值该 number
     * 如果 key 存在,但 value 不是 纯数字 ,将报错
     *
     * @param key
     * @param number
     */
    public Double increment(String key, double number) {
        return redisTemplate.opsForValue().increment(key, number);
    }
 
    //- - - - - - - - - - - - - - - - - - - - -  set类型 - - - - - - - - - - - - - - - - - - - -
 
    /**
     * 将数据放入set缓存
     *
     * @param key 键
     * @return
     */
    public void sSet(String key, String value) {
        redisTemplate.opsForSet().add(key, value);
    }
 
    /**
     * 获取变量中的值
     *
     * @param key 键
     * @return
     */
    public Set<Object> members(String key) {
        return redisTemplate.opsForSet().members(key);
    }
 
    /**
     * 随机获取变量中指定个数的元素
     *
     * @param key   键
     * @param count 值
     * @return
     */
    public void randomMembers(String key, long count) {
        redisTemplate.opsForSet().randomMembers(key, count);
    }
 
    /**
     * 随机获取变量中的元素
     *
     * @param key 键
     * @return
     */
    public Object randomMember(String key) {
        return redisTemplate.opsForSet().randomMember(key);
    }
 
    /**
     * 弹出变量中的元素
     *
     * @param key 键
     * @return
     */
    public Object pop(String key) {
        return redisTemplate.opsForSet().pop("setValue");
    }
 
    /**
     * 获取变量中值的长度
     *
     * @param key 键
     * @return
     */
    public long size(String key) {
        return redisTemplate.opsForSet().size(key);
    }
 
    /**
     * 根据value从一个set中查询,是否存在
     *
     * @param key   键
     * @param value 值
     * @return true 存在 false不存在
     */
    public boolean sHasKey(String key, Object value) {
        return redisTemplate.opsForSet().isMember(key, value);
    }
 
    /**
     * 检查给定的元素是否在变量中。
     *
     * @param key 键
     * @param obj 元素对象
     * @return
     */
    public boolean isMember(String key, Object obj) {
        return redisTemplate.opsForSet().isMember(key, obj);
    }
 
    /**
     * 转移变量的元素值到目的变量。
     *
     * @param key     键
     * @param value   元素对象
     * @param destKey 元素对象
     * @return
     */
    public boolean move(String key, String value, String destKey) {
        return redisTemplate.opsForSet().move(key, value, destKey);
    }
 
    /**
     * 批量移除set缓存中元素
     *
     * @param key    键
     * @param values 值
     * @return
     */
    public void remove(String key, Object... values) {
        redisTemplate.opsForSet().remove(key, values);
    }
 
    /**
     * 通过给定的key求2个set变量的差值
     *
     * @param key     键
     * @param destKey 键
     * @return
     */
    public Set<Set> difference(String key, String destKey) {
        return redisTemplate.opsForSet().difference(key, destKey);
    }
 
 
    //- - - - - - - - - - - - - - - - - - - - -  hash类型 - - - - - - - - - - - - - - - - - - - -
 
    /**
     * 加入缓存
     *
     * @param key 键
     * @param map 键
     * @return
     */
    public void add(String key, Map<String, String> map) {
        redisTemplate.opsForHash().putAll(key, map);
    }
 
    /**
     * 获取 key 下的 所有  hashkey 和 value
     *
     * @param key 键
     * @return
     */
    public Map<Object, Object> getHashEntries(String key) {
        return redisTemplate.opsForHash().entries(key);
    }
 
    /**
     * 验证指定 key 下 有没有指定的 hashkey
     *
     * @param key
     * @param hashKey
     * @return
     */
    public boolean hashKey(String key, String hashKey) {
        return redisTemplate.opsForHash().hasKey(key, hashKey);
    }
 
    /**
     * 获取指定key的值string
     *
     * @param key  键
     * @param key2 键
     * @return
     */
    public String getMapString(String key, String key2) {
        return redisTemplate.opsForHash().get("map1", "key1").toString();
    }
 
    /**
     * 获取指定的值Int
     *
     * @param key  键
     * @param key2 键
     * @return
     */
    public Integer getMapInt(String key, String key2) {
        return (Integer) redisTemplate.opsForHash().get("map1", "key1");
    }
 
    /**
     * 弹出元素并删除
     *
     * @param key 键
     * @return
     */
    public String popValue(String key) {
        return redisTemplate.opsForSet().pop(key).toString();
    }
 
    /**
     * 删除指定 hash 的 HashKey
     *
     * @param key
     * @param hashKeys
     * @return 删除成功的 数量
     */
    public Long delete(String key, String... hashKeys) {
        return redisTemplate.opsForHash().delete(key, hashKeys);
    }
 
    /**
     * 给指定 hash 的 hashkey 做增减操作
     *
     * @param key
     * @param hashKey
     * @param number
     * @return
     */
    public Long increment(String key, String hashKey, long number) {
        return redisTemplate.opsForHash().increment(key, hashKey, number);
    }
 
    /**
     * 给指定 hash 的 hashkey 做增减操作
     *
     * @param key
     * @param hashKey
     * @param number
     * @return
     */
    public Double increment(String key, String hashKey, Double number) {
        return redisTemplate.opsForHash().increment(key, hashKey, number);
    }
 
    /**
     * 获取 key 下的 所有 hashkey 字段
     *
     * @param key
     * @return
     */
    public Set<Object> hashKeys(String key) {
        return redisTemplate.opsForHash().keys(key);
    }
 
    /**
     * 获取指定 hash 下面的 键值对 数量
     *
     * @param key
     * @return
     */
    public Long hashSize(String key) {
        return redisTemplate.opsForHash().size(key);
    }
 
    //- - - - - - - - - - - - - - - - - - - - -  list类型 - - - - - - - - - - - - - - - - - - - -
 
    /**
     * 在变量左边添加元素值
     *
     * @param key
     * @param value
     * @return
     */
    public void leftPush(String key, Object value) {
        redisTemplate.opsForList().leftPush(key, value);
    }
 
    /**
     * 获取集合指定位置的值。
     *
     * @param key
     * @param index
     * @return
     */
    public Object index(String key, long index) {
        return redisTemplate.opsForList().index("list", 1);
    }
 
    /**
     * 获取指定区间的值。
     *
     * @param key
     * @param start
     * @param end
     * @return
     */
    public List<Object> range(String key, long start, long end) {
        return redisTemplate.opsForList().range(key, start, end);
    }
 
    /**
     * 把最后一个参数值放到指定集合的第一个出现中间参数的前面，
     * 如果中间参数值存在的话。
     *
     * @param key
     * @param pivot
     * @param value
     * @return
     */
    public void leftPush(String key, String pivot, String value) {
        redisTemplate.opsForList().leftPush(key, pivot, value);
    }
 
    /**
     * 向左边批量添加参数元素。
     *
     * @param key
     * @param values
     * @return
     */
    public void leftPushAll(String key, String... values) {
//        redisTemplate.opsForList().leftPushAll(key,"w","x","y");
        redisTemplate.opsForList().leftPushAll(key, values);
    }
 
    /**
     * 向集合最右边添加元素。
     *
     * @param key
     * @param value
     * @return
     */
    public void leftPushAll(String key, String value) {
        redisTemplate.opsForList().rightPush(key, value);
    }
 
    /**
     * 向左边批量添加参数元素。
     *
     * @param key
     * @param values
     * @return
     */
    public void rightPushAll(String key, String... values) {
        //redisTemplate.opsForList().leftPushAll(key,"w","x","y");
        redisTemplate.opsForList().rightPushAll(key, values);
    }
 
    /**
     * 向已存在的集合中添加元素。
     *
     * @param key
     * @param value
     * @return
     */
    public void rightPushIfPresent(String key, Object value) {
        redisTemplate.opsForList().rightPushIfPresent(key, value);
    }
 
    /**
     * 向已存在的集合中添加元素。
     *
     * @param key
     * @return
     */
    public long listLength(String key) {
        return redisTemplate.opsForList().size(key);
    }
 
    /**
     * 移除集合中的左边第一个元素。
     *
     * @param key
     * @return
     */
    public void leftPop(String key) {
        redisTemplate.opsForList().leftPop(key);
    }
 
    /**
     * 移除集合中左边的元素在等待的时间里，如果超过等待的时间仍没有元素则退出。
     *
     * @param key
     * @return
     */
    public void leftPop(String key, long timeout, TimeUnit unit) {
        redisTemplate.opsForList().leftPop(key, timeout, unit);
    }
 
    /**
     * 移除集合中右边的元素。
     *
     * @param key
     * @return
     */
    public void rightPop(String key) {
        redisTemplate.opsForList().rightPop(key);
    }
 
    /**
     * 移除集合中右边的元素在等待的时间里，如果超过等待的时间仍没有元素则退出。
     *
     * @param key
     * @return
     */
    public void rightPop(String key, long timeout, TimeUnit unit) {
        redisTemplate.opsForList().rightPop(key, timeout, unit);
    }
}

```
<a name="HcyhK"></a>
## 讲讲序列化
redis的序列化也是我们在使用RedisTemplate的过程中需要注意的事情。上面的案例中，其实我们并没有特殊设置redis的序列化方式，那么它其实使用的是默认的序列化方式。RedisTemplate这个类的泛型是<String,Object>,也就是他是支持写入Object对象的，那么这个对象采取什么方式序列化存入内存中就是它的序列化方式。<br />那么什么是redis的序列化呢？就是我们把对象存入到redis中到底以什么方式存储的，可以是二进制数据，可以是xml也可以是json。比如说我们经常会将POJO 对象存储到 Redis 中，一般情况下会使用 JSON 方式序列化成字符串，存储到 Redis 中 。<br />Redis本身提供了一下一种序列化的方式：

- GenericToStringSerializer: 可以将任何对象泛化为字符串并序列化
- Jackson2JsonRedisSerializer: 跟JacksonJsonRedisSerializer实际上是一样的
- JacksonJsonRedisSerializer: 序列化object对象为json字符串
- JdkSerializationRedisSerializer: 序列化java对象
- StringRedisSerializer: 简单的字符串序列化

如果我们存储的是String类型，默认使用的是StringRedisSerializer 这种序列化方式。如果我们存储的是对象，默认使用的是 JdkSerializationRedisSerializer，也就是Jdk的序列化方式（通过ObjectOutputStream和ObjectInputStream实现，缺点是我们无法直观看到存储的对象内容）。<br />我们可以根据redis操作的不同数据类型，设置对应的序列化方式。![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1690890230725-bcd7514f-61ee-4c31-aa64-43820bafb45d.webp#averageHue=%23f8f5f3&clientId=u38ce344b-096a-4&from=paste&id=uf8e1aa2f&originHeight=774&originWidth=1098&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=ubb2b1276-e38b-41ca-b10d-c021d312562&title=)通过观察RedisTemplate的源码我们就可以看出来，默认使用的是JdkSerializationRedisSerializer. 这种序列化最大的问题就是存入对象后，我们很难直观看到存储的内容，很不方便我们排查问题：![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1690890263077-637aef47-a536-4353-8232-5fc08ef150ed.webp#averageHue=%23f8f7f7&clientId=u38ce344b-096a-4&from=paste&id=ud0ca619a&originHeight=214&originWidth=1096&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u89aaca9d-e934-4f40-8275-c2217ee4821&title=)而一般我们最经常使用的对象序列化方式是： Jackson2JsonRedisSerializer<br />设置序列化方式的主要方法就是我们在配置类中，自己来创建RedisTemplate对象，并在创建的过程中指定对应的序列化方式。
```java
import com.fasterxml.jackson.annotation.JsonAutoDetect;
import com.fasterxml.jackson.annotation.PropertyAccessor;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

@Configuration
public class RedisConfig {
    @Bean(name = "redisTemplate")
    public RedisTemplate<String, Object> getRedisTemplate(RedisConnectionFactory factory) {
        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<String, Object>();
        redisTemplate.setConnectionFactory(factory);
        
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();
        
        redisTemplate.setKeySerializer(stringRedisSerializer); // key的序列化类型

        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper objectMapper = new ObjectMapper();
        objectMapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        // 方法过期，改为下面代码
//        objectMapper.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        objectMapper.activateDefaultTyping(LaissezFaireSubTypeValidator.instance ,
                ObjectMapper.DefaultTyping.NON_FINAL, JsonTypeInfo.As.PROPERTY);
        jackson2JsonRedisSerializer.setObjectMapper(objectMapper);
        jackson2JsonRedisSerializer.setObjectMapper(objectMapper);
       
        redisTemplate.setValueSerializer(jackson2JsonRedisSerializer); // value的序列化类型
        redisTemplate.setHashKeySerializer(stringRedisSerializer);
        redisTemplate.setHashValueSerializer(jackson2JsonRedisSerializer);
        redisTemplate.afterPropertiesSet();
        return redisTemplate;
    }
}

```
<a name="vJtcR"></a>
## 分布式锁
[Redis实现分布式锁的7种方案 - why414 - 博客园](https://www.cnblogs.com/wangyingshuo/p/14510524.html)<br />[Redis实现分布式锁的8大坑！切记！-轻识](https://www.qinglite.cn/doc/870664777eb8ad867)
<a name="QBkhR"></a>
## 工具类(另一版本)
[springboot整合redis，推荐整合和使用案例（2021版）_springboot redis_于大圣的博客-CSDN博客](https://blog.csdn.net/yu102655/article/details/112217778)
:::warning
背景：手下新人在初次使用springboot整合redis，大部分人习惯从网上检索到一份配置，然后不知其所以然的复制粘贴到项目中，网上搜索到的配置良莠不齐但又万变不离其宗。由于springboot最大化地简化了整合redis需要的配置，在用户只需要在配置文件（application.*）中配置少量参数就可以使用官方默认提供的RedisTemplate和StringRedisTemplate来操作redis。由于官方提供的*RedisTemplate提供的功能有限，难以针对java的复杂数据类型进行序列化，且采用直连的方式以及没有对连接数进行限制等诸多因素在现代引用中制约较大，所以项目中一般需要提供一个RedisConfig类来针对redisTemplate做进一步配置
:::

新建一个springboot项目，pom.xml做如下依赖，注意jedis版本与spring-boot-starter-data-redis版本对应，一般来说2.1.x对应jedis 2.9.x，2.2.x对应jedis 3.x，切记，否则可能出现莫名其妙的错误。
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.1.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
  </parent>
  <groupId>thinking-in-spring-boot</groupId>
  <artifactId>first-app-by-gui</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <name>first-app-by-gui</name>
  <description>Demo project for Spring Boot</description>

  <properties>
    <java.version>1.8</java.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-redis</artifactId>
    </dependency>
    <dependency>
      <groupId>redis.clients</groupId>
      <artifactId>jedis</artifactId>
      <version>3.2.0</version>
    </dependency>
    <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
      <scope>test</scope>
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
新建的项目结构如下：
<a name="jWwRC"></a>
### ![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1690893310282-0956191b-89bf-4e3d-b75c-ec8b8d0b6158.png#averageHue=%233f454b&clientId=u38ce344b-096a-4&from=paste&id=u2b2fb6bc&originHeight=707&originWidth=608&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=50358&status=done&style=none&taskId=u0b0f5443-5349-46b4-9d45-69c82cb94b4&title=)
在application.yml配置文件中新增关于redis配置，内容如下：
```yaml
spring:
  redis:
    database: 0
    host: 127.0.0.1
    port: 6379
    password: # 如果未单独配置默认为空即可
    timeout: 1000
    jedis:
      pool:
        max-active: 8
        max-wait: -1
        max-idle: 8
        min-idle: 0
```
编写redis配置类，内容如下，在该类中完成Jedis池、Redis连接和RedisTemplate序列化三个配置完成springboot整合redis的进一步配置。其中RedisTemplate对key和value的序列化类，各人结合自己项目情况进行选择即可。<br />编写redis配置类，内容如下，在该类中完成Jedis池、Redis连接和RedisTemplate序列化三个配置完成springboot整合redis的进一步配置。其中RedisTemplate对key和value的序列化类，各人结合自己项目情况进行选择即可。
```java
package com.dongnao.config;
 
import com.dongnao.cache.IGlobalCache;
import com.dongnao.cache.impl.AppRedisCacheManager;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.cache.annotation.EnableCaching;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Primary;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.connection.RedisStandaloneConfiguration;
import org.springframework.data.redis.connection.jedis.JedisClientConfiguration;
import org.springframework.data.redis.connection.jedis.JedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;
import redis.clients.jedis.JedisPoolConfig;
 
@EnableCaching
@Configuration
public class RedisConfig {
 
    @Value("${spring.redis.host}")
    private String host;
    @Value("${spring.redis.database}")
    private Integer database;
    @Value("${spring.redis.port}")
    private Integer port;
    @Value("${spring.redis.password}")
    private String pwd;
 
    @Primary
    @Bean(name = "jedisPoolConfig")
    @ConfigurationProperties(prefix = "spring.redis.pool")
    public JedisPoolConfig jedisPoolConfig() {
        JedisPoolConfig jedisPoolConfig = new JedisPoolConfig();
        jedisPoolConfig.setMaxWaitMillis(10000);
        return jedisPoolConfig;
    }
 
    @Bean
    public RedisConnectionFactory redisConnectionFactory(JedisPoolConfig jedisPoolConfig) {
        RedisStandaloneConfiguration redisStandaloneConfiguration = new RedisStandaloneConfiguration();
        redisStandaloneConfiguration.setHostName(host);
        redisStandaloneConfiguration.setDatabase(database);
        redisStandaloneConfiguration.setPassword(pwd);
        redisStandaloneConfiguration.setPort(port);
        JedisClientConfiguration.JedisPoolingClientConfigurationBuilder jpcb = (JedisClientConfiguration.JedisPoolingClientConfigurationBuilder) JedisClientConfiguration.builder();
        jpcb.poolConfig(jedisPoolConfig);
        JedisClientConfiguration jedisClientConfiguration = jpcb.build();
        return new JedisConnectionFactory(redisStandaloneConfiguration, jedisClientConfiguration);
    }
 
    /**
     * 配置redisTemplate针对不同key和value场景下不同序列化的方式
     *
     * @param factory Redis连接工厂
     * @return
     */
    @Primary
    @Bean(name = "redisTemplate")
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(factory);
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();
        template.setKeySerializer(stringRedisSerializer);
        template.setHashKeySerializer(stringRedisSerializer);
        Jackson2JsonRedisSerializer redisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        template.setValueSerializer(redisSerializer);
        template.setHashValueSerializer(redisSerializer);
        template.afterPropertiesSet();
        return template;
    }
 
    @Bean
    IGlobalCache cache(RedisTemplate redisTemplate) {
        return new AppRedisCacheManager(redisTemplate);
    }
 
}
```
网络上大部分配置都到此为止，但在实际使用redis缓存的场景，一般由于redisTemplate使用不便、需要解决将java数据类型和redis六种数据结构命令的对应以及不应该直接暴露redisTemplate给service等因素考虑，需要进行进一步对redisTemplate进行封装，封装后代码如下：
```java
package com.dongnao.cache;
 
import org.springframework.data.redis.core.RedisTemplate;
 
import java.util.List;
import java.util.Map;
import java.util.Set;
 
/**
 * 系统全局Cache接口，具体缓存方式需要实现该接口
 *
 * @author YuXD
 * @date 2021-01-05 10:38
 * @since v1.0
 */
public interface IGlobalCache {
 
    /**
     * 指定缓存失效时间
     *
     * @param key  键
     * @param time 时间(秒)
     * @return
     */
    boolean expire(String key, long time);
 
    /**
     * @param key 键 不能为null
     * @return 时间(秒) 返回0代表为永久有效
     */
    long getExpire(String key);
 
    /**
     * 判断key是否存在
     *
     * @param key 键
     * @return true 存在 false不存在
     */
    boolean hasKey(String key);
 
    /**
     * 删除缓存
     *
     * @param key 可以传一个值 或多个
     */
    void del(String... key);
// ============================String=============================
 
    /**
     * 普通缓存获取
     *
     * @param key 键
     * @return 值
     */
    Object get(String key);
 
    /**
     * 普通缓存放入
     *
     * @param key   键
     * @param value 值
     * @return true成功 false失败
     */
    boolean set(String key, Object value);
 
    /**
     * 普通缓存放入并设置时间
     *
     * @param key   键
     * @param value 值
     * @param time  时间(秒) time要大于0 如果time小于等于0 将设置无限期
     * @return true成功 false 失败
     */
    boolean set(String key, Object value, long time);
 
    /**
     * 递增
     *
     * @param key   键
     * @param delta 要增加几(大于0)
     * @return
     */
    long incr(String key, long delta);
 
    /**
     * 递减
     *
     * @param key   键
     * @param delta 要减少几(小于0)
     * @return
     */
    long decr(String key, long delta);
 
    /**
     * HashGet
     *
     * @param key  键 不能为null
     * @param item 项 不能为null
     * @return 值
     */
    Object hget(String key, String item);
 
    /**
     * 获取hashKey对应的所有键值
     *
     * @param key 键
     * @return 对应的多个键值
     */
    Map<Object, Object> hmget(String key);
 
    /**
     * HashSet
     *
     * @param key 键
     * @param map 对应多个键值
     * @return true 成功 false 失败
     */
    boolean hmset(String key, Map<String, Object> map);
 
    /**
     * HashSet 并设置时间
     *
     * @param key  键
     * @param map  对应多个键值
     * @param time 时间(秒)
     * @return true成功 false失败
     */
    boolean hmset(String key, Map<String, Object> map, long time);
 
    /**
     * 向一张hash表中放入数据,如果不存在将创建
     *
     * @param key   键
     * @param item  项
     * @param value 值
     * @return true 成功 false失败
     */
    boolean hset(String key, String item, Object value);
 
    /**
     * 向一张hash表中放入数据,如果不存在将创建
     *
     * @param key   键
     * @param item  项
     * @param value 值
     * @param time  时间(秒) 注意:如果已存在的hash表有时间,这里将会替换原有的时间
     * @return true 成功 false失败
     */
    boolean hset(String key, String item, Object value, long time);
 
    /**
     * 删除hash表中的值
     *
     * @param key  键 不能为null
     * @param item 项 可以使多个 不能为null
     */
    void hdel(String key, Object... item);
 
    /**
     * 判断hash表中是否有该项的值
     *
     * @param key  键 不能为null
     * @param item 项 不能为null
     * @return true 存在 false不存在
     */
    boolean hHasKey(String key, String item);
 
    /**
     * hash递增 如果不存在,就会创建一个 并把新增后的值返回
     *
     * @param key  键
     * @param item 项
     * @param by   要增加几(大于0)
     * @return
     */
    double hincr(String key, String item, double by);
 
    /**
     * hash递减
     *
     * @param key  键
     * @param item 项
     * @param by   要减少记(小于0)
     * @return
     */
    double hdecr(String key, String item, double by);
 
    /**
     * 根据key获取Set中的所有值
     *
     * @param key 键
     * @return
     */
    Set<Object> sGet(String key);
 
    /**
     * 根据value从一个set中查询,是否存在
     *
     * @param key   键
     * @param value 值
     * @return true 存在 false不存在
     */
    boolean sHasKey(String key, Object value);
 
    /**
     * 将数据放入set缓存
     *
     * @param key    键
     * @param values 值 可以是多个
     * @return 成功个数
     */
    long sSet(String key, Object... values);
 
    /**
     * 将set数据放入缓存
     *
     * @param key    键
     * @param time   时间(秒)
     * @param values 值 可以是多个
     * @return 成功个数
     */
    long sSetAndTime(String key, long time, Object... values);
 
 
    /**
     * 获取set缓存的长度
     *
     * @param key 键
     * @return
     */
    long sGetSetSize(String key);
 
    /**
     * 移除值为value的
     *
     * @param key    键
     * @param values 值 可以是多个
     * @return 移除的个数
     */
    long setRemove(String key, Object... values);
 
    /**
     * 获取list缓存的内容
     *
     * @param key   键
     * @param start 开始
     * @param end   结束 0 到 -1代表所有值
     * @return
     */
    List<Object> lGet(String key, long start, long end);
 
    /**
     * 获取list缓存的长度
     *
     * @param key 键
     * @return
     */
    long lGetListSize(String key);
 
    /**
     * 通过索引 获取list中的值
     *
     * @param key   键
     * @param index 索引 index>=0时， 0 表头，1 第二个元素，依次类推；index<0时，-1，表尾，-2倒数第二个元素，依次类推
     * @return
     */
    Object lGetIndex(String key, long index);
 
    /**
     * 将list放入缓存
     *
     * @param key   键
     * @param value 值
     * @return
     */
    boolean lSet(String key, Object value);
 
    /**
     * 将list放入缓存
     *
     * @param key   键
     * @param value 值
     * @return
     */
    boolean lSet(String key, Object value, long time);
 
    /**
     * 将list放入缓存
     *
     * @param key   键
     * @param value 值
     * @param time  时间(秒)
     * @return
     */
    boolean lSetAll(String key, List<Object> value);
 
 
    /**
     * 将list放入缓存
     *
     * @param key   键
     * @param value 值
     * @param time  时间(秒)
     * @return
     */
    boolean lSetAll(String key, List<Object> value, long time);
 
    /**
     * 将list放入缓存
     *
     * @param key   键
     * @param value 值
     * @return
     */
 
    boolean rSet(String key, Object value);
 
    /**
     * 将list放入缓存
     *
     * @param key   键
     * @param value 值
     * @param time  时间(秒)
     * @return
     */
 
    boolean rSet(String key, Object value, long time);
 
    /**
     * 将list放入缓存
     *
     * @param key   键
     * @param value 值
     * @return
     */
    boolean rSetAll(String key, List<Object> value);
 
    /**
     * 将list放入缓存
     *
     * @param key   键
     * @param value 值
     * @param time  时间(秒)
     * @return
     */
    boolean rSetAll(String key, List<Object> value, long time);
 
    /**
     * 根据索引修改list中的某条数据
     *
     * @param key   键
     * @param index 索引
     * @param value 值
     * @return
     */
    boolean lUpdateIndex(String key, long index, Object value);
 
    /**
     * 移除N个值为value
     *
     * @param key   键
     * @param count 移除多少个
     * @param value 值
     * @return 移除的个数
     */
    long lRemove(String key, long count, Object value);
 
    /**
     * 从redis集合中移除[start,end]之间的元素
     *
     * @param key
     * @param stard
     * @param end
     * @return
     */
    void rangeRemove(String key, Long stard, Long end);
 
    /**
     * 返回当前redisTemplate
     *
     * @return
     */
    RedisTemplate getRedisTemplate();
}
```
