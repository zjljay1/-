<a name="PPIc6"></a>
## 配置解析
>  核心配置文件

- mybatis-config.xml 系统核心配置文件
- MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。
- 能配置的内容如下：
```
configuration（配置）
properties（属性）
settings（设置）
typeAliases（类型别名）
typeHandlers（类型处理器）
objectFactory（对象工厂）
plugins（插件）
environments（环境配置）
environment（环境变量）
transactionManager（事务管理器）
dataSource（数据源）
databaseIdProvider（数据库厂商标识）
mappers（映射器）
<!-- 注意元素节点的顺序！顺序不对会报错 -->
```
我们可以阅读 mybatis-config.xml 上面的dtd的头文件！
> environments元素

```xml
<environments default="development">
	<environment id="development">
		<transactionManager type="JDBC">
			<property name="..." value="..."/>
		</transactionManager>
		<dataSource type="POOLED">
			<property name="driver" value="${driver}"/>
			<property name="url" value="${url}"/>
			<property name="username" value="${username}"/>
			<property name="password" value="${password}"/>
		</dataSource>
	</environment>
</environments>
```

- 配置MyBatis的多套运行环境，将SQL映射到多个不同的数据库上，必须指定其中一个为默认运行环境（通过default指定）
- 子元素节点：**environment**
   - dataSource 元素使用标准的 JDBC 数据源接口来配置 JDBC 连接对象的资源。
   - 数据源是必须配置的。
   - 有三种内建的数据源类型
> type="[ UNPOOLED	  |	POOLED	|	JNDI ]"）

   - unpooled：这个数据源的实现只是每次被请求时打开和关闭连接。
   - **pooled**：这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来 , 这是一种使得并发 Web 应用快速响应请求的流行处理方式。
   - jndi：这个数据源的实现是为了能在如 Spring 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置一个 JNDI 上下文的引用。
   - 数据源也有很多第三方的实现，比如dbcp，c3p0，druid等等....
   - 详情：点击查看官方文档
   - 这两种事务管理器类型都不需要设置任何属性。
   - 具体的一套环境，通过设置id进行区别，id保证唯一！
   - 子元素节点：transactionManager - [ 事务管理器 ]	
```xml
<!-- 语法 -->
<transactionManager type="[ JDBC | MANAGED ]"/>
```

   - 子元素节点：**数据源（dataSource）**
> mappers元素

**mappers**

- 映射器 : 定义映射SQL语句文件
- 既然 MyBatis 的行为其他元素已经配置完了，我们现在就要定义 SQL 映射语句了。但是首先我们需要告诉 MyBatis 到哪里去找到这些语句。Java 在自动查找这方面没有提供一个很好的方法，所以最佳的方式是告诉 MyBatis 到哪里去找映射文件。你可以使用相对于类路径的资源引用， 或完全限定资源定位符（包括 file:/// 的 URL），或类名和包名等。映射器是MyBatis中最核心的组件之一，在MyBatis 3之前，只支持xml映射器，即：所有的SQL语句都必须在xml文件中配置。而从MyBatis 3开始，还支持接口映射器，这种映射器方式允许以Java代码的方式注解定义SQL语句，非常简洁。
<a name="sZl0Z"></a>
### 配置详解
```java
#配置mysql数据源
spring.datasource.url=jdbc:mysql://your-mysql-url/database-name?useUnicode=true&characterEncoding=UTF-8&allowMultiQueries=true
spring.datasource.username=username
spring.datasource.password=password
spring.datasource.driverClassName=com.mysql.jdbc.Driver

#security.basic.enabled=false

mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl


## Mybatis 配置
mybatis.type-aliases-package=com.xfind.core.entity.xianyu
mybatis.mapper-locations=classpath:mapper/*.xml
#使全局的映射器启用或禁用缓存。
mybatis.configuration.cache-enabled=true
#全局启用或禁用延迟加载。当禁用时，所有关联对象都会即时加载。
mybatis.configuration.lazy-loading-enabled=true
#当启用时，有延迟加载属性的对象在被调用时将会完全加载任意属性。否则，每种属性将会按需要加载。
mybatis.configuration.aggressive-lazy-loading=true
#是否允许单条sql 返回多个数据集  (取决于驱动的兼容性) default:true
mybatis.configuration.multiple-result-sets-enabled=true
#是否可以使用列的别名 (取决于驱动的兼容性) default:true
mybatis.configuration.use-column-label=true
#允许JDBC 生成主键。需要驱动器支持。如果设为了true，这个设置将强制使用被生成的主键，有一些驱动器不兼容不过仍然可以执行。  default:false
mybatis.configuration.use-generated-keys=true
#指定 MyBatis 如何自动映射 数据基表的列 NONE：不隐射\u3000PARTIAL:部分  FULL:全部
mybatis.configuration.auto-mapping-behavior=partial
#这是默认的执行类型  （SIMPLE: 简单； REUSE: 执行器可能重复使用prepared statements语句；BATCH: 执行器可以重复执行语句和批量更新）
mybatis.configuration.default-executor-type=simple
#使用驼峰命名法转换字段。
mybatis.configuration.map-underscore-to-camel-case=true
#设置本地缓存范围 session:就会有数据的共享  statement:语句范围 (这样就不会有数据的共享 ) defalut:session
mybatis.configuration.local-cache-scope=session
#设置但JDBC类型为空时,某些驱动程序 要指定值,default:OTHER，插入空值时不需要指定类型
mybatis.configuration.jdbc-type-for-null=null
#如果数据为空的字段，则该字段省略不显示，可以通过添加配置文件，规定查询数据为空是则返回null。
mybatis.configuration.call-setters-on-nulls=true
```

