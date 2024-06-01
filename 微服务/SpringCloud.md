<a name="dGK9K"></a>
## Sprng Cloud 五大组件
- 服务注册与发现——Netflix Eureka
- 负载均衡
   - 客户端负载均衡——Netflix Ribbon
   - 服务端负载均衡——Feign（其也是依赖于Ribbon,只是将调用方法RestTemplete更改为Serivce接口）
- 断路器——Netflix Hystrix
- 服务网关——Netflix Zuul
- 分布式配置——Sptring Cloud Config

常见面试题

1. 什么是微服务
2. 微服务之间是怎么通讯的
3. Spring Cloud 和 Dubbo 有哪些区别
4. SpringBoot 和 SpringCloud，请谈谈你对他们的理解
5. 什么是服务熔断？什么是服务降级
6. 微服务的优缺点分别是什么？说下你在项目开发中遇到的坑
7. 你所知道的微服务技术栈有哪些？列举一二
8. SpringBoot 和 SpringCloud，请谈谈你对他们的理解
<a name="NYxvQ"></a>
## 微服务概述
<a name="pvONi"></a>
## 什么是微服务?
微服务(Microservice Architecture) 是近几年流行的一种架构思想，关于它的概念很难一言以蔽之。<br />究竟什么是微服务呢？我们在此引用ThoughtWorks 公司的首席科学家 Martin Fowler 于2014年提出的一段话：<br />原文：[Microservices](https://martinfowler.com/articles/microservices.html)<br />汉化：[微服务（Microservices）——Martin Flower - 船长&CAP - 博客园](https://www.cnblogs.com/liuning8023/p/4493156.html)

- 就目前而言，对于微服务，业界并没有一个统一的，标准的定义。
- 但通常而言，微服务架构是一种架构模式，或者说是一种架构风格，**它体长将单一的应用程序划分成一组小的服务**，每个服务运行在其独立的自己的进程内，服务之间互相协调，互相配置，为用户提供最终价值，服务之间采用轻量级的通信机制(**HTTP**)互相沟通，每个服务都围绕着具体的业务进行构建，并且能狗被独立的部署到生产环境中，另外，应尽量避免统一的，集中式的服务管理机制，对具体的一个服务而言，应该根据业务上下文，选择合适的语言，工具(**Maven**)对其进行构建，可以有一个非常轻量级的集中式管理来协调这些服务，可以使用不同的语言来编写服务，也可以使用不同的数据存储。
> 从技术角度理解

- 微服务化的核心就是将传统的一站式应用，根据业务拆分成一个一个的服务，彻底地去耦合，每一个微服务提供单个业务功能的服务，一个服务做一件事情，从技术角度看就是一种小而独立的处理过程，类似进程的概念，能够自行单独启动或销毁，拥有自己独立的数据库。
<a name="zuN8v"></a>
## 微服务于微服务架构
> 微服务

强调的是服务的大小，它关注的是某一个点，是具体解决某一个问题/提供落地对应服务的一个服务应用，狭义的看，可以看作是IDEA中的一个个微服务工程，或者Moudel。IDEA 工具里面使用Maven开发的一个个独立的小Moudel，它具体是使用SpringBoot开发的一个小模块，专业的事情交给专业的模块来做，一个模块就做着一件事情。强调的是一个个的个体，每个个体完成一个具体的任务或者功能。
> 微服务架构

一种新的架构形式，Martin Fowler 于2014年提出。<br />微服务架构是一种架构模式，它体长将单一应用程序划分成一组小的服务，服务之间相互协调，互相配合，为用户提供最终价值。每个服务运行在其独立的进程中，服务与服务之间采用轻量级的通信机制**(如HTTP)**互相协作，每个服务都围绕着具体的业务进行构建，并且能够被独立的部署到生产环境中，另外，应尽量避免统一的，集中式的服务管理机制，对具体的一个服务而言，应根据业务上下文，选择合适的语言、工具**(如Maven)**对其进行构建。
<a name="smlio"></a>
## 微服务优缺点
> 优点

- 单一职责原则；
- 每个服务足够内聚，足够小，代码容易理解，这样能聚焦一个指定的业务功能或业务需求；
- 开发简单，开发效率高，一个服务可能就是专一的只干一件事；
- 微服务能够被小团队单独开发，这个团队只需2-5个开发人员组成；
- 微服务是松耦合的，是有功能意义的服务，无论是在开发阶段或部署阶段都是独立的；
- 微服务能使用不同的语言开发；
- 易于和第三方集成，微服务允许容易且灵活的方式集成自动部署，通过持续集成工具，如jenkins，Hudson，bamboo；
- 微服务易于被一个开发人员理解，修改和维护，这样小团队能够更关注自己的工作成果，无需通过合作才能体现价值；
- 微服务允许利用和融合最新技术；
- **微服务只是业务逻辑的代码，不会和HTML，CSS，或其他的界面混合;**
- **每个微服务都有自己的存储能力，可以有自己的数据库，也可以有统一的数据库；**
> **缺点**

- 开发人员要处理分布式系统的复杂性；
- 多服务运维难度，随着服务的增加，运维的压力也在增大；
- 系统部署依赖问题；
- 服务间通信成本问题；
- 数据一致性问题；
- 系统集成测试问题；
- 性能和监控问题；
<a name="k3cOX"></a>
## 微服务技术栈有那些?
```java
| **微服务技术条目**          | 落地技术                                             |
| -------------------- | ------------------------------------------------ |
| 服务开发                 | SpringBoot、Spring、SpringMVC等                     |
| 服务配置与管理              | Netfix公司的Archaius、阿里的Diamond等                    |
| 服务注册与发现              | Eureka、Consul、Zookeeper等                         |
| 服务调用                 | Rest、PRC、gRPC                                    |
| 服务熔断器                | Hystrix、Envoy等                                   |
| 负载均衡                 | Ribbon、Nginx等                                    |
| 服务接口调用(客户端调用服务的简化工具) | Fegin等                                           |
| 消息队列                 | Kafka、RabbitMQ、ActiveMQ等                         |
| 服务配置中心管理             | SpringCloudConfig、Chef等                          |
| 服务路由(API网关)          | Zuul等                                            |
| 服务监控                 | Zabbix、Nagios、Metrics、Specatator等                |
| 全链路追踪                | Zipkin、Brave、Dapper等                             |
| 数据流操作开发包             | SpringCloud Stream(封装与Redis，Rabbit，Kafka等发送接收消息) |
| 时间消息总栈               | SpringCloud Bus                                  |
| 服务部署                 | Docker、OpenStack、Kubernetes等                     |
```
<a name="KkMty"></a>
## 为什么选择Spring Cloud作为微服务架构

1. 选型依据
   - 整体解决方案和框架成熟度
   - 社区热度
   - 可维护性
   - 学习曲线
2. 当前各大IT公司用的微服务架构有那些？
   - 阿里：dubbo+HFS
   - 京东：JFS
   - 新浪：Motan
   - 当当网：DubboX

各微服务框架对比

| **功能点 / 服务框架** | **Netflix / SpringCloud** | **Motan** | **gRPC** | **Thirt** | **Dubbo / Dubbox** |
| --- | --- | --- | --- | --- | --- |
| 功能定位 | 完整的微服务框架 | RPC框架，但整合了ZK或Consul，实现集群环境的基本服务注册发现  | RPC框架 | RPC框架 | RPC框架 |
| 支持Rest | 是,Ribbon支持多种可插拔的多序列化选择 | 否 | 否 | 否 | 否 |
| 支持RPC | 否 | 是(Hession2) | 是 | 是 | 是 |
| 支持多语言 | 是(Rest形式)？ | 否 | 是 | 是 | 是 |
| 负载均衡 | 是(服务端zuul+客户端Ribbon),zuul-服务，动态路由，云端负载均衡Eureka（针对中间层服务器） | 是（客户端） | 否 | 否 | 是（客户端） |
| 配置服务 | Netfix Archaius，Spring Cloud Config Server 集中配置 | 是(Zookeeper提供) | 否 | 否 | 否 |
| 服务调用链监控 | 是(zuul)，zuul提供边缘服务，API网关 | 否 | 否 | 否 | 否 |
| 高可用/容错 | 是(服务端Hystrix+客户端Ribbon) | 是(客户端) | 否 | 否 | 是（客户端） |
| 典型应用案例 | Netflix  | Sina  | Google  | Facebook  |  |

<a name="N9bp7"></a>
## SpringCloud入门概述
<a name="cDt6m"></a>
### SpringCloud是什么？
Spring官网：[spring](https://spring.io/)<br />![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1673936545777-19bd776d-7f99-491e-b9d3-1f6223c7b78b.png#averageHue=%23f2f1f0&clientId=uf3e0cf9a-8712-4&from=paste&id=u927d68b5&originHeight=805&originWidth=1308&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u77fc4b7c-6511-4a28-95b1-9ed341840f7&title=)<br />![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1673936552157-99edb3cc-8c2f-438d-9b96-8f8948176af5.png#averageHue=%23f5f5f5&clientId=uf3e0cf9a-8712-4&from=paste&id=u2189da55&originHeight=395&originWidth=755&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uf27b3bb6-081f-4129-9909-8c685a5048e&title=)
<a name="USsod"></a>
### SpringCloud和Springboot的关系

- SpringBoot专注于开发方便的开发单个个体微服务；
- SpringCloud是关注全局的微服务协调整理治理框架，它将SpringBoot开发的一个个单体微服务，整合并管理起来，为各个微服务之间提供：配置管理、服务发现、断路器、路由、为代理、事件总栈、全局锁、决策竞选、分布式会话等等集成服务；
- SpringBoot可以离开SpringCloud独立使用，开发项目，但SpringCloud离不开SpringBoot，属于依赖关系；
- 版本的兼容关系，如下：
- ![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1676118587177-6277420e-7227-4534-8a8c-7c27ac90cab4.png#averageHue=%23fefefd&clientId=u3cb89470-9e40-4&from=paste&id=u85ce7dca&originHeight=362&originWidth=885&originalType=url&ratio=1&rotation=0&showTitle=false&size=52400&status=done&style=none&taskId=u85a5816e-9efa-41af-9a41-c694f999b7c&title=)
<a name="Km4bX"></a>
### Dubbon和SpringCloud技术选型
<a name="QnRx6"></a>
#### 1.分布式+服务治理Dubbon
目前成熟的互联网架构，应用服务化拆分 + 消息中间件<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1673937260513-c9bae0bb-f4c2-4500-9bfe-5812e5943874.png#averageHue=%23f5f5f5&clientId=uf3e0cf9a-8712-4&from=paste&height=695&id=uee989cac&originHeight=764&originWidth=1455&originalType=binary&ratio=1&rotation=0&showTitle=false&size=252059&status=done&style=none&taskId=u340d308c-c8cf-4d3e-9f38-fc73ef5f7c4&title=&width=1322.7272440579318)
<a name="CIUgJ"></a>
#### 2.Dubbon和SpringCloud对比
社区活跃度：<br />Dubbon：[链接](https://github.com/dubbo)<br />SpringCloud:[链接](https://github.com/spring-cloud)
```java
|        | Dubbo         | SpringCloud                  |
| ------ | ------------- | ---------------------------- |
| 服务注册中心 | Zookeeper     | Spring Cloud Netfilx Eureka  |
| 服务调用方式 | RPC           | REST API                     |
| 服务监控   | Dubbo-monitor | Spring Boot Admin            |
| 断路器    | 不完善           | Spring Cloud Netfilx Hystrix |
| 服务网关   | 无             | Spring Cloud Netfilx Zuul    |
| 分布式配置  | 无             | Spring Cloud Config          |
| 服务跟踪   | 无             | Spring Cloud Sleuth          |
| 消息总栈   | 无             | Spring Cloud Bus             |
| 数据流    | 无             | Spring Cloud Stream          |
| 批量任务   | 无             | Spring Cloud Task            |
```
**最大区别：Spring Cloud 抛弃了Dubbo的RPC通信，采用的是基于HTTP的REST方式**<br />严格来说，这两种方式各有优劣。虽然从一定程度上来说，后者牺牲了服务调用的性能，但也避免了上面提到的原生RPC带来的问题。而且REST相比RPC更为灵活，服务提供方和调用方的依赖只依靠一纸契约，不存在代码级别的强依赖，这个优点在当下强调快速演化的微服务环境下，显得更加合适。<br /> **品牌机和组装机的区别**<br />**社区支持与更新力度的区别**<br />**总结：**二者解决的问题域不一样：Dubbo的定位是一款RPC框架，而SpringCloud的目标是微服务架构下的一站式解决方案。
<a name="STGkO"></a>
#### 3.SpringCloud能干嘛

- Distributed/versioned configuration 分布式/版本控制配置
- Service registration and discovery 服务注册与发现
- Routing 路由
- Service-to-service calls 服务到服务的调用
- Load balancing 负载均衡配置
- Circuit Breakers 断路器
- Distributed messaging 分布式消息管理
<a name="HUPu2"></a>
#### 4.SpringCloud下载
官网：[链接](http://projects.spring.io/spring-cloud/)<br />版本号有点特别：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1673936870098-599cc887-8b36-43e3-b8e7-d3f5eda0a6b1.png#averageHue=%23fcfbfb&clientId=uf3e0cf9a-8712-4&from=paste&id=u655b03ce&originHeight=421&originWidth=756&originalType=url&ratio=1&rotation=0&showTitle=false&size=40628&status=done&style=none&taskId=uc64a2e17-86cc-44e4-8973-67c3650044a&title=)<br />SpringCloud没有采用数字编号的方式命名版本号，而是采用了伦敦地铁站的名称，**同时根据字母表的顺序来对应版本时间顺序**，比如最早的Realse版本：Angel，第二个Realse版本：Brixton，然后是Camden、Dalston、Edgware，目前最新的是Hoxton SR4 CURRENT GA通用稳定版。<br />**自学参考书**

- SpringCloud Netflix 中文文档：[https://springcloud.cc/spring-cloud-netflix.html](https://springcloud.cc/spring-cloud-netflix.html)
- SpringCloud 中文API文档(官方文档翻译版)：[https://springcloud.cc/spring-cloud-dalston.html](https://springcloud.cc/spring-cloud-dalston.html)
- SpringCloud中国社区：[http://springcloud.cn/](http://springcloud.cn/)
- SpringCloud中文网：[https://springcloud.cc](https://springcloud.cc/)
<a name="H9xSw"></a>
## SpringCloud Rest学习环境搭建：服务提供者
<a name="JKHZa"></a>
### 4.1 介绍

- 我们会使用一个Dept部门模块做一个微服务通用案例**Consumer**消费者(**Client**)通过REST调用**Provider**提供者(**Server**)提供的服务。
- 回顾Spring，SpringMVC，Mybatis等以往学习的知识。
- Maven的分包分模块架构复习。
```properties
一个简单的Maven模块结构是这样的：
-- app-parent: 一个父项目(app-parent)聚合了很多子项目(app-util\app-dao\app-web...)
  |-- pom.xml
  |
  |-- app-core
  ||---- pom.xml
  |
  |-- app-web
  ||---- pom.xml
  ......
```
一个父工程带着多个Moudule子模块<br />MicroServiceCloud父工程(Project)下初次带着3个子模块(Module)

- microservicecloud-api 【封装的整体entity/接口/公共配置等】
- microservicecloud-consumer-dept-80 【服务提供者】
- microservicecloud-provider-dept-8001 【服务消费者】
<a name="Okx1w"></a>
### 4.2 SpringCloud版本选择
**大版本说明**
```properties
| SpringBoot | SpringCloud       | 关系                                 |
| ---------- | ----------------- | ---------------------------------- |
| 1.2.x      | Angel版本(天使)       | 兼容SpringBoot1.2x                   |
| 1.3.x      | Brixton版本(布里克斯顿)  | 兼容SpringBoot1.3x，也兼容SpringBoot1.4x |
| 1.4.x      | Camden版本(卡姆登)     | 兼容SpringBoot1.4x，也兼容SpringBoot1.5x |
| 1.5.x      | Dalston版本(多尔斯顿)   | 兼容SpringBoot1.5x，不兼容SpringBoot2.0x |
| 1.5.x      | Edgware版本(埃奇韦尔)   | 兼容SpringBoot1.5x，不兼容SpringBoot2.0x |
| 2.0.x      | Finchley版本(芬奇利)   | 兼容SpringBoot2.0x，不兼容SpringBoot1.5x |
| 2.1.x      | Greenwich版本(格林威治) |                                    |
```
**实际开发版本关系**
```properties
| spring-boot-starter-parent |          | spring-cloud-dependencles |          |
|:--------------------------:| --------:|:-------------------------:|:--------:|
|          **版本号**        | 发布日期 |          **版本号**        |发布日期  |
|       1.5.2.RELEASE        |  2017-03 |        Dalston.RC1        |  2017-x  |
|       1.5.9.RELEASE        |  2017-11 |      Edgware.RELEASE      | 2017-11  |
|       1.5.16.RELEASE       |  2018-04 |        Edgware.SR5        | 2018-10  |
|       1.5.20.RELEASE       |  2018-09 |        Edgware.SR5        | 2018-10  |
|       2.0.2.RELEASE        |  2018-05 |  Fomchiey.BULD-SNAPSHOT   |  2018-x  |
|       2.0.6.RELEASE        |  2018-10 |       Fomchiey-SR2        | 2018-10  |
|       2.1.4.RELEASE        |  2019-04 |       Greenwich.SR1       | 2019-03  |
```
**使用后两个**
<a name="sufqv"></a>
### 4.3 创建父工程

- 新建父工程项目springcloud，切记**Packageing是pom模式**
- 主要是定义POM文件，将后续各个子模块公用的jar包等统一提取出来，类似一个抽象父类
- ![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1674029079771-59f4637d-4855-48a9-893f-7c945164577d.png#averageHue=%23f4f2f2&clientId=u0f625a0e-276a-4&from=paste&id=u348b490f&originHeight=148&originWidth=435&originalType=url&ratio=1&rotation=0&showTitle=false&size=13054&status=done&style=none&taskId=ua3cd90f5-37c2-4cb1-a71f-a6bc87355bb&title=)
- **pom.xml**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.haust</groupId>
  <artifactId>springcloud</artifactId>
  <version>1.0-SNAPSHOT</version>
  <modules>
    <module>springcloud-api</module>
    <module>springcloud-provider-dept-8001</module>
    <module>springcloud-consumer-dept-80</module>
    <module>springcloud-eureka-7001</module>
    <module>springcloud-eureka-7002</module>
    <module>springcloud-eureka-7003</module>
    <module>springcloud-provider-dept-8002</module>
    <module>springcloud-provider-dept-8003</module>
    <module>springcloud-consumer-dept-feign</module>
    <module>springcloud-provider-dept-hystrix-8001</module>
    <module>springcloud-consumer-hystrix-dashboard</module>
    <module>springcloud-zuul-9527</module>
    <module>springcloud-config-server-3344</module>
    <module>springcloud-config-client-3355</module>
    <module>springcloud-config-eureka-7001</module>
    <module>springcloud-config-dept-8001</module>
  </modules>
  <!--打包方式  pom-->
  <packaging>pom</packaging>
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <junit.version>4.12</junit.version>
    <log4j.version>1.2.17</log4j.version>
    <lombok.version>1.16.18</lombok.version>
  </properties>
  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-alibaba-dependencies</artifactId>
        <version>0.2.0.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <!--springCloud的依赖-->
      <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-dependencies</artifactId>
        <version>Greenwich.SR1</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <!--SpringBoot-->
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-dependencies</artifactId>
        <version>2.1.4.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <!--数据库-->
      <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.47</version>
      </dependency>
      <dependency>
        <groupId>com.alibaba</groupId>
          <artifactId>druid</artifactId>
          <version>1.1.10</version>
          </dependency>
          <!--SpringBoot 启动器-->
          <dependency>
          <groupId>org.mybatis.spring.boot</groupId>
          <artifactId>mybatis-spring-boot-starter</artifactId>
          <version>1.3.2</version>
          </dependency>
          <!--日志测试~-->
          <dependency>
          <groupId>ch.qos.logback</groupId>
          <artifactId>logback-core</artifactId>
          <version>1.2.3</version>
          </dependency>
          <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>${junit.version}</version>
          </dependency>
          <dependency>
          <groupId>log4j</groupId>
          <artifactId>log4j</artifactId>
          <version>${log4j.version}</version>
          </dependency>
          <dependency>
          <groupId>org.projectlombok</groupId>
          <artifactId>lombok</artifactId>
          <version>${lombok.version}</version>
          </dependency>
          </dependencies>
          </dependencyManagement>
          </project>
```
父工程为springcloud，其下有多个子mudule，详情参考完整代码了解<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1674029116816-d7f062b1-06a5-4847-a2c6-228935525558.png#averageHue=%23d7b883&clientId=u0f625a0e-276a-4&from=paste&id=u2f648e9e&originHeight=154&originWidth=425&originalType=url&ratio=1&rotation=0&showTitle=false&size=16623&status=done&style=none&taskId=ua053e686-9e42-45b4-b658-df5058ac2c0&title=)<br />springcloud-consumer-dept-80访问springcloud-provider-dept-8001下的controller使用REST方式<br />如**DeptConsumerController.java**
```java
/**
* @Auther: csp1999
* @Date: 2020/05/17/22:44
* @Description:
*/
@RestController
    public class DeptConsumerController {
        /**
* 理解：消费者，不应该有service层~
* RestTemplate .... 供我们直接调用就可以了！ 注册到Spring中
* (地址：url, 实体：Map ,Class<T> responseType)
* <p>
* 提供多种便捷访问远程http服务的方法，简单的Restful服务模板~
*/
        @Autowired
        private RestTemplate restTemplate;
        /**
* 服务提供方地址前缀
* <p>
* Ribbon:我们这里的地址，应该是一个变量，通过服务名来访问
*/
        private static final String REST_URL_PREFIX = "http://localhost:8001";
        //private static final String REST_URL_PREFIX = "http://SPRINGCLOUD-PROVIDER-DEPT";
        /**
* 消费方添加部门信息
* @param dept
* @return
*/
        @RequestMapping("/consumer/dept/add")
        public boolean add(Dept dept) {
            // postForObject(服务提供方地址(接口),参数实体,返回类型.class)
            return restTemplate.postForObject(REST_URL_PREFIX + "/dept/add", dept, Boolean.class);
        }
        /**
* 消费方根据id查询部门信息
* @param id
* @return
*/
        @RequestMapping("/consumer/dept/get/{id}")
        public Dept get(@PathVariable("id") Long id) {
            // getForObject(服务提供方地址(接口),返回类型.class)
            return restTemplate.getForObject(REST_URL_PREFIX + "/dept/get/" + id, Dept.class);
        }
        /**
* 消费方查询部门信息列表
* @return
*/
        @RequestMapping("/consumer/dept/list")
        public List<Dept> list() {
            return restTemplate.getForObject(REST_URL_PREFIX + "/dept/list", List.class);
        }
    }
```
使用RestTemplete先需要放入Spring容器中<br />**ConfigBean.java**
```java
@Configuration
public class ConfigBean {
    //@Configuration -- spring  applicationContext.xml
    //配置负载均衡实现RestTemplate
    // IRule
    // RoundRobinRule 轮询
    // RandomRule 随机
    // AvailabilityFilteringRule ： 会先过滤掉，跳闸，访问故障的服务~，对剩下的进行轮询~
    // RetryRule ： 会先按照轮询获取服务~，如果服务获取失败，则会在指定的时间内进行，重试
    @Bean
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }
}
```
springcloud-provider-dept-8001的dao接口调用springcloud-api模块下的pojo，可使用在springcloud-provider-dept-8001的pom文件导入springcloud-api模块依赖的方式：
```xml
<!--我们需要拿到实体类，所以要配置api module-->
<dependency>
  <groupId>com.jay</groupId>
  <artifactId>springcloud-api</artifactId>
  <version>1.0-SNAPSHOT</version>
</dependency>
```
springcloud-consumer-dept-80和springcloud-provider-dept-8001的pom.xml和父工程下的依赖基本一样，直接看完整代码即可，不再添加重复笔记。
<a name="ooWnp"></a>
## Eureka服务注册中心
简介Eureka 一词来源于古希腊词汇，是“发现了”的意思。在软件领域，Eureka 是 Netflix 公司开发的一款开源的服务注册与发现组件。

Spring Cloud 将 Eureka 与 Netflix 中的其他开源服务组件（例如 Ribbon、Feign 以及 Hystrix 等）一起整合进 Spring Cloud Netflix 模块中，整合后的组件全称为 Spring Cloud Netflix Eureka。

Eureka 是 Spring Cloud Netflix 模块的子模块，它是 Spring Cloud 对 Netflix Eureka 的二次封装，主要负责 Spring Cloud 的服务注册与发现功能。

Spring Cloud 使用 Spring Boot 思想为 Eureka 增加了自动化配置，开发人员只需要引入相关依赖和注解，就能将 Spring Boot 构建的微服务轻松地与 Eureka 进行整合。
<a name="YOfDU"></a>
### Eureka两大组件
Eureka 采用CS架构(Client/Server , 客户端/服务端), 他包括以下两大组件

- **Erueka Server **: Eureka 服务注册中心，主要提供服务注册功能。当启动微服务时，会将自己的服务祖注册到Eureka Server 维护了一个可用服务列表 存储了所有注册到Eureka Server 的可用服务可以在Erueka Server 的管理界面中直观的看到
- **Eureka Client** : Eureka 客户端 通常指的是微服务系统中的各个微服务 主要用于和Eureka Server 交互，在微服务应用启动后 ， Eureka Client 会向Eureka Server发送心跳( 默认周期为30秒 ) 。 若Eureka Server 多个心跳周期没有接受到某个Eureka Client 的心跳 ，Eureka Server 会将他从可用服务列表中移除(  默认90秒 ) 
<a name="rp06u"></a>
### Eureka 服务注册与发现
Eureka 实现服务注册与发现的原理，如下图所示。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675002889431-66209033-ad7d-4ce1-ae2e-319f37113f2f.png#averageHue=%23343434&clientId=uda64a55d-c74e-4&from=paste&id=uc8e3aeb3&originHeight=179&originWidth=452&originalType=url&ratio=1&rotation=0&showTitle=false&size=17695&status=done&style=none&taskId=u864502c8-cbeb-4cf9-a3bf-fe0a95b0c5a&title=)<br />上图共涉及以下3个角色

- **服务注册中心(Register Service)** : 它是一个 Eureka Server，用于提供服务注册和发现功能。
- **服务提供者（Provider Serivce） : **他是一个Eureka Client 用于提供服务，它将自己提供的服务注册到服务注册中心，以供服务消费者发现
- **服务消费者（Consumer Serivce）: **他是一个Eureka Client 用于消费服务。它可以从服务注册中心获取服务列表，调用所需的服务。

Eureka 实现服务注册与发现的流程如下：

1. 搭建一个Eureka Server 作为服务注册中心
2. 服务提供者 Eureka Client 启动时， 会把当前服务器的信息以服务名(spring.application,name) 的方式注册到服务注册中心
3. 服务消费者Eureka Client 启动时 也会向服务注册中心注册
4. 服务消费者还会获取一份可用服务列表，该列表中包含了所有注册到服务注册中心的服务信息(包括服务提供者和自身的信息)
5. 在获得了可用服务列表后，服务消费者通过 HTTP 或消息中间件远程调用服务提供者提供的服务。

服务注册中心（Eureka Server）所扮演的角色十分重要，它是服务提供者和服务消费者之间的桥梁。服务提供者只有将自己的服务注册到服务注册中心才可能被服务消费者调用，而服务消费者也只有通过服务注册中心获取可用服务列表后，才能调用所需的服务。
<a name="Bz3l5"></a>
### 示例1
下面，我们通过一个案例来展示下 Eureka 是如何实现服务注册与发现的。
<a name="dLNTC"></a>
#### eureka-server

1. springcloud-eureka-7001 模块建立
2. pom.xml 配置
```xml
<!--导包~-->
<dependencies>
	<!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-eureka-server -->
	<!--导入Eureka Server依赖-->
	<dependency>
		<groupId>org.springframework.cloud</groupId>
		<artifactId>spring-cloud-starter-eureka-server</artifactId>
		<version>1.4.6.RELEASE</version>
	</dependency>
	<!--热部署工具-->
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-devtools</artifactId>
	</dependency>
</dependencies>
```

3. application.yml
```yaml
server:
  port: 7001
# Eureka配置
eureka:
  instance:
    # Eureka服务端的实例名字
    hostname: 127.0.0.1
  client:
    # 表示是否向 Eureka 注册中心注册自己(这个模块本身是服务器,所以不需要)
    register-with-eureka: false
    # fetch-registry如果为false,则表示自己为注册中心,客户端的化为 ture
    fetch-registry: false
    # Eureka监控页面~
    service-url:
      defaultZone: http://${
      eureka.instance.hostname}:${
      server.port}/eureka/
```
源码中Eureka的默认端口以及访问路径:<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675007694274-a94b8bd9-a602-4404-903f-b94840be87b3.png#averageHue=%23fefefd&clientId=uda64a55d-c74e-4&from=paste&id=u70340151&originHeight=203&originWidth=953&originalType=url&ratio=1&rotation=0&showTitle=false&size=31474&status=done&style=none&taskId=uee575d90-903d-4447-b5e7-13f3cc3bad1&title=)

4. 主启动类
```java
/**
* @Auther: csp1999
* @Date: 2020/05/18/10:26
* @Description: 启动之后，访问 http://127.0.0.1:7001/
*/
@SpringBootApplication
	// @EnableEurekaServer 服务端的启动类，可以接受别人注册进来~
	@EnableEurekaServer
	public class EurekaServer_7001 {
		public static void main(String[] args) {
			SpringApplication.run(EurekaServer_7001.class,args);
		}
	}
```

5. 启动成功后访问 [http://localhost:7001/](http://localhost:7001/) 得到以下页面

![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675007823599-a864f0df-8ce3-45f3-86e6-9f04e30b4313.png#averageHue=%23cac9c7&clientId=uda64a55d-c74e-4&from=paste&height=1018&id=ub9e6d08f&originHeight=1018&originWidth=1873&originalType=binary&ratio=1&rotation=0&showTitle=false&size=126929&status=done&style=none&taskId=u41e3c564-6ffe-4a44-a24f-dd1513c7d3b&title=&width=1873)

修改Eureka上的默认描述信息
```yaml
# Eureka配置：配置服务注册中心地址
eureka:
  client:
    service-url:
      defaultZone: http://localhost:7001/eureka/
  instance:
    instance-id: springcloud-provider-dept-8001 #修改Eureka上的默认描述信息
```
结果如图：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675009586602-cfb9691d-6e35-4065-a6d4-faa3d9835ad1.png#averageHue=%23d5d3d2&clientId=uda64a55d-c74e-4&from=paste&id=u21dd723a&originHeight=245&originWidth=1231&originalType=url&ratio=1&rotation=0&showTitle=false&size=41306&status=done&style=none&taskId=ua718f713-58dd-4159-974a-2c719ef8498&title=)<br />如果此时停掉springcloud-provider-dept-8001 等**30s**后 监控会开启保护机制：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675009599251-364613cd-ff42-4593-8727-282f2c58d8cf.png#averageHue=%23dbd6d5&clientId=uda64a55d-c74e-4&from=paste&id=uf419943d&originHeight=322&originWidth=1346&originalType=url&ratio=1&rotation=0&showTitle=false&size=68162&status=done&style=none&taskId=u5d470842-036f-4917-8bd6-49d11f58db9&title=)<br />配置关于服务加载的监控信息<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675009608454-3ff64f71-d68e-46a3-8df4-0a932815d063.png#averageHue=%23cecccb&clientId=uda64a55d-c74e-4&from=paste&id=ub93885ff&originHeight=455&originWidth=1288&originalType=url&ratio=1&rotation=0&showTitle=false&size=75290&status=done&style=none&taskId=u70c7679c-b01f-44f9-8009-c09a4d20db0&title=)<br />在客服端 8001 pom.xml中添加依赖
```xml
<!--actuator完善监控信息-->
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```
application.yml中添加配置
```yaml
# info配置
info:
# 项目的名称
app.name: jay-springcloud
# 公司的名称
company.name: 中山职业技术学院
```
此时刷新监控页，点击进入![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675009713043-d7098f1f-69fd-4ca1-89e5-5323ec9ec2af.png#averageHue=%23b0aeab&clientId=uda64a55d-c74e-4&from=paste&id=ue0f3abf9&originHeight=82&originWidth=324&originalType=url&ratio=1&rotation=0&showTitle=false&size=5542&status=done&style=none&taskId=u20e47e39-6ccb-4747-beee-522d8772abe&title=)跳转新页面显示如下内容:<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675009734415-2f59c705-8a13-4284-bfa8-a89f56d8e1e7.png#averageHue=%23d6d9dc&clientId=uda64a55d-c74e-4&from=paste&id=u5b58749b&originHeight=211&originWidth=413&originalType=url&ratio=1&rotation=0&showTitle=false&size=23312&status=done&style=none&taskId=ucf5d2a77-a4d6-4646-bebc-eae30e7de3d&title=)<br />**EureKa自我保护机制：好死不如赖活着**<br />一句话总结就是：**某时刻某一个微服务不可用，eureka不会立即清理，依旧会对该微服务的信息进行保存！**

- 默认情况下，当eureka server在一定时间内没有收到实例的心跳，便会把该实例从注册表中删除（**默认是90秒**），但是，如果短时间内丢失大量的实例心跳，便会触发eureka server的自我保护机制，比如在开发测试时，需要频繁地重启微服务实例，但是我们很少会把eureka server一起重启（因为在开发过程中不会修改eureka注册中心），**当一分钟内收到的心跳数大量减少时，会触发该保护机制**。可以在eureka管理界面看到Renews threshold和Renews(last min)，当后者（最后一分钟收到的心跳数）小于前者（心跳阈值）的时候，触发保护机制，会出现红色的警告：EMERGENCY!EUREKA MAY BE INCORRECTLY CLAIMING INSTANCES ARE UP WHEN THEY'RE NOT.RENEWALS ARE LESSER THAN THRESHOLD AND HENCE THE INSTANCES ARE NOT BEGING EXPIRED JUST TO BE SAFE.从警告中可以看到，eureka认为虽然收不到实例的心跳，但它认为实例还是健康的，eureka会保护这些实例，不会把它们从注册表中删掉。
- 该保护机制的目的是避免网络连接故障，在发生网络故障时，微服务和注册中心之间无法正常通信，但服务本身是健康的，不应该注销该服务，如果eureka因网络故障而把微服务误删了，那即使网络恢复了，该微服务也不会重新注册到eureka server了，因为只有在微服务启动的时候才会发起注册请求，后面只会发送心跳和服务列表请求，这样的话，该实例虽然是运行着，但永远不会被其它服务所感知。所以，eureka server在短时间内丢失过多的客户端心跳时，会进入自我保护模式，该模式下，eureka会保护注册表中的信息，不在注销任何微服务，当网络故障恢复后，eureka会自动退出保护模式。自我保护模式可以让集群更加健壮。
- 但是我们在开发测试阶段，需要频繁地重启发布，如果触发了保护机制，则旧的服务实例没有被删除，这时请求有可能跑到旧的实例中，而该实例已经关闭了，这就导致请求错误，影响开发测试。所以，在开发测试阶段，我们可以把自我保护模式关闭，只需在eureka server配置文件中加上如下配置即可：eureka.server.enable-self-preservation=false【不推荐关闭自我保护机制】

详细内容可以参考下这篇博客内容：[https://blog.csdn.net/wudiyong22/article/details/80827594](https://blog.csdn.net/wudiyong22/article/details/80827594)<br />**注册进来的微服务，获取一些消息（团队开发会用到）**<br />**DeptController.java**新增方法
```java
/**
* DiscoveryClient 可以用来获取一些配置的信息，得到具体的微服务！
*/
@Autowired
	private DiscoveryClient client;
/**
* 获取一些注册进来的微服务的信息~，
*
* @return
*/
@GetMapping("/dept/discovery")
	public Object discovery() {
	// 获取微服务列表的清单
	List<String> services = client.getServices();
	System.out.println("discovery=>services:" + services);
	// 得到一个具体的微服务信息,通过具体的微服务id，applicaioinName；
	List<ServiceInstance> instances = client.getInstances("SPRINGCLOUD-PROVIDER-DEPT");
	for (ServiceInstance instance : instances) {
		System.out.println(
			instance.getHost() + "\t" + // 主机名称
			instance.getPort() + "\t" + // 端口号
			instance.getUri() + "\t" + // uri
			instance.getServiceId() // 服务id
		);
	}
	return this.client;
}
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675009850621-1d98aa9d-bd9d-4a88-bdcc-ce148508fb88.png#averageHue=%23cac7c6&clientId=uda64a55d-c74e-4&from=paste&id=ue5c52576&originHeight=203&originWidth=717&originalType=url&ratio=1&rotation=0&showTitle=false&size=27777&status=done&style=none&taskId=uaa15dd33-3562-4753-bcc2-0298157dab0&title=)<br />主启动类中加入[@EnableDiscoveryClient](https://github.com/EnableDiscoveryClient) 注解
```java
@SpringBootApplication
// @EnableEurekaClient 开启Eureka客户端注解，在服务启动后自动向注册中心注册服务
@EnableEurekaClient
// @EnableEurekaClient 开启服务发现客户端的注解，可以用来获取一些配置的信息，得到具体的微服务
@EnableDiscoveryClient
public class DeptProvider_8001 {
    ...
}
```
结果如图：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675009897717-771bcadc-6492-46b1-8296-9cb9f514e905.png#averageHue=%23f5f5fb&clientId=uda64a55d-c74e-4&from=paste&id=u4f6d5a73&originHeight=351&originWidth=486&originalType=url&ratio=1&rotation=0&showTitle=false&size=33716&status=done&style=none&taskId=ub345e546-fa9f-4f6f-94a1-f1d9f0740af&title=)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675009904105-74b7b0bd-80fd-4e89-8d22-1cf958cbd8c4.png#averageHue=%23f5f4f2&clientId=uda64a55d-c74e-4&from=paste&id=u6d2a11bb&originHeight=217&originWidth=833&originalType=url&ratio=1&rotation=0&showTitle=false&size=52408&status=done&style=none&taskId=u22138d23-6a45-4a41-8605-7410ef5e0d3&title=)
<a name="k8Fgr"></a>
### Eureka Server 集群
在微服务架构中，一个系统往往由十几甚至几十个服务组成，若将这些服务全部注册到同一个 Eureka Server 中，就极有可能导致 Eureka Server 因不堪重负而崩溃，最终导致整个系统瘫痪。解决这个问题最直接的办法就是部署 Eureka Server 集群。<br /> 我们知道，在 Eureka 实现服务注册与发现时一共涉及了 3 个角色：服务注册中心、服务提供者以及服务消费者，这三个角色分工明确，各司其职。但是其实在 Eureka 中，所有服务都既是服务消费者也是服务提供者，服务注册中心 Eureka Server 也不例外。<br />我们在搭建服务注册中心时，在 application.yml 中涉及了这样的配置：
```yaml
eureka:
  client:
    register-with-eureka: false  #false 表示不向注册中心注册自己。
    fetch-registry: false  #false表示自己端就是注册中心，职责就是维护服务实例，并不需要去检索服务
```
这样设置的原因是 micro-service-cloud-eureka-7001 本身自己就是服务注册中心，服务注册中心是不能将自己注册到自己身上的，但服务注册中心是可以将自己作为服务向其他的服务注册中心注册自己的。<br />举个例子，有两个 Eureka Server 分别为 A 和 B，虽然 A 不能将自己注册到 A 上，B 也不能将自己注册到 B 上，但 A 是可以作为一个服务把自己注册到 B 上的，同理 B 也可以将自己注册到 A 上。

这样就可以形成一组互相注册的 Eureka Server 集群，当服务提供者发送注册请求到 Eureka Server 时，Eureka Server 会将请求转发给集群中所有与之相连的 Eureka Server 上，以实现 Eureka Server 之间的服务同步。

通过服务同步，服务消费者可以在集群中的任意一台 Eureka Server 上获取服务提供者提供的服务。这样，即使集群中的某个服务注册中心发生故障，服务消费者仍然可以从集群中的其他 Eureka Server 中获取服务信息并调用，而不会导致系统的整体瘫痪，这就是 Eureka Server 集群的高可用性。
<a name="eGnMW"></a>
#### 示例
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675049135357-e6c6affb-03a5-469c-a041-e704bcb3a647.png#averageHue=%23dbdcda&clientId=u4a01bc1c-9631-4&from=paste&id=u5a093d90&originHeight=240&originWidth=363&originalType=url&ratio=1&rotation=0&showTitle=false&size=54317&status=done&style=none&taskId=ua5ae6699-ca31-46f1-bf8b-7e1fdf40933&title=)<br />**初始化**<br />新建springcloud-eureka-7002、springcloud-eureka-7003 模块

1. 添加POM依赖 （springcloud-eureka-7002、springcloud-eureka-7003）
```xml
<!--导包~-->
<dependencies>
	<!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-eureka-server -->
	<!--导入Eureka Server依赖-->
	<dependency>
		<groupId>org.springframework.cloud</groupId>
		<artifactId>spring-cloud-starter-eureka-server</artifactId>
		<version>1.4.6.RELEASE</version>
	</dependency>
	<!--热部署工具-->
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-devtools</artifactId>
	</dependency>
</dependencies>
```

2. application.yml
```yaml
server:
  port: 7002
eureka:
  instance:
    hostname: localhost #Eureka服务端名称
  client:
    register-with-eureka: false #表示是否向eureka注册中心注册自己
    fetch-registry: false #fetch-registry如果为false 则表示自己为注册中心
    service-url: # eureka监控页面
#      单机配置： defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
#      多人配置 eureka集群
      defaultZone: http://localhost:7001/eureka/,http://localhost:7003/eureka/
```

3. 主启动类(与springcloud-eureka-7001相同)
```java
/**
* @Auther: csp1999
* @Date: 2020/05/18/10:26
* @Description: 启动之后，访问 http://127.0.0.1:7003/
*/
@SpringBootApplication
	// @EnableEurekaServer 服务端的启动类，可以接受别人注册进来~
	public class EurekaServer_7003 {
		public static void main(String[] args) {
			SpringApplication.run(EurekaServer_7003.class,args);
		}
	}
```
**集群成员相互关联**<br />配置一些自定义本机名字，找到本机hosts文件并打开<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675049679908-153af66f-7d37-4c0e-adc3-223a703ce94a.png#averageHue=%23fafaf9&clientId=u4a01bc1c-9631-4&from=paste&id=u9a6e9da1&originHeight=258&originWidth=899&originalType=url&ratio=1&rotation=0&showTitle=false&size=30923&status=done&style=none&taskId=u8dc3223f-4443-4764-b7d7-db96bf9756d&title=)<br />在hosts文件最后加上，要访问的本机名称，默认是localhost<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675049691310-4099dbfd-b368-4a50-90d6-1dd82c194bea.png#averageHue=%23f5f4f2&clientId=u4a01bc1c-9631-4&from=paste&id=u9fe781b2&originHeight=323&originWidth=769&originalType=url&ratio=1&rotation=0&showTitle=false&size=35712&status=done&style=none&taskId=u7b97dfed-0a2f-4619-99ae-e529e1fd43d&title=)<br />修改application.yml的配置，如图为springcloud-eureka-7001配置，springcloud-eureka-7002/springcloud-eureka-7003同样分别修改为其对应的名称即可<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675049701056-65cafe90-ff74-4c48-b70d-e289e848e393.png#averageHue=%23fdfaf7&clientId=u4a01bc1c-9631-4&from=paste&id=uf8c68a0f&originHeight=138&originWidth=621&originalType=url&ratio=1&rotation=0&showTitle=false&size=21194&status=done&style=none&taskId=u03859f09-02d7-40a6-9bdf-bd32c9c231d&title=)<br />在集群中使springcloud-eureka-7001关联springcloud-eureka-7002、springcloud-eureka-7003<br />完整的springcloud-eureka-7001下的application.yml如下
```yaml
server:
  port: 7001
#Eureka配置
eureka:
  instance:
    hostname: eureka7001.com #Eureka服务端的实例名字
  client:
    register-with-eureka: false #表示是否向 Eureka 注册中心注册自己(这个模块本身是服务器,所以不需要)
    fetch-registry: false #fetch-registry如果为false,则表示自己为注册中心
    service-url: #监控页面~
      #重写Eureka的默认端口以及访问路径 --->http://localhost:7001/eureka/
      # 单机： defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
      # 集群（关联）：7001关联7002、7003
      defaultZone: http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
```
同时在集群中使springcloud-eureka-7002关联springcloud-eureka-7001、springcloud-eureka-7003<br />完整的springcloud-eureka-7002下的application.yml如下
```yaml
server:
  port: 7002
#Eureka配置
eureka:
  instance:
    hostname: eureka7002.com #Eureka服务端的实例名字
  client:
    register-with-eureka: false #表示是否向 Eureka 注册中心注册自己(这个模块本身是服务器,所以不需要)
    fetch-registry: false #fetch-registry如果为false,则表示自己为注册中心
    service-url: #监控页面~
      #重写Eureka的默认端口以及访问路径 --->http://localhost:7001/eureka/
      # 单机： defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
      # 集群（关联）：7002关联7001、7003
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7003.com:7003/eureka/
```
springcloud-eureka-7003配置方式同理可得.<br />通过springcloud-provider-dept-8001下的yml配置文件，修改**Eureka配置：配置服务注册中心地址**
```yaml
# Eureka配置：配置服务注册中心地址
eureka:
  client:
    service-url:
      # 注册中心地址7001-7003
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
  instance:
    instance-id: springcloud-provider-dept-8001 #修改Eureka上的默认描述信息
```
这样模拟集群就搭建号了，就可以把一个项目挂载到三个服务器上了<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675050220411-d6055e30-6531-459b-be03-9c390521cfde.png#averageHue=%23cbcac9&clientId=u4a01bc1c-9631-4&from=paste&height=1024&id=uc7a429c1&originHeight=1024&originWidth=1894&originalType=binary&ratio=1&rotation=0&showTitle=false&size=160340&status=done&style=none&taskId=uc4464cc8-9507-4378-bdd7-fe7399892e6&title=&width=1894)
<a name="s6sl8"></a>
#### CAP原则及对比Zookeeper
<a name="DgVtO"></a>
###### **1. 回顾CAP原则**
RDBMS (MySQL\Oracle\sqlServer) ===> ACID<br />NoSQL (Redis\MongoDB) ===> CAP
<a name="bNrBX"></a>
###### **2. ACID是什么？**

- A (Atomicity) 原子性
- C (Consistency) 一致性
- I (Isolation) 隔离性
- D (Durability) 持久性
<a name="GlwO6"></a>
###### **3. CAP是什么?**

- C (Consistency) 强一致性
- A (Availability) 可用性
- P (Partition tolerance) 分区容错性

CAP的三进二：CA、AP、CP
<a name="SA2Sd"></a>
###### **4. CAP理论的核心**

- 一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求
- 根据CAP原理，将NoSQL数据库分成了满足CA原则，满足CP原则和满足AP原则三大类
   - CA：单点集群，满足一致性，可用性的系统，通常可扩展性较差
   - CP：满足一致性，分区容错的系统，通常性能不是特别高
   - AP：满足可用性，分区容错的系统，通常可能对一致性要求低一些
<a name="Up5XE"></a>
###### **5. 作为分布式服务注册中心，Eureka比Zookeeper好在哪里？**
著名的CAP理论指出，一个分布式系统不可能同时满足C (一致性) 、A (可用性) 、P (容错性)，由于分区容错性P再分布式系统中是必须要保证的，因此我们只能再A和C之间进行权衡。

- Zookeeper 保证的是 CP —> 满足一致性，分区容错的系统，通常性能不是特别高
- Eureka 保证的是 AP —> 满足可用性，分区容错的系统，通常可能对一致性要求低一些

**Zookeeper保证的是CP**<br />当向注册中心查询服务列表时，我们可以容忍注册中心返回的是几分钟以前的注册信息，但不能接收服务直接down掉不可用。也就是说，**服务注册功能对可用性的要求要高于一致性**。但zookeeper会出现这样一种情况，当master节点因为网络故障与其他节点失去联系时，剩余节点会重新进行leader选举。问题在于，选举leader的时间太长，30-120s，且选举期间整个zookeeper集群是不可用的，这就导致在选举期间注册服务瘫痪。在云部署的环境下，因为网络问题使得zookeeper集群失去master节点是较大概率发生的事件，虽然服务最终能够恢复，但是，漫长的选举时间导致注册长期不可用，是不可容忍的。<br />**Eureka保证的是AP**<br />Eureka看明白了这一点，因此在设计时就优先保证可用性。**Eureka各个节点都是平等的**，几个节点挂掉不会影响正常节点的工作，剩余的节点依然可以提供注册和查询服务。而Eureka的客户端在向某个Eureka注册时，如果发现连接失败，则会自动切换至其他节点，只要有一台Eureka还在，就能保住注册服务的可用性，只不过查到的信息可能不是最新的，除此之外，Eureka还有之中自我保护机制，如果在15分钟内超过85%的节点都没有正常的心跳，那么Eureka就认为客户端与注册中心出现了网络故障，此时会出现以下几种情况：

- Eureka不在从注册列表中移除因为长时间没收到心跳而应该过期的服务
- Eureka仍然能够接受新服务的注册和查询请求，但是不会被同步到其他节点上 (即保证当前节点依然可用)
- 当网络稳定时，当前实例新的注册信息会被同步到其他节点中

因此，Eureka可以很好的应对因网络故障导致部分节点失去联系的情况，而不会像zookeeper那样使整个注册服务瘫痪
<a name="nVGxi"></a>
## Ribbon：负载均衡(基于客户端)
简介Spring Cloud Ribbon 是一套基于 Netflix Ribbon 实现的**客户端负载均衡和服务调用工具**<br />Netflix Ribbon 是 Netflix 公司发布的开源组件，其主要功能是提供客户端的负载均衡算法和服务调用。Spring Cloud 将其与 Netflix 中的其他开源服务组件（例如 Eureka、Feign 以及 Hystrix 等）一起整合进 Spring Cloud Netflix 模块中，整合后全称为 Spring Cloud Netflix Ribbon。<br />简单的说，Ribbon 是 Netflix 发布的开源项目，主要功能是提供客户端的软件负载均衡算法，将 Netflix 的中间层服务连接在一起。Ribbon 的客户端组件提供一系列完整的配置项，如：**连接超时、重试**等。简单的说，就是在配置文件中列出 LoadBalancer (简称LB：负载均衡) 后面所有的及其，Ribbon 会自动的帮助你基于某种规则 (如**简单轮询，随机连接**等等) 去连接这些机器。我们也容易使用** Ribbon 实现自定义的负载均衡算法**！

Ribbon 是 Spring Cloud Netflix 模块的子模块，它是 Spring Cloud 对 Netflix Ribbon 的二次封装。通过它，我们可以将面向服务的 REST 模板（RestTemplate）请求转换为客户端负载均衡的服务调用。

Ribbon 是 Spring Cloud 体系中最核心、最重要的组件之一。它虽然只是一个工具类型的框架，并不像 Eureka Server（服务注册中心）那样需要独立部署，但它几乎存在于每一个使用 Spring Cloud 构建的微服务中。

Spring Cloud 微服务之间的调用，API 网关的请求转发等内容，实际上都是通过 Spring Cloud Ribbon 来实现的，包括后续我们要介绍的 [OpenFeign](http://c.biancheng.net/springcloud/open-feign.html) 也是基于它实现的。<br />**Ribbon能干嘛**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675091745859-7cdf1e91-ddba-4646-9d0e-5ecc37fbfd66.png#averageHue=%23fcfdfd&clientId=u1711647b-7072-4&from=paste&id=u6d727935&originHeight=482&originWidth=779&originalType=url&ratio=1&rotation=0&showTitle=false&size=267331&status=done&style=none&taskId=u9d820d8e-8547-466f-b280-b6d5a698850&title=)

- LB，即负载均衡 (LoadBalancer) ，在微服务或分布式集群中经常用的一种应用。
- 负载均衡简单的说就是将用户的请求平摊的分配到多个服务上，从而达到系统的HA (高用)。
- 常见的负载均衡软件有 Nginx、Lvs 等等。
- Dubbo、SpringCloud 中均给我们提供了负载均衡，**SpringCloud 的负载均衡算法可以自定义**。
- 负载均衡简单分类：
   - 集中式LB
      - 即在服务的提供方和消费方之间使用独立的LB设施，如**Nginx(反向代理服务器)**，由该设施负责把访问请求通过某种策略转发至服务的提供方！
   - 进程式 LB
      - 将LB逻辑集成到消费方，消费方从服务注册中心获知有哪些地址可用，然后自己再从这些地址中选出一个合适的服务器。
      - **Ribbon 就属于进程内LB**，它只是一个类库，集成于消费方进程，消费方通过它来获取到服务提供方的地址！
<a name="WaLGw"></a>
### 集成Ribbon
**springcloud-consumer-dept-80**向pom.xml中添加Ribbon和Eureka依赖
```xml
<!--Ribbon-->
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-ribbon</artifactId>
  <version>1.4.6.RELEASE</version>
</dependency>
<!--Eureka: Ribbon需要从Eureka服务中心获取要拿什么-->
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-eureka</artifactId>
  <version>1.4.6.RELEASE</version>
</dependency>
```
在application.yml文件中配置Eureka
```yaml
# Eureka配置
eureka:
  client:
    register-with-eureka: false # 不向 Eureka注册自己
    service-url: # 从三个注册中心中随机取一个去访问
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
```
主启动类加上[@EnableEurekaClient](https://github.com/EnableEurekaClient)注解，开启Eureka
```java
//Ribbon 和 Eureka 整合以后，客户端可以直接调用，不用关心IP地址和端口号
@SpringBootApplication
    @EnableEurekaClient //开启Eureka 客户端
    public class DeptConsumer_80 {
        public static void main(String[] args) {
            SpringApplication.run(DeptConsumer_80.class, args);
        }
    }
```
自定义Spring配置类：ConfigBean.java 配置负载均衡实现RestTemplate
```java
@Configuration
    public class ConfigBean {
        //@Configuration -- spring  applicationContext.xml
        @LoadBalanced //配置负载均衡实现RestTemplate
        @Bean
        public RestTemplate getRestTemplate() {
            return new RestTemplate();
        }
    }
```
修改conroller：DeptConsumerController.java
```java
//Ribbon:我们这里的地址，应该是一个变量，通过服务名来访问
//private static final String REST_URL_PREFIX = "http://localhost:8001";
private static final String REST_URL_PREFIX = "http://SPRINGCLOUD-PROVIDER-DEPT";
```
<a name="VTyis"></a>
### 使用Ribbon实现负载均衡
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675091912600-18e46586-a22c-4dbe-b604-9e8cef7a5fab.png#averageHue=%23f7f7f7&clientId=u1711647b-7072-4&from=paste&id=u3a8c1714&originHeight=419&originWidth=951&originalType=url&ratio=1&rotation=0&showTitle=false&size=65454&status=done&style=none&taskId=u58ab63e0-3eca-4b99-8f5e-1ccc7a7316a&title=)<br />1.新建两个服务提供者Moudle：springcloud-provider-dept-8003、springcloud-provider-dept-8002<br />2.参照springcloud-provider-dept-8001 依次为另外两个Moudle添加pom.xml依赖 、resourece下的mybatis和application.yml配置，Java代码<br />3.启动所有服务测试(根据自身电脑配置决定启动服务的个数)，访问[http://eureka7001.com:7002/查看结果](http://eureka7001.com:7002/%E6%9F%A5%E7%9C%8B%E7%BB%93%E6%9E%9C)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675091925896-7d4b4e3f-c310-4ca1-b93a-54a1210a85bf.png#averageHue=%23dedbda&clientId=u1711647b-7072-4&from=paste&id=u7033ee58&originHeight=645&originWidth=1339&originalType=url&ratio=1&rotation=0&showTitle=false&size=109512&status=done&style=none&taskId=u9988cdf2-9983-445e-a15d-559b981d8aa&title=)<br />测试访问[http://localhost/consumer/dept/list](http://localhost/consumer/dept/list) 这时候随机访问的是服务提供者8003<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675095572631-1056db4d-1092-40a3-b8bc-20ab737875f4.png#averageHue=%23f7f8fd&clientId=u1711647b-7072-4&from=paste&id=ue4b8a489&originHeight=562&originWidth=1075&originalType=url&ratio=1&rotation=0&showTitle=false&size=75351&status=done&style=none&taskId=u36ee0974-cca1-436c-b861-5b98b810f2b&title=)<br />再次访问[http://localhost/consumer/dept/list这时候随机的是服务提供者8001](http://localhost/consumer/dept/list%E8%BF%99%E6%97%B6%E5%80%99%E9%9A%8F%E6%9C%BA%E7%9A%84%E6%98%AF%E6%9C%8D%E5%8A%A1%E6%8F%90%E4%BE%9B%E8%80%858001)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675095584324-de4b08a0-718e-41e0-be8a-96a7cd0b0ff7.png#averageHue=%23f7f8fd&clientId=u1711647b-7072-4&from=paste&id=ub4079870&originHeight=550&originWidth=1124&originalType=url&ratio=1&rotation=0&showTitle=false&size=80123&status=done&style=none&taskId=ud01c7ef2-0b23-478c-a0bc-5b61581c875&title=)<br />以上这种**每次访问**[http://localhost/consumer/dept/list随机访问集群中某个服务提供者，这种情况叫做轮询](http://localhost/consumer/dept/list%E9%9A%8F%E6%9C%BA%E8%AE%BF%E9%97%AE%E9%9B%86%E7%BE%A4%E4%B8%AD%E6%9F%90%E4%B8%AA%E6%9C%8D%E5%8A%A1%E6%8F%90%E4%BE%9B%E8%80%85%EF%BC%8C%E8%BF%99%E7%A7%8D%E6%83%85%E5%86%B5%E5%8F%AB%E5%81%9A%E8%BD%AE%E8%AF%A2)，轮询算法在SpringCloud中可以自定义。 
<a name="HF0Wu"></a>
### **如何切换或者自定义规则呢？**
在springcloud-provider-dept-80模块下的ConfigBean中进行配置，切换使用不同的规则
```java
@Configuration
public class ConfigBean {
    //@Configuration -- spring  applicationContext.xml
    /**
     * IRule:
     * RoundRobinRule 轮询策略
     * RandomRule 随机策略
     * AvailabilityFilteringRule ： 会先过滤掉，跳闸，访问故障的服务~，对剩下的进行轮询~
     * RetryRule ： 会先按照轮询获取服务~，如果服务获取失败，则会在指定的时间内进行，重试
     */
    @Bean
    public IRule myRule() {
        return new RandomRule();//使用随机策略
        //return new RoundRobinRule();//使用轮询策略
        //return new AvailabilityFilteringRule();//使用轮询策略
        //return new RetryRule();//使用轮询策略
    }
}
```
也可以自定义规则，在myRule包下自定义一个配置类MyRule.java，注意：**该包不要和主启动类所在的包同级，要跟启动类所在包同级**：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675095648745-700a5361-864d-4f1c-a052-143b6f56c53a.png#averageHue=%23f3f2f0&clientId=u1711647b-7072-4&from=paste&id=u672238c6&originHeight=165&originWidth=375&originalType=url&ratio=1&rotation=0&showTitle=false&size=10693&status=done&style=none&taskId=u72c5ab52-4bbd-4927-bb21-a67fa2985bf&title=)

```java
/**
 * @Auther: csp1999
 * @Date: 2020/05/19/11:58
 * @Description: 自定义规则
 */
@Configuration
public class MyRule {
    @Bean
    public IRule myRule(){
        return new MyRandomRule();//默认是轮询RandomRule,现在自定义为自己的
    }
}
```
 主启动类开启负载均衡并指定自定义的MyRule配置类
```java
//Ribbon 和 Eureka 整合以后，客户端可以直接调用，不用关心IP地址和端口号
@SpringBootApplication
@EnableEurekaClient
//在微服务启动的时候就能加载自定义的Ribbon类(自定义的规则会覆盖原有默认的规则)
//开启负载均衡,并指定自定义的规则
@RibbonClient(name = "SPRINGCLOUD-PROVIDER-DEPT",configuration = MyRule.class)
public class DeptConsumer_80 {
    public static void main(String[] args) {
        SpringApplication.run(DeptConsumer_80.class, args);
    }
}
```
自定义的规则(这里我们参考Ribbon中默认的规则代码自己稍微改动)：MyRandomRule.java
```java
public class MyRandomRule extends AbstractLoadBalancerRule {
    /**
* 每个服务访问5次则换下一个服务(总共3个服务)
* <p>
* total=0,默认=0,如果=5,指向下一个服务节点
* index=0,默认=0,如果total=5,index+1
*/
    private int total = 0;//被调用的次数
    private int currentIndex = 0;//当前是谁在提供服务
    //@edu.umd.cs.findbugs.annotations.SuppressWarnings(value = "RCN_REDUNDANT_NULLCHECK_OF_NULL_VALUE")
    public Server choose(ILoadBalancer lb, Object key) {
        if (lb == null) {
            return null;
        }
        Server server = null;
        while (server == null) {
            if (Thread.interrupted()) {
                return null;
            }
            List<Server> upList = lb.getReachableServers();//获得当前活着的服务
            List<Server> allList = lb.getAllServers();//获取所有的服务
            int serverCount = allList.size();
            if (serverCount == 0) {
                /*
                * No servers. End regardless of pass, because subsequent passes
                * only get more restrictive.
                */
                return null;
            }
            //int index = chooseRandomInt(serverCount);//生成区间随机数
            //server = upList.get(index);//从或活着的服务中,随机获取一个
            //=====================自定义代码=========================
            if (total < 5) {
                server = upList.get(currentIndex);
                total++;
            } else {
                total = 0;
                currentIndex++;
                if (currentIndex > upList.size()) {
                    currentIndex = 0;
                }
                server = upList.get(currentIndex);//从活着的服务中,获取指定的服务来进行操作
            }
            //======================================================
            if (server == null) {
                /*
* The only time this should happen is if the server list were
* somehow trimmed. This is a transient condition. Retry after
* yielding.
*/
                Thread.yield();
                continue;
            }
            if (server.isAlive()) {
                return (server);
            }
            // Shouldn't actually happen.. but must be transient or a bug.
            server = null;
            Thread.yield();
        }
        return server;
    }
    protected int chooseRandomInt(int serverCount) {
        return ThreadLocalRandom.current().nextInt(serverCount);
    }
    @Override
    public Server choose(Object key) {
        return choose(getLoadBalancer(), key);
    }
    @Override
    public void initWithNiwsConfig(IClientConfig clientConfig) {
        // TODO Auto-generated method stub
    }
}
```
<a name="Lnhjn"></a>
## Feign：负载均衡(基于服务端)
<a name="UkHUm"></a>
#### Feign简介
Feign是声明式Web Service客户端，它让微服务之间的调用变得更简单，类似controller调用service。SpringCloud集成了Ribbon和Eureka，可以使用Feigin提供负载均衡的http客户端<br />**只需要创建一个接口，然后添加注解即可~**<br />Feign，主要是社区版，大家都习惯面向接口编程。这个是很多开发人员的规范。调用微服务访问两种方法

1. 微服务名字 【ribbon】
2. 接口和注解 【feign】

**Feign能干什么？**

- Feign旨在使编写Java Http客户端变得更容易
- 前面在使用**Ribbon** + **RestTemplate**时，利用**RestTemplate**对Http请求的封装处理，形成了一套模板化的调用方法。但是在实际开发中，由于对服务依赖的调用可能不止一处，往往一个接口会被多处调用，所以通常都会针对每个微服务自行封装一个客户端类来包装这些依赖服务的调用。所以，**Feign**在此基础上做了进一步的封装，由他来帮助我们定义和实现依赖服务接口的定义，在Feign的实现下，我们只需要创建一个接口并使用注解的方式来配置它 (类似以前Dao接口上标注Mapper注解，现在是一个微服务接口上面标注一个Feign注解)，即可完成对服务提供方的接口绑定，简化了使用Spring Cloud Ribbon 时，自动封装服务调用客户端的开发量。

**Feign默认集成了Ribbon**

- 利用**Ribbon**维护了MicroServiceCloud-Dept的服务列表信息，并且通过轮询实现了客户端的负载均衡，而与**Ribbon**不同的是，通过**Feign**只需要定义服务绑定接口且以声明式的方法，优雅而简单的实现了服务调用。

**Feign的使用步骤**<br />创建springcloud-consumer-fdept-feign模块<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675142809776-d32cdcd7-f474-4adb-815f-f79a076f6b4a.png#averageHue=%233d4145&clientId=u19bd3f88-51d9-4&from=paste&id=uae54fac7&originHeight=547&originWidth=347&originalType=url&ratio=1&rotation=0&showTitle=false&size=33949&status=done&style=none&taskId=u466ed3c0-7c15-4989-85ae-e293f00e845&title=)<br />拷贝springcloud-consumer-dept-80模块下的pom.xml，resource，以及java代码到springcloud-consumer-feign模块，并添加feign依赖。
```xml
<!--Feign的依赖-->
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-feign</artifactId>
  <version>1.4.6.RELEASE</version>
</dependency>
```
通过**Ribbon**实现：—原来的controller：**DeptConsumerController.java**
```java

@RestController
    public class DeptConsumerController {
```````````````````````````````````
通过**Feign**实现：—改造后controller：**DeptConsumerController.java**
``````````````````````````````````

@RestController
public class DeptConsumerController {
    @Autowired
    private DeptClientService deptClientService;
    /**
     * 消费方添加部门信息
     * @param dept
     * @return
     */
    @RequestMapping("/dept/add")
    public boolean add(@RequestBody dept) {
        return deptClientService.addDept(dept);
    }
    /**
     * 消费方根据id查询部门信息
     * @param id
     * @return
     */
    @RequestMapping("/dept/get/{id}")
    public Dept get(@PathVariable("id") Long id) {
       return deptClientService.queryById(id);
    }
    /**
     * 消费方查询部门信息列表
     * @return
     */
    @RequestMapping("/dept/list")
    public List<Dept> list() {
        return deptClientService.queryAll();
    }
}

        
``````````
Feign和Ribbon二者对比，前者显现出面向接口编程特点，代码看起来更清爽，而且Feign调用方式更符合我们之前在做SSM或者SprngBoot项目时，Controller层调用Service层的编程习惯！
**主配置类**：
``````````

@SpringBootApplication
@EnableEurekaClient
// feign客户端注解,并指定要扫描的包以及配置接口DeptClientService
@EnableFeignClients(basePackages = {"com.haust.springcloud"})
// 切记不要加这个注解，不然会出现404访问不到
//@ComponentScan("com.haust.springcloud")
public class FeignDeptConsumer_80 {
    public static void main(String[] args) {
        SpringApplication.run(FeignDeptConsumer_80.class, args);
    }
}
``````````
```
**改造springcloud-api模块**<br />pom.xml添加feign依赖
```xml
<!--Feign的依赖-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-feign</artifactId>
    <version>1.4.6.RELEASE</version>
</dependency>
```
新建service包，并新建DeptClientService.java接口
```java
// @FeignClient:微服务客户端注解,value:指定微服务的名字,这样就可以使Feign客户端直接找到对应的微服务
@FeignClient(value = “SPRINGCLOUD-PROVIDER-DEPT”)
    public interface DeptClientService {
        @GetMapping("/dept/get/{id}")
        public Dept queryById(@PathVariable("id") Long id);
        
        @GetMapping("/dept/list")
        public Dept queryAll();
        
        @GetMapping("/dept/add")
        public Dept addDept(Dept dept);
}
``````````
```
<a name="ousYG"></a>
#### Feign和Ribbon如何选择？
**根据个人习惯而定，如果喜欢REST风格使用Ribbon；如果喜欢社区版的面向接口风格使用Feign.**<br />Feign 本质上也是实现了 Ribbon，只不过后者是在调用方式上，为了满足一些开发者习惯的接口调用习惯！<br />下面我们关闭springcloud-consumer-dept-80 这个服务消费方，换用springcloud-consumer-dept-feign(端口还是80) 来代替：(依然可以正常访问，就是调用方式相比于Ribbon变化了)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675143031875-fdc3041a-b01e-4226-b89b-6dd78de7fc47.png#averageHue=%23f7f7fc&clientId=u19bd3f88-51d9-4&from=paste&id=ua02ba976&originHeight=552&originWidth=518&originalType=url&ratio=1&rotation=0&showTitle=false&size=52845&status=done&style=none&taskId=ua54ca9b1-dc01-44d3-b785-56f8180e271&title=)
<a name="XCoIQ"></a>
## OpenFeign Spring Cloud声明式服务调用组件
Netflix Feign 是 Netflix 公司发布的一种实现负载均衡和服务调用的开源组件。Spring Cloud 将其与 Netflix 中的其他开源服务组件（例如 Eureka、Ribbon 以及 Hystrix 等）一起整合进 Spring Cloud Netflix 模块中，整合后全称为 Spring Cloud Netflix Feign。<br /> <br />Feign 对 [Ribbon](http://c.biancheng.net/springcloud/ribbon.html) 进行了集成，利用 Ribbon 维护了一份可用服务清单，并通过 Ribbon 实现了客户端的负载均衡。

Feign 是一种声明式服务调用组件，它在 RestTemplate 的基础上做了进一步的封装。通过 Feign，我们只需要声明一个接口并通过注解进行简单的配置（类似于 Dao 接口上面的 Mapper 注解一样）即可实现对 HTTP 接口的绑定。

通过 Feign，我们可以像调用本地方法一样来调用远程服务，而完全感觉不到这是在进行远程调用。

Feign 支持多种注解，例如 Feign 自带的注解以及 JAX-RS 注解等，但遗憾的是 Feign 本身并不支持 Spring MVC 注解，这无疑会给广大 Spring 用户带来不便。

2019 年 Netflix 公司宣布 Feign 组件正式进入停更维护状态，于是 Spring 官方便推出了一个名为 OpenFeign 的组件作为 Feign 的替代方案。
<a name="sgpxB"></a>
### OpenFegin
OpenFeign 全称 Spring Cloud OpenFeign，它是 Spring 官方推出的一种声明式服务调用与负载均衡组件，它的出现就是为了替代进入停更维护状态的 Feign。

OpenFeign 是 Spring Cloud 对 Feign 的二次封装，它**具有 Feign 的所有功能**，**并在 Feign 的基础上增加了对 Spring MVC 注解的支持，例如 @RequestMapping、@GetMapping 和 @PostMapping** 等。

<a name="qy4NT"></a>
#### OpenFeign常用注解
| 注解 | 说明 |
| --- | --- |
| @FeignClient | 该注解用于通知OpenFeiogn组件对@RequestMapping 注解下的接口进行解析，并通过动态代理的方式产生实现类，实现负载均衡和服务调用 |
| @EnableFeignClients | 该注解用于开启OpenFeign 功能，当SpringCloud应用启动时，OpenFeign会扫描标有@FeginClient注解的接口，生成代理并注册代Spring容器中 |
| @RequestMapping | Spring MVC 注解，在 Spring MVC 中使用该注解映射请求，通过它来指定控制器（Controller）可以处理哪些 URL 请求，相当于 Servlet 中 web.xml 的配置。 |
| @GetMapping | Spring MVC 注解，用来映射 GET 请求，它是一个组合注解，相当于 @RequestMapping(method = RequestMethod.GET) 。 |
| @PostMapping | Spring MVC 注解，用来映射 POST 请求，它是一个组合注解，相当于 @RequestMapping(method = RequestMethod.POST) 。 |

Spring Cloud Finchley 及以上版本一般使用 OpenFeign 作为其服务调用组件。由于 OpenFeign 是在 2019 年 Feign 停更进入维护后推出的，因此大多数 2019 年及以后的新项目使用的都是 OpenFeign，而 2018 年以前的项目一般使用 Feign。
<a name="YjmMC"></a>
### Feign VS OpenFeign 
<a name="SeeOL"></a>
#### 相同点
Feign 和 OpenFegin 具有以下相同点：

- Feign 和 OpenFeign 都是 Spring Cloud 下的远程调用和负载均衡组件。
- Feign 和 OpenFeign 作用一样，都可以实现服务的远程调用和负载均衡。
- Feign 和 OpenFeign 都对 Ribbon 进行了集成，都利用 Ribbon 维护了可用服务清单，并通过 Ribbon 实现了客户端的负载均衡。
- Feign 和 OpenFeign 都是在服务消费者（客户端）定义服务绑定接口并通过注解的方式进行配置，以实现远程服务的调用。
<a name="EEZq6"></a>
#### 不同点
Feign 和 OpenFeign 具有以下不同：

- Feign 和 OpenFeign 的依赖项不同，Feign 的依赖为 spring-cloud-starter-feign，而 OpenFeign 的依赖为 spring-cloud-starter-openfeign。
- Feign 和 OpenFeign 支持的注解不同，Feign 支持 Feign 注解和 JAX-RS 注解，但不支持 Spring MVC 注解；OpenFeign 除了支持 Feign 注解和 JAX-RS 注解外，还支持 Spring MVC 注解。
<a name="c8oc7"></a>
### OpenFeign实现运程服务调用
项目结构：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1676124899234-0da92e1c-73b3-4804-9360-ad39f4d28520.png#averageHue=%23f9f8f7&clientId=u3cb89470-9e40-4&from=paste&height=303&id=u933cc377&originHeight=303&originWidth=385&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11479&status=done&style=none&taskId=u037babdc-92ca-47e5-8bd3-23024344628&title=&width=385) 

1. 在server_9002导入依赖
```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

2. 在server_9002启动类加上@EnableFeignClients注解
```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
import org.springframework.cloud.openfeign.EnableFeignClients;

@SpringBootApplication
@EnableEurekaClient
@EnableFeignClients
public class server_9002 {
    public static void main(String[] args) {
        SpringApplication.run(server_9002.class,args);
    }
}

```

3. 在server_9002下新建接口ServerCline
```java
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.GetMapping;

@FeignClient("server9001")
public interface ServerCline {

    //以下方法对应server9001想要调用的方法
    @GetMapping("/dome1/test")
    String test();
}

```

4. 在server_9002 Service层下引用ServerCline
```java

import com.jay.springcloud.clients.ServerCline;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class testService {

    // feign接口引用
    @Autowired
    ServerCline serverCline;

    public String test(){
        String test = serverCline.test();
        System.out.println(test);
        return test;
    }

}


```

5. Server_9002 Controllrt层
```java

import com.jay.springcloud.Service.testService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("dome2")
public class testController {

    @Autowired
    testService testService;

    @GetMapping("/test1")
    public String test(){
        return testService.test();
    }
}

```
这样就可以在server_9002调用server_9001的接口了
<a name="Ncff7"></a>
### Feign自定义配置
Feign可以支持很多的自定义配置，如下表所示

| 类型 | 作用 | 说明 |
| --- | --- | --- |
| feign.Logger.Level | 修改日志级别 | 包含四种不同的级别：NONE、BASIC、HEADERS、FULL |
| feign.codec.Decoder | 响应结果的解析器 | http远程调用的结果做解析，例如解析json字符串为java对象 |
| feign.codec.Encoder | 请求参数编码 | 将请求参数编码，便于通过http请求发送 |
| feign.Contract | 支持的注解格式 | 默认是SpringMVC的注解 |
| feign.Retryer | 失败重试机制 | 请求失败的重试机制，默认是没有，不过会使用Ribbon的重试 |

一般情况下，默认值就能满足我们使用，如果要自定义时，只需要创建自定义的@Bean覆盖默认Bean即可。<br />下面以日志为例来演示如何自定义配置。
<a name="DfYqH"></a>
#### 配置文件方式
基于配置文件修改feign的日志级别可以针对单个服务：
```yaml
feign:  
  client:
    config: 
      userservice: # 针对某个微服务的配置
        loggerLevel: FULL #  日志级别 

```
也可以针对所有服务：
```yaml
feign:  
  client:
    config: 
      default: # 这里用default就是全局配置，如果是写服务名称，则是针对某个微服务的配置
        loggerLevel: FULL #  日志级别 

```
而日志的级别分为四种：

- NONE：不记录任何日志信息，这是默认值。
- BASIC：仅记录请求的方法，URL以及响应状态码和执行时间
- HEADERS：在BASIC的基础上，额外记录了请求和响应的头信息
- FULL：记录所有请求和响应的明细，包括头信息、请求体、元数据。
<a name="Gh8gs"></a>
#### Java代码方式
也可以基于Java代码来修改日志级别，先声明一个类，然后声明一个Logger.Level的对象：
```java
public class DefaultFeignConfiguration  {
    @Bean
    public Logger.Level feignLogLevel(){
        return Logger.Level.BASIC; // 日志级别为BASIC
    }
}
```
如果要全局生效，将其放到启动类的@EnableFeignClients这个注解中：
```java
@EnableFeignClients(defaultConfiguration = DefaultFeignConfiguration .class) 
```
如果是局部生效，则把它放到接口对应的@FeignClient这个注解中：
```java
@FeignClient(value = "userservice", configuration = DefaultFeignConfiguration .class) 
```

<a name="ocZVE"></a>
### Feign使用优化
Feign底层发起http请求，依赖于其它的框架。其底层客户端实现包括：<br />•URLConnection：默认实现，不支持连接池<br />•Apache HttpClient ：支持连接池<br />•OKHttp：支持连接池<br />因此提高Feign的性能主要手段就是使用连接池代替默认的URLConnection。<br />这里我们用Apache的HttpClient来演示。

- 引入依赖

在server_9002的pom文件中引入Apache的HttpClient依赖：
```xml
<!--httpClient的依赖 -->
<dependency>
  <groupId>io.github.openfeign</groupId>
  <artifactId>feign-httpclient</artifactId>
</dependency>
```

- 配置连接池

在server_9002的application.yml中添加配置
```yaml
feign:
  client:
    config:
      default: # default全局的配置
        loggerLevel: BASIC # 日志级别，BASIC就是基本的请求和响应信息
  httpclient:
    enabled: true # 开启feign对HttpClient的支持
    max-connections: 200 # 最大的连接数
    max-connections-per-route: 50 # 每个路径的最大连接数

```
总结，Feign的优化：<br />1.日志级别尽量用basic<br />2.使用HttpClient或OKHttp代替URLConnection<br />① 引入feign-httpClient依赖<br />② 配置文件开启httpClient功能，设置连接池参数
<a name="cxMHI"></a>
### Feign最佳实践
所谓最近实践，就是使用过程中总结的经验，最好的一种使用方式。<br />自习观察可以发现，Feign的客户端与服务提供者的controller代码非常相似：<br />feign客户端：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1676127501698-6e575ed6-8a1c-475f-95a6-85523b3a6adb.png#averageHue=%23f1f5e8&clientId=u3cb89470-9e40-4&from=paste&id=ub2a7e47a&originHeight=179&originWidth=685&originalType=url&ratio=1&rotation=0&showTitle=false&size=39663&status=done&style=none&taskId=ucc76d653-80ef-4e1a-a2a2-6259c447966&title=)<br />UserController：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1676127511498-c0375fd1-1cc4-42be-9666-f4fa66571d13.png#averageHue=%23f2f6ef&clientId=u3cb89470-9e40-4&from=paste&id=udce50497&originHeight=418&originWidth=1003&originalType=url&ratio=1&rotation=0&showTitle=false&size=104328&status=done&style=none&taskId=ua1cb6431-8bf4-4420-85d0-0da46270424&title=)<br /> 有没有一种办法简化这种重复的代码编写呢？
<a name="sGXTV"></a>
#### 继承方式
一样的代码可以通过继承来共享：<br />1）定义一个API接口，利用定义方法，并基于SpringMVC注解做声明。<br />2）Feign客户端和Controller都集成改接口<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1676127539679-2795d0ff-9d84-4460-8a0f-40577b82a1c6.png#averageHue=%23e3e9d2&clientId=u3cb89470-9e40-4&from=paste&id=u9ce475bb&originHeight=405&originWidth=1221&originalType=url&ratio=1&rotation=0&showTitle=false&size=50062&status=done&style=none&taskId=uc9fcb2cf-92e6-4545-8764-45f29b17afd&title=)<br />优点：

- 简单
- 实现了代码共享

缺点：

- 服务提供方、服务消费方紧耦合
- 参数列表中的注解映射并不会继承，因此Controller中必须再次声明方法、参数列表、注解
<a name="BjuTY"></a>
#### 抽取方式
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1676127567123-5bdd39ef-70e0-4bde-b9fc-39be20668deb.png#averageHue=%23cecd99&clientId=u3cb89470-9e40-4&from=paste&id=qJmWf&originHeight=465&originWidth=1383&originalType=url&ratio=1&rotation=0&showTitle=false&size=80923&status=done&style=none&taskId=u3963aa04-b728-4773-8bc0-b5cf6320635&title=)<br />将Feign的Client抽取为独立模块，并且把接口有关的POJO、默认的Feign配置都放到这个模块中，提供给所有消费者使用。<br />例如，将UserClient、User、Feign的默认配置都抽取到一个feign-api包中，所有微服务引用该依赖包，即可直接使用。
<a name="JpdfO"></a>
#### 实现基于抽取的最佳实践
首先创建一个module，命名为feign-api：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1676127583406-4929c390-c5da-4861-ba1e-43139dfb249c.png#averageHue=%23f7f6f6&clientId=u3cb89470-9e40-4&from=paste&id=u39ed6d37&originHeight=315&originWidth=967&originalType=url&ratio=1&rotation=0&showTitle=false&size=71085&status=done&style=none&taskId=uc26d1384-4ce1-4026-ae6f-3f3c7fab08a&title=)<br /> 项目结构<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1676127608740-8fa4db30-da89-4e71-b344-c492d8b4b691.png#averageHue=%23f9f9f8&clientId=u3cb89470-9e40-4&from=paste&height=325&id=u75d41c53&originHeight=325&originWidth=496&originalType=binary&ratio=1&rotation=0&showTitle=false&size=12300&status=done&style=none&taskId=u6c9be426-5519-4ca0-b0a2-aa8d0c9bd80&title=&width=496)<br />在feign-api中然后引入feign的starter依赖
```xml
<dependencies>
  <dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
  </dependency>
  <dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-eureka</artifactId>
    <version>1.4.6.RELEASE</version>
  </dependency>
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
</dependencies>
```
在server_9002中引用feign-api
```xml
 <dependency>
    <groupId>org.example</groupId>
    <artifactId>feign-api</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
```
后面一样。。。
<a name="PkfJV"></a>
## Hystrix：服务熔断



<a name="DLzAo"></a>
## Zuul路由网关



<a name="JI1Qv"></a>
## Geteway路由
在微服务架构中，一个系统往往由多个微服务组成，而这些服务可能部署在不同机房、不同地区、不同域名下。这种情况下，客户端（例如浏览器、手机、软件工具等）想要直接请求这些服务，就需要知道它们具体的地址信息，例如 IP 地址、端口号等<br />这种客户端直接请求服务的方式存在以下问题：

- 当服务数量众多时，客户端需要维护大量的服务地址，这对于客户端来说，是非常繁琐复杂的。
- 在某些场景下可能会存在跨域请求的问题。
- 身份认证的难度大，每个微服务需要独立认证。

我们可以通过 API 网关来解决这些问题，下面就让我们来看看什么是 API 网关。
<a name="zldDS"></a>
### API网关 
API 网关是一个搭建在客户端和微服务之间的服务，我们可以在 API 网关中处理一些非业务功能的逻辑，例如权限验证、监控、缓存、请求路由等。<br /> <br />API 网关就像整个微服务系统的门面一样，是系统对外的唯一入口。有了它，客户端会先将请求发送到 API 网关，然后由 API 网关根据请求的标识信息将请求转发到微服务实例。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675649655061-65cf6890-17c8-4219-9712-78a5a24dfbaa.png#averageHue=%23585858&clientId=u7d566e6e-8152-4&from=paste&id=uba5defbc&originHeight=363&originWidth=649&originalType=url&ratio=1&rotation=0&showTitle=false&size=35602&status=done&style=none&taskId=u5d461c06-8157-4973-9ede-f3b6d70a186&title=)

对于服务数量众多，复杂度较高，规模比较大的系统来说使用API网关有以下好处：

- 客户端通过API网关与微服务交互式，客户端只需要知道API网关地址，而不需要维护大量的服务地址，简化了客户端的开发。
- 客户端直接与 API 网关通信，能够减少客户端与各个服务的交互次数
- 客户端与后端的服务耦合度降低。
- 节省流量，提高性能，提升用户体验。
- API 网关还提供了安全、流控、过滤、缓存、计费以及监控等 API 管理功能。

常见的 API 网关实现方案主要有以下 5 种：

- Spring Cloud Gateway
- Spring Cloud Netflix Zuul
- Kong
- Nginx+Lua
- Traefik
<a name="xTdt9"></a>
### Spring Cloud Gateway
Spring Cloud Gateway 是spirng cloud 团队基于Spring 5.0 ，Spring Boot 2.0 和 Project Reactor 等技术开发的高性能API网关组件<br />Spring Cloud Gateway 旨在提供一种简单而有效的途径来发送 API，并为它们提供横切关注点，例如：安全性，监控/指标和弹性。 <br />Spring Cloud Gateway 是基于 WebFlux 框架实现的，而 WebFlux 框架底层则使用了高性能的 Reactor 模式通信框架 Netty。
<a name="xX1M6"></a>
#### Spring Cloud Gateway 核心概念
SpringCloud Gateway 最主要的功能就是路由转发，而在定义转发规则是主要涉及以下三个概念

| 核心概念 | 描述 |
| --- | --- |
| Route（路由） | 网关最基本的模块。它由一个 ID、一个目标 URI、一组断言（Predicate）和一组过滤器（Filter）组成。 |
| Predicate（断言） | 路由转发的判断条件，我们可以通过 Predicate 对 HTTP 请求进行匹配，例如请求方式、请求路径、请求头、参数等，如果请求与断言匹配成功，则将请求转发到相应的服务。 |
| Filter（过滤器） | 过滤器，我们可以使用它对请求进行拦截和修改，还可以使用它对上文的响应进行再处理。 |

注意：其中 Route 和 Predicate 必须同时声明。
<a name="JKaI2"></a>
#### Spring Cloud Gateway 的特征

- 基于 Spring Framework 5、Project Reactor 和 Spring Boot 2.0 构建。
- 能够在任意请求属性上匹配路由。
- predicates（断言） 和 filters（过滤器）是特定于路由的。
- 集成了 Hystrix 熔断器。
- 集成了 Spring Cloud DiscoveryClient（服务发现客户端）。
- 易于编写断言和过滤器。
- 能够限制请求频率。
- 能够重写请求路径
<a name="rTnxy"></a>
###  Gateway 的工作流程
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675650393115-bdb43d99-2e8d-4543-abf8-10abbd2b23e0.png#averageHue=%23b8b8b8&clientId=u7d566e6e-8152-4&from=paste&id=uf7c32add&originHeight=418&originWidth=298&originalType=url&ratio=1&rotation=0&showTitle=false&size=30319&status=done&style=none&taskId=udabd711b-c7c6-4625-a9e1-58cdd454e53&title=)<br />Spring Cloud Gateway 工作流程说明如下：

1. 客户端将请求发送到 Spring Cloud Gateway 上。
2. Spring Cloud Gateway 通过 Gateway Handler Mapping 找到与请求相匹配的路由，将其发送给 Gateway Web Handler。
3. Gateway Web Handler 通过指定的过滤器链（Filter Chain），将请求转发到实际的服务节点中，执行业务逻辑返回响应结果。
4. 过滤器之间用虚线分开是因为过滤器可能会在转发请求之前（pre）或之后（post）执行业务逻辑。
5. 过滤器（Filter）可以在请求被转发到服务端前，对请求进行拦截和修改，例如参数校验、权限校验、流量监控、日志输出以及协议转换等。
6. 过滤器可以在响应返回客户端之前，对响应进行拦截和再处理，例如修改响应内容或响应头、日志输出、流量监控等。
7. 响应原路返回给客户端。

 总而言之，客户端发送到 Spring Cloud Gateway 的请求需要通过一定的匹配条件，才能定位到真正的服务节点。在将请求转发到服务进行处理的过程前后（pre 和 post），我们还可以对请求和响应进行一些精细化控制。

Predicate 就是路由的匹配条件，而 Filter 就是对请求和响应进行精细化控制的工具。有了这两个元素，再加上目标 URI，就可以实现一个具体的路由了。
<a name="XZhFb"></a>
### Predicate 断言
Spring Cloud Gateway 通过 Predicate 断言来实现 Route 路由的匹配规则。简单点说，Predicate 是路由转发的判断条件，请求只有满足了 Predicate 的条件，才会被转发到指定的服务上进行处理。<br />使用 Predicate 断言需要注意以下 3 点：

- Route 路由与 Predicate 断言的对应关系为“一对多”，一个路由可以包含多个不同断言。
- 一个请求想要转发到指定的路由上，就必须同时匹配路由上的所有断言。
- 当一个请求同时满足多个路由的断言条件时，请求只会被首个成功匹配的路由转发。

![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1675654635398-219b7414-43e3-4b52-8f89-397c93d54511.png#averageHue=%23a4a4a4&clientId=u7d566e6e-8152-4&from=paste&id=udad0f937&originHeight=284&originWidth=533&originalType=url&ratio=1&rotation=0&showTitle=false&size=39867&status=done&style=none&taskId=u6b6e59c0-25b5-446b-b22a-53d544b3cc2&title=)

 常见的 Predicate 断言如下表（假设转发的 URI 为 http://localhost:8001）

| 断言 | 示例 | 说明 |
| --- | --- | --- |
| Path | - Path=/dept/list/**  | 当请求路径与 /dept/list/** 匹配时，该请求才能被转发到 http://localhost:8001 上。 |
| Before | - Before=2021-10-20T11:47:34.255+08:00[Asia/Shanghai] | 在 2021 年 10 月 20 日 11 时 47 分 34.255 秒之前的请求，才会被转发到 http://localhost:8001 上。 |
| After | - After=2021-10-20T11:47:34.255+08:00[Asia/Shanghai] | 在 2021 年 10 月 20 日 11 时 47 分 34.255 秒之后的请求，才会被转发到 http://localhost:8001 上。 |
| Between | - Between=2021-10-20T15:18:33.226+08:00[Asia/Shanghai],2021-10-20T15:23:33.226+08:00[Asia/Shanghai] | 在 2021 年 10 月 20 日 15 时 18 分 33.226 秒 到 2021 年 10 月 20 日 15 时 23 分 33.226 秒之间的请求，才会被转发到 http://localhost:8001 服务器上。 |
| Cookie | - Cookie=name,c.biancheng.net | 携带 Cookie 且 Cookie 的内容为 name=c.biancheng.net 的请求，才会被转发到 http://localhost:8001 上。 |
| Header | - Header=X-Request-Id,\\d+ | 请求头上携带属性 X-Request-Id 且属性值为整数的请求，才会被转发到 http://localhost:8001 上。 |
| Method | - Method=GET | 只有 GET 请求才会被转发到 http://localhost:8001 上。 |

<a name="vP31L"></a>
### spring cloud gateway动态路由
默认情况下，Spring Cloud Gateway 会根据服务注册中心（例如 Eureka Server）中维护的服务列表，以服务名（spring.application.name）作为路径创建动态路由进行转发，从而实现动态路由功能。

我们可以在配置文件中，将 Route 的 uri 地址修改为以下形式
```yaml
lb://service-name
```
以上配置说明如下：

- lb：uri 的协议，表示开启 Spring Cloud Gateway 的负载均衡功能。
- service-name：服务名，Spring Cloud Gateway 会根据它获取到具体的微服务地址。
<a name="w7j6N"></a>
### Filter 过滤器
通常情况下，出于安全方面的考虑，服务端提供的服务往往都会有一定的校验逻辑，例如用户登陆状态校验、签名校验等。

在微服务架构中，系统由多个微服务组成，所有这些服务都需要这些校验逻辑，此时我们就可以将这些校验逻辑写到 Spring Cloud Gateway 的 Filter 过滤器中。
<a name="N8rFt"></a>
#### Filter 的分类
Spring Cloud Gateway 提供了以下两种类型的过滤器，可以对请求和响应进行精细化控制。

| 过滤器类型 | 说明 |
| --- | --- |
| Pre 类型 | 这种过滤器在请求被转发到微服务之前可以对请求进行拦截和修改，例如参数校验、权限校验、流量监控、日志输出以及协议转换等操作。 |
| Post 类型 | 这种过滤器在微服务对请求做出响应后可以对响应进行拦截和再处理，例如修改响应内容或响应头、日志输出、流量监控等。 |

 按照作用范围划分，Spring Cloud gateway 的 Filter 可以分为 2 类：

- GatewayFilter：应用在单个路由或者一组路由上的过滤器。
- GlobalFilter：应用在所有的路由上的过滤器。
<a name="Jy1Jx"></a>
#### GatewayFilter 网关过滤器
GatewayFilter 是 Spring Cloud Gateway 网关中提供的一种应用在单个或一组路由上的过滤器。它可以对单个路由或者一组路由上传入的请求和传出响应进行拦截，并实现一些与业务无关的功能，比如登陆状态校验、签名校验、权限校验、日志输出、流量监控等。

GatewayFilter 在配置文件（例如 application.yml）中的写法与 Predicate 类似，格式如下。
```yaml
spring:
  cloud:
    gateway: 
      routes:
        - id: xxxx
          uri: xxxx
          predicates:
            - Path=xxxx
          filters:
          	#过滤器工厂会在匹配的请求头加上一对请求头，名称为 X-Request-Id 值为 1024
            - AddRequestParameter=X-Request-Id,1024 
            - PrefixPath=/dept #在请求路径前面加上 /dept
            ……
```
Spring Cloud Gateway 内置了多达 31 种 GatewayFilter，下表中列举了几种常用的网关过滤器及其使用示例。

| 路由过滤器 | 描述 | 参数 | 使用示例 |
| --- | --- | --- | --- |
| AddRequestHeader |  拦截传入的请求，并在请求上添加一个指定的请求头参数。 | name：需要添加的请求头参数的 key；<br />value：需要添加的请求头参数的 value。 | - AddRequestHeader=my-request-header,1024 |
| AddRequestParameter | 拦截传入的请求，并在请求上添加一个指定的请求参数。 | name：需要添加的请求参数的 key；<br />value：需要添加的请求参数的 value。 | - AddRequestParameter=my-request-param,c.biancheng.net |
| AddResponseHeader | 拦截响应，并在响应上添加一个指定的响应头参数。 | name：需要添加的响应头的 key；<br />value：需要添加的响应头的 value。 | - AddResponseHeader=my-response-header,c.biancheng.net |
| PrefixPath | 拦截传入的请求，并在请求路径增加一个指定的前缀。 |  prefix：需要增加的路径前缀。 | - PrefixPath=/consumer |
| PreserveHostHeader | 转发请求时，保持客户端的 Host 信息不变，然后将它传递到提供具体服务的微服务中。 | 无 | - PreserveHostHeader |
| RemoveRequestHeader | 移除请求头中指定的参数。 | name：需要移除的请求头的 key。 | - RemoveRequestHeader=my-request-header |
| RemoveResponseHeader | 移除响应头中指定的参数。 | name：需要移除的响应头。 | - RemoveResponseHeader=my-response-header |
| RemoveRequestParameter | 移除指定的请求参数。 | name：需要移除的请求参数。 | - RemoveRequestParameter=my-request-param |
| RequestSize | 配置请求体的大小，当请求体过大时，将会返回 413 Payload Too Large。 | maxSize：请求体的大小。 | - name: RequestSize<br />args:<br />maxSize: 5000000 |

<a name="yIDMB"></a>
#### gateway快速入门

- 创建gateway模块

项目结构：![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1676128986955-d2cc1eae-1ab8-44af-860d-d927237f642b.png#averageHue=%23fcfbfa&clientId=u3cb89470-9e40-4&from=paste&height=300&id=ufdf8669c&originHeight=300&originWidth=423&originalType=binary&ratio=1&rotation=0&showTitle=false&size=12308&status=done&style=none&taskId=u8f7cb5f5-15ee-4ac8-b5ed-2b99af59f41&title=&width=423)

- 引入依赖
```xml
  <dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
    <version>2.1.1.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-eureka</artifactId>
    <version>1.4.6.RELEASE</version>
</dependency>

```

- 编写启动类
```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
    public class gateway_9000 {
        public static void main(String[] args) {
            SpringApplication.run(gateway_9000.class, args);
        }
    }
```

- 编写基础配置和路由规则
```yaml
server:
  port: 9999 # 网关端口
spring:
  application:
    name: gateway9999 # 服务名称
  cloud:
    nacos:
      server-addr: localhost:8848 # nacos地址
    gateway:
      routes: # 网关路由配置
        - id: user-service                 # 路由id，自定义，只要唯一即可
          # uri: http://127.0.0.1:8081     # 路由的目标地址 http就是固定地址
          uri: lb://userservice            # 路由的目标地址 lb就是负载均衡，后面跟服务名称
          predicates:                      # 路由断言，也就是判断请求是否符合路由规则的条件
            - Path=/user/**                # 这个是按照路径匹配，只要以/user/开头就符合要求
```
我们将符合Path 规则的一切请求，都代理到 uri参数指定的地址。<br />本例中，我们将 /user/**开头的请求，代理到lb://userservice，lb是负载均衡，根据服务名拉取服务列表，实现负载均衡。<br />总结：<br />网关搭建步骤：<br />创建项目，引入nacos服务发现和gateway依赖<br />配置application.yml，包括服务基本信息、nacos地址、路由<br />路由配置包括：<br />路由id：路由的唯一标示<br />路由目标（uri）：路由的目标地址，http代表固定地址，lb代表根据服务名负载均衡<br />路由断言（predicates）：判断路由的规则，<br />路由过滤器（filters）：对请求或响应做处理
<a name="O4rJm"></a>
## Nginx网关


<a name="fN7Nd"></a>
## Spring Cloud Config 分布式配置