- **mybatis.check-config-location : java.lang.Boolean , 默认false**是否执行MyBatis xml配置文件的状态检查, 只是检查状态
- **mybatis.config-location : java.lang.String**mybatis-config.xml文件的位置
- **mybatis.configuration-properties : java.util.Properties**mybatis 配置的扩展属性,配置在这里
- **mybatis.type-aliases-package : java.lang.String**使用别名的路径 , 针对的是pojo , 也可以是工具类
- **mybatis.type-aliases-super-type : java.lang.Class<?>**指定alias类的父类 .当没有指定父类时 , 那么 type-aliases-package下的所有类都会指定别名 .当指定父类后 , type-aliases-package 下的指定父类的子类 ,才会加载别名 .
- **mybatis.type-handlers-package : java.lang.String**类型转换器的路径包名. 加载类型转换器. javaType 与 JdbcType互转 .
- **mybatis.lazy-initialization : java.lang.Boolean**懒初始化 , mybatis 会为每个mapper 生成一个bean , 那么对于这些bean是否需要延迟初始化. 延迟初始化会有多线程问题 , 慎用 , 默认的false就OK.true : 启用 , false : 禁用 , 默认false
- **mybatis.executor-type : org.apache.ibatis.session.ExecutorType**指定以何种方式执行 SqlSessionTemplate . 有三种模式
   - simple(默认) : 为每个语句的执行创建一个新的预处理语句,单条提交sql .每执行一次update或select，就开启一个Statement对象，用完立刻关闭Statement对象。（可以是Statement或PrepareStatement对象）
   - reuse : 执行update或select，以sql作为key查找Statement对象，存在就使用，不存在就创建，用完后，不关闭Statement对象，而是放置于Map<String, Statement>内，供下一次使用。（可以是Statement或PrepareStatement对象）
   - batch : 重复使用已经预处理的语句. 执行update（没有select，JDBC批处理不支持select），将所有sql都添加到批处理中（addBatch()），等待统一执行（executeBatch()），它缓存了多个Statement对象，每个Statement对象都是addBatch()完毕后，等待逐一执行executeBatch()批处理的
- **mybatis.configuration.lazy-loading-enabled: java.lang.Boolean**全局启用或禁用延迟加载。当禁用时，所有关联对象都会即时加载。Mybatis仅支持association关联对象和collection关联集合对象的延迟加载，association指的就是一对一，collection指的就是一对多查询。true: 启用 , false : 禁用 , 默认禁用
- **mybatis.configuration.aggressive-lazy-loading : java.lang.Boolean**设置延迟加载的所有属性 是 全部加载 , 还是按需加载.<br />true: 全部加载 , false , 按需加载, 默认false
- **mybatis.configuration.lazy-load-trigger-methods : java.util.Set<java.lang.String>**懒加载属性的触发条件, 当执行指定方法时,触发延迟加载 .<br />默认是: "equals", "clone", "hashCode", "toString"
- **configuration.log-impl: Class<? extends Log>**输出日志的实现类 , 支持 6 种日志模式 . 默认依次使用以下顺序.
- **mybatis.configuration.log-prefix: java.lang.String**指定日志输出的前缀
- **mybatis.configuration.interceptors:java.util.List<org.apache.ibatis.plugin.Interceptor>**拦截器 , 可以对执行sql做自定义处理 . 也可以阻止执行sql .
- **mybatis.configuration.jdbc-type-for-null : org.apache.ibatis.type.JdbcType**当写入 null 值的字段时 , 部分数据库需要指定null的数据类型 . mysql不用设置 . oracle需要设置 .
- **mybatis.configuration.cache-enabled : java.lang.Boolean**是否启用缓存 , 默认 true (启用缓存) . 这里是一级缓存 .
- **mybatis.configuration.caches : java.util.Collection<org.apache.ibatis.cache.Cache>**缓存方案 , 已提供如下缓存方案 . 缓存的装饰器 . 也可以自定义缓存,实现Cache接口 .
- **mybatis.configuration.cache-names : java.util.Collection<java.lang.String>**所有缓存的namespace .<br />※: 这里配置没有效果 . 应该是已经废弃了
- **mybatis.configuration.local-cache-scope : org.apache.ibatis.session.LocalCacheScope**本地缓存的有效范围, 支持 SESSION,STATEMENT .
   - SESSION : 一个sqlsession中有效.
   - STATEMENT: 针对单独的sql有效. 可以在不同session中
- **mybatis.configuration.auto-mapping-behavior : org.apache.ibatis.session.AutoMappingBehavior**自动匹配属性字段的动作, 支持三种方式:
   - NONE : 不自动匹配
   - PARTIAL (默认) : 会自动匹配字段 , 但内嵌字段 / 多层级复杂字段属性不匹配
   - FULL : 会自动匹配字段 , 内嵌字段 / 多层级复杂字段属性也会匹配 . 但性能不佳 , 从实用角度来说 . 不会有这么复杂的sql查询结果.
- **mybatis.configuration.auto-mapping-unknown-column-behavior : org.apache.ibatis.session.AutoMappingUnknownColumnBehavior**没有匹配的属性字段时,要怎么处理的动作 , 有以下三种方式:
   - NONE : 不处理 , 跳过.
   - WARNING : 日志打出 警告信息 .
   - FAILING : 抛出异常信息 , SqlSessionException
- **mybatis.configuration.call-setters-on-nulls : java.lang.Boolean**null , 空值时, 是否调用setter方法 , 默认false 不调用 .
- **mybatis.configuration.environment : org.apache.ibatis.mapping.Environment**环境标识, 可以做环境隔离,和环境区分. 不同环境设置不同的事务工厂和不同的数据源
```
Copymybatis.configuration-properties.ext1=123 mybatis.configuration-properties.ext2=abc
```

```
Copy  tryImplementation(LogFactory::useSlf4jLogging);   tryImplementation(LogFactory::useCommonsLogging);   tryImplementation(LogFactory::useLog4J2Logging);   tryImplementation(LogFactory::useLog4JLogging);   tryImplementation(LogFactory::useJdkLogging);   tryImplementation(LogFactory::useNoLogging);
```

```
Copy  BlockingCache   FifoCache   LoggingCache   LruCache   ScheduledCache   SerializedCache   SoftCache   SynchronizedCache   TransactionalCache   WeakCache
```
※: 这里配置没有效果 , 需要在mapper.xml文件里配置 .
<a name="zhzAl"></a>
## 缓存
简介1、什么是缓存 [ Cache ]？

- 存在内存中的临时数据。
- 将用户经常查询的数据放在缓存（内存）中，用户去查询数据就不用从磁盘上(关系型数据库数据文件)查询，从缓存中查询，从而提高查询效率，解决了高并发系统的性能问题。

2、为什么使用缓存？

- 减少和数据库的交互次数，减少系统开销，提高系统效率。

3、什么样的数据能使用缓存？

- 经常查询并且不经常改变的数据。
> Mybatis缓存

- MyBatis包含一个非常强大的查询缓存特性，它可以非常方便地定制和配置缓存。缓存可以极大的提升查询效率。
- MyBatis系统中默认定义了两级缓存：**一级缓存**和**二级缓存**
   - 默认情况下，只有一级缓存开启。（SqlSession级别的缓存，也称为本地缓存）
   - 二级缓存需要手动开启和配置，他是基于namespace级别的缓存。
   - 为了提高扩展性，MyBatis定义了缓存接口Cache。我们可以通过实现Cache接口来自定义二级缓存



