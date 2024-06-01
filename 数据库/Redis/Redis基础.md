<a name="VTm9b"></a>
# 简介
Redis 是完全开源免费的，遵守 BSD 协议，是一个灵活的高性能 key-value 数据结构存储，可以用来作为数据库、缓存和消息队列。<br />Redis 比其他 key-value 缓存产品有以下三个特点：

- Redis 支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载到内存使用。
- Redis 不仅支持简单的 key-value 类型的数据，同时还提供 list，set，zset，hash 等数据结构的存储。
- Redis 支持主从复制，即 master-slave 模式的数据备份。
<a name="UF6th"></a>
## Redis 的特点

- **高性能**： Redis 将所有数据集存储在内存中，可以在入门级 Linux 机器中每秒写（SET）11 万次，读（GET）8.1 万次。Redis 支持 Pipelining 命令，可一次发送多条命令来提高吞吐率，减少通信延迟。
- **持久化**：当所有数据都存在于内存中时，可以根据自上次保存以来经过的时间和/或更新次数，使用灵活的策略将更改异步保存在磁盘上。Redis 支持仅附加文件（AOF）持久化模式。
- **数据结构**： Redis 支持各种类型的数据结构，例如字符串、散列、集合、列表、带有范围查询的有序集、位图、超级日志和带有半径查询的地理空间索引。
- **原子操作**：处理不同数据类型的 Redis 操作是原子操作，因此可以安全地 [SET](https://redis.com.cn/commands/set.html) 或 [INCR](https://redis.com.cn/commands/incr.html) 键，添加和删除集合中的元素等。
- **支持的语言**： Redis 支持许多语言，如 C、C++、Erlang、Go、Haskell、Java、JavaScript（Node.js)、Lua、Objective-C、Perl、PHP、Python、R、Ruby、Rust、Scala、Smalltalk 等。
- **主/从复制**： Redis 遵循非常简单快速的主/从复制。配置文件中只需要一行来设置它，而 Slave 在 Amazon EC2 实例上完成 10 MM key 集的初始同步只需要 21 秒。
- **分片**： Redis 支持分片。与其他键值存储一样，跨多个 Redis 实例分发数据集非常容易。
- **可移植**： Redis 是用 C 编写的，适用于大多数 POSIX 系统，如 Linux、BSD、Mac OS X、Solaris 等。
<a name="TX0gJ"></a>
## Redis 与其他 key-value 存储有什么不同？

- Redis 有着更为复杂的数据结构并且提供对它们的原子性操作，这是一个不同于其他数据库的进化路径。Redis 的数据类型都是基于基本数据结构的同时对程序员透明，无需进行额外的抽象。
- Redis 运行在内存中但是可以持久化到磁盘，所以在对不同数据集进行高速读写时需要权衡内存，因为数据量不能大于硬件内存。在内存数据库方面的另一个优点是，相比在磁盘上相同的复杂的数据结构，在内存中操作起来非常简单，这样 Redis 可以做很多内部复杂性很强的事情。同时，因 RDB 和 AOF 两种磁盘持久化方式是不适合随机访问，因为它们是顺序写入的。
<a name="zd1gz"></a>
## Redis 架构
Redis 主要由有两个程序组成：

- Redis 客户端 redis-cli
- Redis 服务器 redis-server

客户端、服务器可以位于同一台计算机或两台不同的计算机中
<a name="qPv7L"></a>
# 在 windows 上安装 Redis
Redis 官方不建议在 windows 下使用 Redis，所以官网没有 windows 版本可以下载。还好微软团队维护了开源的 windows 版本，虽然只有 3.2 版本，对于普通测试使用足够了。
<a name="yuWsB"></a>
## 安装包方式安装 Redis 服务
下载地址：<br />[Releases · microsoftarchive/redis](https://github.com/MicrosoftArchive/redis/releases)<br />点击下载：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681692770298-1922c750-06a6-4603-a892-d1ab9311778d.png#averageHue=%23fbf9f8&clientId=u7a939507-0444-4&from=paste&id=u8c85ad00&originHeight=406&originWidth=624&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=82751&status=done&style=none&taskId=u3ab89c81-fb3a-4058-9c93-f679c8f7487&title=)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681692791019-e0b462fd-dda7-4a89-884c-25971d517c88.png#averageHue=%23f9f8f7&clientId=u7a939507-0444-4&from=paste&id=u228de290&originHeight=406&originWidth=624&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=91579&status=done&style=none&taskId=u4d93b71a-45bf-4436-80e0-38c76ae9122&title=)<br />下载完成之后双击按着引导流程安装。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681693156560-ea54edf6-14df-4357-9eaa-28aacf8656f9.png#averageHue=%23b6b3b2&clientId=u7a939507-0444-4&from=paste&id=ua851e8c0&originHeight=387&originWidth=499&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=17637&status=done&style=none&taskId=u33bec1f8-18c2-4f9f-9faa-d031adc897b&title=)<br /> ![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681693166613-b24553d0-df0a-41b8-8442-2b8b680a1c31.png#averageHue=%23edeae8&clientId=u7a939507-0444-4&from=paste&id=ub55fa400&originHeight=387&originWidth=499&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=26684&status=done&style=none&taskId=u088d68d0-c106-494f-841d-7f8d2b3fe43&title=)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681693170476-61662649-5e66-4eda-83d0-bd6d1e8d8344.png#averageHue=%23edebe9&clientId=u7a939507-0444-4&from=paste&id=uafa1a1c3&originHeight=387&originWidth=499&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=18146&status=done&style=none&taskId=ud4dc23ce-a56c-4360-9d29-960805433b3&title=)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681693176434-6f8aafb7-9219-433e-96c8-7e9fda73caca.png#averageHue=%23edebea&clientId=u7a939507-0444-4&from=paste&id=u09c78743&originHeight=387&originWidth=499&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=17584&status=done&style=none&taskId=u03581cd9-3fcb-41b2-850f-6f0d42a58ce&title=)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681693182732-075c9fb4-e290-4e39-a07e-17ad7fed16ea.png#averageHue=%23ece9e8&clientId=u7a939507-0444-4&from=paste&id=u3adf501b&originHeight=387&originWidth=499&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=16973&status=done&style=none&taskId=u862b5d4a-2928-47c1-a567-2c41d7e781c&title=)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681693187907-03b4b917-261c-43e3-9b2b-7d49f9c40e35.png#averageHue=%23b6b3b2&clientId=u7a939507-0444-4&from=paste&id=uea7631fe&originHeight=387&originWidth=499&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=17358&status=done&style=none&taskId=ud5313650-46c8-43d6-bc33-3cde62a6606&title=)
<a name="kbBwr"></a>
## 启动 Redis
Redis 现在可以使用了。打开 Redis 程序目录：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681693239838-9506057e-7336-4327-b643-b546552d08e8.png#averageHue=%23fbf9f8&clientId=u7a939507-0444-4&from=paste&height=373&id=u59be3703&originHeight=513&originWidth=906&originalType=binary&ratio=1.375&rotation=0&showTitle=false&size=62088&status=done&style=none&taskId=uff551a6f-eef4-4746-b11b-985eca8343e&title=&width=658.9090909090909)<br />文件介绍：<br />redis-server.exe：服务端程序，提供 redis 服务<br />redis-cli.exe: 客户端程序，通过它连接 redis 服务并进行操作<br />redis-check-dump.exe：RDB 文件修复工具<br />redis-check-aof.exe：AOF 文件修复工具<br />redis-benchmark.exe：性能测试工具，用以模拟同时由 N 个客户端发送 M 个 SETs/GETs 查询 (类似于 Apache 的 ab 工具)<br />redis.windows.conf： 配置文件，将 redis 作为普通软件使用的配置，命令行关闭则 redis 关闭<br />redis.windows-service.conf：配置文件，将 redis 作为系统服务的配置<br />单击 redis-server.exe，启动 Redis 服务。<br /> ![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681780492097-1036499e-a8cf-49bc-a455-5c38a47a5ba2.png#averageHue=%23f7f7f4&clientId=ua7248091-1823-4&from=paste&id=u800daae2&originHeight=272&originWidth=624&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=100614&status=done&style=none&taskId=u23606f24-8862-440f-b287-94e7aeb0d2d&title=)<br />启动redis 客户端<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681780514908-dacda0e0-ae8c-4a43-b7b8-0958f9890f27.png#averageHue=%23faf9f8&clientId=ua7248091-1823-4&from=paste&height=412&id=uaa31391b&originHeight=567&originWidth=940&originalType=binary&ratio=1.375&rotation=0&showTitle=false&size=68464&status=done&style=none&taskId=uf5a4f3c9-2112-426e-9308-c02f2177cb1&title=&width=683.6363636363636)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681694439581-e78a0c71-9422-4f25-907d-600c84624ef5.png#averageHue=%232b2b2b&clientId=u7a939507-0444-4&from=paste&height=276&id=u5952c0f1&originHeight=380&originWidth=805&originalType=binary&ratio=1.375&rotation=0&showTitle=false&size=7447&status=done&style=none&taskId=uc3ab41ff-d8e8-4434-878c-bf728cf4ca9&title=&width=585.4545454545455)<br />检查 Redis 是否已连接。<br />使用 PING 命令。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681694479881-4f852984-240e-4115-87c5-137ae6b3b086.png#averageHue=%23474644&clientId=u7a939507-0444-4&from=paste&height=111&id=ud8b879b3&originHeight=152&originWidth=397&originalType=binary&ratio=1.375&rotation=0&showTitle=false&size=7381&status=done&style=none&taskId=u68cd21ab-49dd-47b0-935c-07f35628613&title=&width=288.72727272727275)<br />Redis 服务窗口也输出 1 个客户端已连接。<br /> ![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681694548311-1e103276-83fd-4db6-8a4a-85663acec40b.png#averageHue=%232a2928&clientId=u7a939507-0444-4&from=paste&id=u8267f85e&originHeight=164&originWidth=624&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=43430&status=done&style=none&taskId=ubf7d30c3-0b75-417b-8e8d-cd4e704ea71&title=)
<a name="sESBW"></a>
### BUG 注：windows启动redis时可能会遇到的bug
[解决启动redis出现的creating server tcp listening socket *:6379: listen: unknown error - 腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/1619341)
:::success
报错信息: creating server tcp listening socket *:6379: listen: unknown error
:::
:::success
可能原因： <br />与安装运行成功的redis服务进行比较，比较了redis-server.exe和配置文件redis.windows.conf<br />通过与安装成功的redis配置文件进行对比，发现，配置文件redis.windows.conf存在差异； <br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681780691465-5cbc1b28-8a03-4931-ad02-2b38e055850d.png#averageHue=%23d0e9d2&clientId=ua7248091-1823-4&from=paste&id=u5e81495a&originHeight=177&originWidth=1123&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=30651&status=done&style=none&taskId=u83f887c7-e4e0-4c5b-ad3a-ea8e0ad45ae&title=)<br />bind 如果是 127.0.0.1的 话，只能本机 访问，如果是 0.0.0.0的话，代表任何机器都可以访问。<br />解决问题方案：在配置文件redis.windows.conf找到代码# bind 127.0.0.1;去掉前面的”#“;在本地就可以成功运行。如果需要远程连接服务还需要将127..0.0.1改为 0.0.0.0。
:::
<a name="MBv5I"></a>
#### 解决：
[Creating Server TCP listening socket *:6379: bind: No error_呼伦贝尔-钢蛋儿的博客-CSDN博客](https://blog.csdn.net/qq_38220334/article/details/105527236)
<a name="kZ4rf"></a>
### 安装 redis 到 windows 服务
```java
redis-server --service-install redis.windows.conf
```
查看 windows 服务是否加入：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681694632392-56905d9b-249b-4f4b-9255-c2390b3c9ef1.png#averageHue=%23f8f7f6&clientId=u7a939507-0444-4&from=paste&id=u84923ad3&originHeight=741&originWidth=1007&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=274317&status=done&style=none&taskId=u9c131e62-84a3-40fb-a5ac-afe29f6eb65&title=)<br />这时候先关闭打开的第一个 cmd 窗口，然后执行以下命令启动再次 redis：
```java
redis-server --service-start
```
停止 redis 服务：
```java
redis-server --service-stop
```
最后，测试一下 redis 是否能够正常使用：<br />切换到 redis 目录下：E:\tools\redis-3.2.100  下
```java
redis-cli.exe -h 127.0.0.1 -p 6379 
```
<a name="H5BGQ"></a>
# Linux 安装 Redis
下载地址：<br />[Download](http://redis.io/download)<br />，下载最新稳定版本。<br />本教程使用的最新文档版本为 6.0.8，下载并安装：
```java
# wget http://download.redis.io/redis-stable.tar.gz
# tar xzf redis-6.0.8.tar.gz
# cd redis-6.0.8
# make
```
执行完 make 命令后，redis-6.0.8 目录下会出现编译后的 redis 服务程序 redis-server，还有用于测试的客户端程序 redis-cli，两个程序位于安装目录 src 目录下：<br />下面启动 redis 服务：
```java
# cd src
# ./redis-server
```
注意这种方式启动 redis 使用的是默认配置。也可以通过启动参数告诉 redis 使用指定配置文件使用下面命令启动。
```java
# cd src
# ./redis-server ../redis.conf
```
redis.conf 是一个默认的配置文件。我们可以根据需要使用自己的配置文件。<br />启动 redis 服务进程后，就可以使用测试客户端程序 redis-cli 和 redis 服务交互了。 比如：
```java
# cd src
# ./redis-cli
redis> set foo bar
OK
redis> get foo
"bar"
```
**配置 Redis 为后台服务** 将配置文件中的 daemonize no 改成 daemonize yes，配置 redis 为后台启动。<br />**Redis 设置访问密码** 在配置文件中找到 requirepass，去掉前面的注释，并修改后面的密码。<br />常用配置文件例子 redis.conf
```java
#默认端口6379
port 6379
#绑定ip，如果是内网可以直接绑定 127.0.0.1, 或者忽略, 0.0.0.0是外网
bind 0.0.0.0
#守护进程启动
daemonize yes
#超时
timeout 300
loglevel notice
#分区
databases 16
save 900 1
save 300 10
save 60 10000
rdbcompression yes
#存储文件
dbfilename dump.rdb
#密码 abcd123
requirepass abcd123
```
<a name="Hb1PN"></a>
# 在 ubuntu 上安装 Redis
<br /> 按照下面给出的步骤在 Ubuntu 上安装 Redis：<br />首先使用 sudo 设置非 root 用户，然后安装构建和测试依赖项：
```java
sudo apt update 
sudo apt full-upgrade
sudo apt install build-essential tcl
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681694922660-d3ac747f-ebda-40b2-9429-fe60a342db05.png#averageHue=%23310c27&clientId=u7a939507-0444-4&from=paste&id=ueb3823c4&originHeight=417&originWidth=664&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=268552&status=done&style=none&taskId=u91b1e1c7-8c31-4904-b841-c184d7b3d63&title=)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681694943960-248e346b-f4c9-4133-8804-1b87f8440342.png#averageHue=%234d406e&clientId=u7a939507-0444-4&from=paste&id=ueb53992a&originHeight=417&originWidth=664&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=436365&status=done&style=none&taskId=uf03d01a5-1b70-4837-b950-d58dd35a41d&title=)<br />要继续按 Y 键<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681694958815-ebae96b9-6a9b-4644-9754-ecfcb0085cce.png#averageHue=%2343315c&clientId=u7a939507-0444-4&from=paste&id=ub1c0ff4a&originHeight=417&originWidth=664&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=382883&status=done&style=none&taskId=u78bc2309-d238-40b3-894a-50e221bea1a&title=)
<a name="flaau"></a>
## 安装 Redis 服务器
使用以下命令安装 Redis 服务器：<br />sudo apt-get install redis-server<br /> ![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681694974960-0be25dcb-607a-43d6-8270-ad8f1d954ac0.png#averageHue=%23412c55&clientId=u7a939507-0444-4&from=paste&id=udff0d777&originHeight=417&originWidth=664&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=361704&status=done&style=none&taskId=ucda75ef8-150a-4896-94f5-6df4d02ca02&title=)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681694989586-47e40efc-944f-490f-942d-e606d9da0aaa.png#averageHue=%234d406e&clientId=u7a939507-0444-4&from=paste&id=ua66e40c8&originHeight=417&originWidth=664&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=424682&status=done&style=none&taskId=u6867057e-d551-4211-89ab-1ac68cb1973&title=)<br />现在安装了 Redis Server。您可以启动 Redis 服务器：
<a name="FmHGJ"></a>
## 启动 Redis 服务器
您使用以下命令启动 redis 服务器： 
```java
 redis-server
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681695039547-ef7d15ae-3102-4afb-86d9-2e9712559c32.png#averageHue=%23320d28&clientId=u7a939507-0444-4&from=paste&id=ucc4a89f1&originHeight=614&originWidth=664&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=396723&status=done&style=none&taskId=ud75e6069-57e2-4173-b49c-1f81a3988e1&title=)
<a name="XxhJM"></a>
## 启动 Redis 客户端
Redis 服务器已启动，因此您可以启动 redis 客户端以在它们之间进行通信
```java
redis-cli
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681695049398-3b05fdb9-e1d7-47b5-bdab-996db9507d21.png#averageHue=%23300a24&clientId=u7a939507-0444-4&from=paste&id=ub16b928a&originHeight=243&originWidth=664&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=28850&status=done&style=none&taskId=ue638fa8f-c042-4ca3-bf21-cd651602f24&title=)
<a name="n7lAm"></a>
## 验证 Redis 是否正常工作
执行以下命令：<br />redis-cli<br />这将打开一个 redis 提示符。<br />**redis 127.0.0.1:6379>**<br />在上面的提示中，127.0.0.1 是机器的 IP 地址，6379 是 Redis 服务器运行的端口。<br />现在键入以下 PING 命令。返回 PONG 表示 Redis 已成功安装在您的系统上。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681695073923-f1721804-1018-426d-9626-49e7bfcdfdbb.png#averageHue=%23300a24&clientId=u7a939507-0444-4&from=paste&id=u5b4486c2&originHeight=397&originWidth=664&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=41269&status=done&style=none&taskId=u3aa97341-6841-41f8-bcb8-496ab7193f0&title=)
<a name="ypoJC"></a>
# Redis 配置详解
 在 Redis 中，Redis 的根目录中有一个配置文件（redis.conf）。您可以通过 Redis CONFIG 命令获取和设置所有 Redis 配置。<br />**句法**<br />以下是 Redis CONFIG 命令的基本语法。
```java
redis 127.0.0.1:6379> CONFIG GET CONFIG_SETTING_NAME
```
**例**
```
redis 127.0.0.1:6379> CONFIG GET loglevel
```
**输出**

1. “loglevel”
2. “verbose”

![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681695152811-6fd0119a-ef1a-4c95-965f-fa5cf2464059.png#averageHue=%23393534&clientId=u7a939507-0444-4&from=paste&id=ue94c1009&originHeight=164&originWidth=637&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=7939&status=done&style=none&taskId=uf4398304-c712-43aa-baa2-2b34e5111da&title=)<br />要获取所有配置设置，请使用 * 代替 CONFIG_SETTING_NAME<br /> **例**
```
redis 127.0.0.1:6379> CONFIG GET *
```
**输出**
```java
1. “dir”
2. “C:\\Program Files\\Redis”
3. “dbfilename”
4. “dump.rdb”
5. “requirepass”
6. (nil)
7. “masterauth”
8. (nil)
9. “maxmemory”
10. “0”
11. “maxmemory-policy”
12. “volatile-lru”
13. “maxmemory-samples”
14. “3”
15. “timeout”
16. “0”
17. “appendonly”
18. “no”
19. “no-appendfsync-on-rewrite”
20. “no”
21. “appendfsync”
22. “everysec”
23. “save”
24. “3600 1 300 100 60 10000”
25. “auto-aof-rewrite-percentage”
26. “100”
27. “auto-aof-rewrite-min-size”
28. “1048576”
29. “slave-serve-stale-data”
30. “yes”
31. “hash-max-zipmap-entries”
32. “512”
33. “hash-max-zipmap-value”
34. “64”
35. “list-max-ziplist-entries”
36. “512”
37. “list-max-ziplist-value”
38. “64”
39. “set-max-intset-entries”
40. “512”
41. “zset-max-ziplist-entries”
42. “128”
43. “zset-max-ziplist-value”
44. “64”
45. “slowlog-log-slower-than”
46. “10000”
47. “slowlog-max-len”
48. “64”
49. “loglevel”
50. “verbose”
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681695227917-2908c8b6-d0c2-4180-86e3-45caa3887596.png#averageHue=%231a1918&clientId=u7a939507-0444-4&from=paste&id=u9bf6b2bb&originHeight=462&originWidth=637&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=26735&status=done&style=none&taskId=u7f879baa-7e39-4a1c-aaab-cd41cb6e39f&title=)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681695232342-a21b8c0a-9826-4318-a69e-acb95f216be8.png#averageHue=%231a1918&clientId=u7a939507-0444-4&from=paste&id=ue7d523f3&originHeight=462&originWidth=637&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=26943&status=done&style=none&taskId=u003b1f4f-db2d-46d5-a041-50827573e26&title=)
<a name="mr4wZ"></a>
## 编辑配置
要更新配置，可以直接编辑 redis.conf 文件，也可以通过 CONFIG set 命令更新配置。<br />**语法**<br />以下是 CONFIG SET 命令的基本语法。
```
redis 127.0.0.1:6379> CONFIG SET CONFIG_SETTING_NAME NEW_CONFIG_VALUE
```
**例**
```
CONFIG GET "loglevel"
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681695265257-a59e6044-def7-4902-9a08-cbdc2cf98f42.png#averageHue=%23424140&clientId=u7a939507-0444-4&from=paste&id=u7bdfc419&originHeight=131&originWidth=637&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=7663&status=done&style=none&taskId=ucce5318f-dc91-4bc6-a28f-50be52ec6f1&title=)
<a name="x3lit"></a>
## 常用配置参数说明
redis.conf 配置项说明如下：<br />1.Redis 默认不是以守护进程的方式运行，可以通过该配置项修改，使用 yes 启用守护进程<br /> **daemonize no**<br />2.当 Redis 以守护进程方式运行时，Redis 默认会把 pid 写入 /var/run/redis.pid 文件，可以通过 pidfile 指定<br /> **pidfile /var/run/redis.pid**<br />3.指定 Redis 监听端口，默认端口为 6379，作者在自己的一篇博文中解释了为什么选用 6379 作为默认端口，因为 6379 在手机按键上 MERZ 对应的号码，而 MERZ 取自意大利歌女 Alessia Merz 的名字<br /> **port 6379**

1. 绑定的主机地址

 **bind 127.0.0.1**<br />5.当客户端闲置多长时间后关闭连接，如果指定为 0，表示关闭该功能<br /> **timeout 300**<br />6.指定日志记录级别，Redis 总共支持四个级别：debug、verbose、notice、warning，默认为 verbose<br /> **loglevel verbose**<br />7.日志记录方式，默认为标准输出，如果配置 Redis 为守护进程方式运行，而这里又配置为日志记录方式为标准输出，则日志将会发送给 /dev/null<br /> **logfile stdout**<br />8.设置数据库的数量，默认数据库为 0，可以使用 SELECT <dbid> 命令在连接上指定数据库 id<br /> **databases 16**<br />9.指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件配合<br /> **save <seconds> <changes>**<br />Redis 默认配置文件中提供了三个条件：<br /> **save 900 1**<br />**save 300 10**<br />**save 60 10000**<br />分别表示 90 0 秒（15 分钟）内有 1 个更改，300 秒（5 分钟）内有 10 个更改以及 60 秒内有 10000 个更改。<br />10.指定存储至本地数据库时是否压缩数据，默认为 yes，Redis 采用 LZF 压缩，如果为了节省 CPU 时间，可以关闭该选项，但会导致数据库文件变的巨大<br /> **rdbcompression yes**

1. 指定本地数据库文件名，默认值为 dump.rdb

 **dbfilename dump.rdb**

1. 指定本地数据库存放目录

 **dir ./**<br />13.设置当本机为 slave 服务时，设置 master 服务的 IP 地址及端口，在 Redis 启动时，它会自动从 master 进行数据同步<br /> **slaveof <masterip> <masterport>**

1. 当 master 服务设置了密码保护时，slave 服务连接 master 的密码

 **masterauth <master-password>**<br />15.设置 Redis 连接密码，如果配置了连接密码，客户端在连接 Redis 时需要通过 AUTH <password>命令提供密码，默认关闭<br /> **requirepass foobared**<br />16.设置同一时间最大客户端连接数，默认无限制，Redis 可以同时打开的客户端连接数为 Redis 进程可以打开的最大文件描述符数，如果设置 maxclients 0，表示不作限制。当客户端连接数到达限制时，Redis 会关闭新的连接并向客户端返回 max number of clients reached 错误信息<br /> **maxclients 128**<br />17.指定 Redis 最大内存限制，Redis 在启动时会把数据加载到内存中，达到最大内存后，Redis 会先尝试清除已到期或即将到期的 Key，当此方法处理后，仍然到达最大内存设置，将无法再进行写入操作，但仍然可以进行读取操作。Redis 新的 vm 机制，会把 Key 存放内存，Value 会存放在 swap 区<br /> **maxmemory <bytes>**<br />18.指定是否在每次更新操作后进行日志记录，Redis 在默认情况下是异步的把数据写入磁盘，如果不开启，可能会在断电时导致一段时间内的数据丢失。因为 redis 本身同步数据文件是按上面 save 条件来同步的，所以有的数据会在一段时间内只存在于内存中。默认为 no<br /> **appendonly no**

1. 指定更新日志文件名，默认为 appendonly.aof

 **appendfilename appendonly.aof**

1. 指定更新日志条件，共有 3 个可选值：  <br />**no**：表示等操作系统进行数据缓存同步到磁盘（快）  <br />**always**：表示每次更新操作后手动调用 fsync() 将数据写到磁盘（慢，安全） **everysec**：表示每秒同步一次（折衷，默认值）

 **appendfsync everysec**<br />21.指定是否启用虚拟内存机制，默认值为 no，简单的介绍一下，VM 机制将数据分页存放，由 Redis 将访问量较少的页即冷数据 swap 到磁盘上，访问多的页面由磁盘自动换出到内存中（在后面的文章我会仔细分析 Redis 的 VM 机制）<br /> **vm-enabled no**

1. 虚拟内存文件路径，默认值为 /tmp/redis.swap，不可多个 Redis 实例共享

 **vm-swap-file /tmp/redis.swap**<br />23.将所有大于 vm-max-memory 的数据存入虚拟内存,无论 vm-max-memory 设置多小,所有索引数据都是内存存储的( Redis 的索引数据就是 keys ),也就是说,当 vm-max-memory 设置为 0 的时候,其实是所有 value 都存在于磁盘。默认值为 0<br /> **vm-max-memory 0**

1. Redis swap 文件分成了很多的 page，一个对象可以保存在多个 page 上面，但一个 page 上不能被多个对象共享，vm-page-size 是要根据存储的数据大小来设定的，作者建议如果存储很多小对象，page 大小最好设置为 32 或者 64 bytes；如果存储很大大对象，则可以使用更大的 page，如果不确定，就使用默认值

 **vm-page-size 32**<br />25.设置 swap 文件中的 page 数量，由于页表（一种表示页面空闲或使用的 bitmap）是在放在内存中的，在磁盘上每 8 个 pages 将消耗 1byte 的内存。<br /> **vm-pages 134217728**<br />26.设置访问 swap 文件的线程数,最好不要超过机器的核数,如果设置为 0,那么所有对 swap 文件的操作都是串行的，可能会造成比较长时间的延迟。默认值为 4<br /> **vm-max-threads 4**

1. 设置在向客户端应答时，是否把较小的包合并为一个包发送，默认为开启

 **glueoutputbuf yes**<br />28.指定在超过一定的数量或者最大的元素超过某一临界值时，采用一种特殊的哈希算法<br /> **hash-max-zipmap-entries 64**<br />**hash-max-zipmap-value 512**<br />29.指定是否激活重置哈希，默认为开启（后面在介绍 Redis 的哈希算法时具体介绍）<br /> **activerehashing yes**<br />30.指定包含其它的配置文件，可以在同一主机上多个 Redis 实例之间使用同一份配置文件，而同时各个实例又拥有自己的特定配置文件<br /> **include /path/to/local.conf**
<a name="Z4QOh"></a>
# redis 支持的数据类型
Redis 数据库支持五种数据类型。

- 字符串（string）
- 哈希（hash）
- 列表（list）
- 集合（set）
- 有序集合（sorted set）
- 位图 ( Bitmaps )
- 基数统计 ( HyperLogLogs )
<a name="XHaaO"></a>
## 字符串
String 是一组字节。在 Redis 数据库中，字符串是二进制安全的。这意味着它们具有已知长度，并且不受任何特殊终止字符的影响。可以在一个字符串中存储最多 512 兆字节的内容。
<a name="UUHCV"></a>
### 例
使用 SET 命令在 name 键中存储字符串 redis.com.cn，然后使用 GET 命令查询 name。
```
SET name "redis.com.cn"  
OK  
GET name   
"redis.com.cn"
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681782304722-2b416f2a-4fdf-48a4-8ee7-1f281d69f5c3.png#averageHue=%23333231&clientId=u4f3bb963-3ce3-4&from=paste&id=uc69cca65&originHeight=178&originWidth=612&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=8916&status=done&style=none&taskId=uf1fe7a5f-1d73-4629-93ce-0f799d1a864&title=)<br /> 在上面的例子中，SET 和 GET 是 Redis 命令，name 是 Redis 中使用的 key，redis.com.cn 是存储在 Redis 中的字符串值。

---

<a name="cPmfv"></a>
## 哈希
哈希是键值对的集合。在 Redis 中，哈希是字符串字段和字符串值之间的映射。因此，它们适合表示对象。
<a name="cJkzR"></a>
### 例
让我们存储一个用户的对象，其中包含用户的基本信息。
```
HMSET user:1 username ajeet password javatpoint alexa 2000  
OK  
HGETALL  user:1  
"username"  
"ajeet"  
"password"  
"javatpoint"  
"alexa"  
"2000"
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681785517808-c09726c5-9e99-4567-b3e2-aca8fad8d9da.png#averageHue=%23333231&clientId=u4f3bb963-3ce3-4&from=paste&id=u65f1936c&originHeight=179&originWidth=765&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=12583&status=done&style=none&taskId=uc7ece79c-f9f4-40b9-8114-1bc53e393db&title=)<br />这里，HMSET 和 HGETALL 是 Redis 的命令，而 user：1 是键。<br />每个哈希可以存储多达 232-1 个字段-值对。

---

<a name="Tu3Y4"></a>
## 列表
Redis 列表定义为字符串列表，按插入顺序排序。可以将元素添加到 Redis 列表的头部或尾部。
<a name="vTUgj"></a>
### 例
```
lpush javatpoint java  
(integer) 1  
lpush javatpoint java  
(integer) 1  
lpush javatpoint java  
(integer) 1  
lpush javatpoint java  
(integer) 1  
lrange javatpoint 0 10  
"cassandra"  
"mongodb"  
"sql"  
"java"
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681785517971-381d9d71-605f-4b91-8bb8-084355631cdd.png#averageHue=%23272423&clientId=u4f3bb963-3ce3-4&from=paste&id=u778ba37c&originHeight=293&originWidth=614&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=18467&status=done&style=none&taskId=u20b0ee9e-2953-4c67-b1c5-ac7202b0230&title=)<br />列表的最大长度为 232 – 1 个元素（超过 40 亿个元素）。

---

<a name="evyJE"></a>
## 集合
集合（set）是 Redis 数据库中的无序字符串集合。在 Redis 中，添加，删除和查找的时间复杂度是 O(1)。
<a name="aXRnr"></a>
### 例
```
sadd tutoriallist redis  
(integer) 1  
redis 127.0.0.1:6379> sadd tutoriallist sql  
(integer) 1  
redis 127.0.0.1:6379> sadd tutoriallist postgresql  
(integer) 1  
redis 127.0.0.1:6379> sadd tutoriallist postgresql  
(integer) 0  
redis 127.0.0.1:6379> sadd tutoriallist postgresql  
(integer) 0  
redis 127.0.0.1:6379> smembers tutoriallist  
1) "redis"  
2) "postgresql"  
3) "sql"
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681785517985-d8aef06e-1996-4733-b18e-1a70b6d0ba17.png#averageHue=%23201f1e&clientId=u4f3bb963-3ce3-4&from=paste&id=ua6f157aa&originHeight=345&originWidth=676&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=21496&status=done&style=none&taskId=u70ecd222-d872-464c-b6aa-23f06c1b2e9&title=)<br />在上面的示例中，您可以看到 postgresql 被添加了三次，但由于该集的唯一属性，它只添加一次。<br />集合中的最大成员数为 232-1 个元素（超过 40 亿个元素）。

---

<a name="JCNTw"></a>
## 有序集合
Redis 有序集合类似于 Redis 集合，也是一组非重复的字符串集合。但是，排序集的每个成员都与一个分数相关联，该分数用于获取从最小到最高分数的有序排序集。虽然成员是独特的，但可以重复分数。
<a name="kqOxU"></a>
### 例
```
redis 127.0.0.1:6379> zadd tutoriallist 0 redis  
(integer) 1  
redis 127.0.0.1:6379> zadd tutoriallist 0 sql  
(integer) 1  
redis 127.0.0.1:6379> zadd tutoriallist 0 postgresql  
(integer) 1  
redis 127.0.0.1:6379> zadd tutoriallist 0 postgresql  
(integer) 0  
redis 127.0.0.1:6379> zadd tutoriallist 0 postgresql  
(integer) 0  
redis 127.0.0.1:6379> ZRANGEBYSCORE tutoriallist 0 10  
1) "postgresql"  
2) "redis"  
3) "sql"
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681785517967-b6537cd3-605f-4ef5-a8ce-5fd5c8d872e1.png#averageHue=%23232221&clientId=u4f3bb963-3ce3-4&from=paste&id=u666117a1&originHeight=307&originWidth=695&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=21731&status=done&style=none&taskId=u2c171ec2-edc1-45d7-95f2-ead237cc7ea&title=)

---

<a name="V9jmO"></a>
## 位图 Redis Bitmap
Redis Bitmap 通过类似 map 结构存放 0 或 1 ( bit 位 ) 作为值。<br />Redis Bitmap 可以用来统计状态，如日活是否浏览过某个东西。
<a name="eSfiN"></a>
### Redis setbit 命令
Redis setbit 命令用于设置或者清除一个 bit 位。
<a name="YiXzk"></a>
#### [*](https://redis.com.cn/redis-data-types.html#redis-setbit-%E5%91%BD%E4%BB%A4%E8%AF%AD%E6%B3%95%E6%A0%BC%E5%BC%8F)Redis setbit 命令语法格式
```
SETBIT key offset value
```
<a name="MVA3X"></a>
#### [*](https://redis.com.cn/redis-data-types.html#%E8%8C%83%E4%BE%8B)范例
```
127.0.0.1:6379> setbit aaa:001 10001 1 # 返回操作之前的数值
(integer) 0
127.0.0.1:6379> setbit aaa:001 10002 2 # 如果值不是0或1就报错
(error) ERR bit is not an integer or out of range
127.0.0.1:6379> setbit aaa:001 10002 0
(integer) 0
127.0.0.1:6379> setbit aaa:001 10003 1
(integer) 0
```

---

<a name="Lcn0s"></a>
### 基数统计 HyperLogLogs
Redis HyperLogLog 可以接受多个元素作为输入，并给出输入元素的基数估算值

- 基数

集合中不同元素的数量，比如 {'apple', 'banana', 'cherry', 'banana', 'apple'} 的基数就是 3

- 估算值

算法给出的基数并不是精确的，可能会比实际稍微多一些或者稍微少一些，但会控制在合 理的范围之内<br />HyperLogLog 的优点是：**即使输入元素的数量或者体积非常非常大，计算基数所需的空间总是固定的、并且是很小的**。<br />在 Redis 里面，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 264 个不同元素的基数。<br />这和计算基数时，元素越多耗费内存就越多的集合形成鲜明对比。<br />因为 HyperLogLog 只会根据输入元素来计算基数，而不会储存输入元素本身，所以 HyperLogLog 不能像集合那样，返回输入的各个元素。
<a name="Ndw3W"></a>
### Redis PFADD 命令
Redis PFADD 命令将元素添加至 HyperLogLog
<a name="WO4zU"></a>
#### [*](https://redis.com.cn/redis-data-types.html#redis-pfadd-%E5%91%BD%E4%BB%A4%E8%AF%AD%E6%B3%95%E6%A0%BC%E5%BC%8F)Redis PFADD 命令语法格式
```
PFADD key element [element ...]
```
<a name="NektI"></a>
#### [*](https://redis.com.cn/redis-data-types.html#%E8%8C%83%E4%BE%8B)范例
```
127.0.0.1:6379> PFADD unique::ip::counter '192.168.0.1'
(integer) 1
127.0.0.1:6379> PFADD unique::ip::counter '127.0.0.1'
(integer) 1
127.0.0.1:6379> PFADD unique::ip::counter '255.255.255.255'
(integer) 1
127.0.0.1:6379> PFCOUNT unique::ip::counter
(integer) 3
```
<a name="a6Dx4"></a>
# Redis 命令
Redis 命令用于在 redis 服务上执行操作。<br />要在 redis 服务上执行命令需要一个 redis 客户端。Redis 客户端在我们之前下载的 redis 安装包中。
<a name="blfkx"></a>
### 语法
Redis 客户端的基本语法为：
```
$ redis-cli
```
<a name="PUTH9"></a>
### 实例
以下实例讲解了如何启动 redis 客户端：<br />启动 redis 客户端，打开终端并输入命令 **redis-cli**。该命令会连接本地的 redis 服务。
```
$redis-cli
redis 127.0.0.1:6379>
redis 127.0.0.1:6379> PING

PONG
```
在以上实例中我们连接到本地的 redis 服务并执行 **PING** 命令，该命令用于检测 redis 服务是否启动。

---

<a name="YznUQ"></a>
## 在远程服务上执行命令
如果需要在远程 redis 服务上执行命令，同样我们使用的也是 **redis-cli** 命令。
<a name="QHmfb"></a>
### 语法
```
$ redis-cli -h host -p port -a password
```
<a name="yukAR"></a>
### 实例
以下实例演示了如何连接到主机为 127.0.0.1，端口为 6379 ，密码为 mypass 的 redis 服务上。
```
$redis-cli -h 127.0.0.1 -p 6379 -a "mypass"
redis 127.0.0.1:6379>
redis 127.0.0.1:6379> PING

PONG
```
redis 命令列表请查看 <br />[redis 命令手册](https://redis.com.cn/commands.html)<br /> 
<a name="Apif4"></a>
# Redis 键(Keys)
Redis Keys 作为参数与 Redis 命令配合使用。<br />**句法：**
```
redis 127.0.0.1:6379> COMMAND KEY_NAME
```
**例**<br />让我们以 Redis DEL 命令为例。如果键被删除，它将给出输出 1，否则它将为 0。
```
redis 127.0.0.1:6379> SET javatpoint redis   
OK   
redis 127.0.0.1:6379> DEL javatpoint  
(integer) 1
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681786732515-869a1b8a-1879-473e-8bb3-30051e5687c6.png#averageHue=%2330302f&clientId=u4f3bb963-3ce3-4&from=paste&id=u9c45f6b2&originHeight=189&originWidth=588&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=8884&status=done&style=none&taskId=u237b8851-4ed4-47fe-aad8-2d4cce357f9&title=)<br />这里，[DEL](https://redis.com.cn/commands/del.html) 是 Redis 命令，而 javatpoint 是键。

---

<a name="E5TtO"></a>
## Redis keys 命令
下表列出了 Redis 键相关的命令:

| 命令 | 描述 |
| --- | --- |
| [DEL](https://redis.com.cn/commands/del.html) | 用于删除 key |
| [DUMP](https://redis.com.cn/commands/dump.html) | 序列化给定 key ，并返回被序列化的值 |
| [EXISTS](https://redis.com.cn/commands/exists.html) | 检查给定 key 是否存在 |
| [EXPIRE](https://redis.com.cn/commands/expire.html) | 为给定 key 设置过期时间 |
| [EXPIREAT](https://redis.com.cn/commands/expireat.html) | 用于为 key 设置过期时间，接受的时间参数是 UNIX 时间戳 |
| [PEXPIRE](https://redis.com.cn/commands/pexpire.html) | 设置 key 的过期时间，以毫秒计 |
| [PEXPIREAT](https://redis.com.cn/commands/pexpireat.html) | 设置 key 过期时间的时间戳(unix timestamp)，以毫秒计 |
| [KEYS](https://redis.com.cn/commands/keys.html) | 查找所有符合给定模式的 key |
| [MOVE](https://redis.com.cn/commands/move.html) | 将当前数据库的 key 移动到给定的数据库中 |
| [PERSIST](https://redis.com.cn/commands/persist.html) | 移除 key 的过期时间，key 将持久保持 |
| [PTTL](https://redis.com.cn/commands/pttl.html) | 以毫秒为单位返回 key 的剩余的过期时间 |
| [TTL](https://redis.com.cn/commands/ttl.html) | 以秒为单位，返回给定 key 的剩余生存时间( |
| [RANDOMKEY](https://redis.com.cn/commands/randomkey.html) | 从当前数据库中随机返回一个 key |
| [RENAME](https://redis.com.cn/commands/rename.html) | 修改 key 的名称 |
| [RENAMENX](https://redis.com.cn/commands/renamenx.html) | 仅当 newkey 不存在时，将 key 改名为 newkey |
| [TYPE](https://redis.com.cn/commands/type.html) | 返回 key 所储存的值的类型 |

<a name="u6bCT"></a>
# Redis 字符串 ( Strings )
Redis 字符串命令用于管理 Redis 中的字符串值。<br />**语法：**
```
redis 127.0.0.1:6379> COMMAND KEY_NAME
```
**例**
```
redis 127.0.0.1:6379> SET javatpoint redis   
OK   
redis 127.0.0.1:6379> GET javatpoint   
"redis"
```
这里，SET 和 GET 是命令，“javatpoint” 是键。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681786783514-5e00c3f2-fec2-45d0-be87-92ca339aaa17.png#averageHue=%23323130&clientId=u4f3bb963-3ce3-4&from=paste&id=u99cad7af&originHeight=187&originWidth=589&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=10043&status=done&style=none&taskId=u4888bc08-3cbe-404e-82e5-078d0f3e3bc&title=)

---

<a name="RddDe"></a>
## Redis 字符串命令
以下是一些用于在 Redis 中管理字符串的基本命令的列表：

| 命令 | 描述 |
| --- | --- |
| [SET](https://redis.com.cn/commands/set.html) | 设置指定 key 的值 |
| [GET](https://redis.com.cn/commands/get.html) | 获取指定 key 的值 |
| [GETRANGE](https://redis.com.cn/commands/getrange.html) | 返回 key 中字符串值的子字符 |
| [GETSET](https://redis.com.cn/commands/getset.html) | 将给定 key 的值设为 value ，并返回 key 的旧值 ( old value ) |
| [GETBIT](https://redis.com.cn/commands/getbit.html) | 对 key 所储存的字符串值，获取指定偏移量上的位 ( bit ) |
| [MGET](https://redis.com.cn/commands/mget.html) | 获取所有(一个或多个)给定 key 的值 |
| [SETBIT](https://redis.com.cn/commands/setbit.html) | 对 key 所储存的字符串值，设置或清除指定偏移量上的位(bit) |
| [SETEX](https://redis.com.cn/commands/setex.html) | 设置 key 的值为 value 同时将过期时间设为 seconds |
| [SETNX](https://redis.com.cn/commands/setnx.html) | 只有在 key 不存在时设置 key 的值 |
| [SETRANGE](https://redis.com.cn/commands/setrange.html) | 从偏移量 offset 开始用 value 覆写给定 key 所储存的字符串值 |
| [STRLEN](https://redis.com.cn/commands/strlen.html) | 返回 key 所储存的字符串值的长度 |
| [MSET](https://redis.com.cn/commands/mset.html) | 同时设置一个或多个 key-value 对 |
| [MSETNX](https://redis.com.cn/commands/msetnx.html) | 同时设置一个或多个 key-value 对 |
| [PSETEX](https://redis.com.cn/commands/psetex.html) | 以毫秒为单位设置 key 的生存时间 |
| [INCR](https://redis.com.cn/commands/incr.html) | 将 key 中储存的数字值增一 |
| [INCRBY](https://redis.com.cn/commands/incrby.html) | 将 key 所储存的值加上给定的增量值 ( increment ) |
| [INCRBYFLOAT](https://redis.com.cn/commands/incrbyfloat.html) | 将 key 所储存的值加上给定的浮点增量值 ( increment ) |
| [DECR](https://redis.com.cn/commands/decr.html) | 将 key 中储存的数字值减一 |
| [DECRBY](https://redis.com.cn/commands/decrby.html) | 将 key 所储存的值减去给定的减量值 ( decrement ) |
| [APPEND](https://redis.com.cn/commands/append.html) | 将 value 追加到 key 原来的值的末尾 |

<a name="jD7l6"></a>
# Redis 哈希 ( Hashes )
<br /> Redis hash 是一个 string 类型的 field 和 value 的映射表，hash 特别适合用于存储对象。<br />每个哈希键中可以存储多达 40 亿个字段值对。
<a name="r9LB4"></a>
### 例
```
127.0.0.1:6379> HMSET rediscomcn name "redis" url "http://www.redis.com.cn" rank 1 visitors 240000000
OK
127.0.0.1:6379> HGETALL rediscomcn
1) "name"
2) "redis"
3) "url"
4) "http://www.qq.com"
5) "rank"
6) "1"
7) "visitors"
8) "230000000"
```
在上面的例子中，“rediscomcn” 是 Redis 哈希，它包含详细信息（name，url，rank，visitors）属性。

---

<a name="JoOM6"></a>
## Redis哈希命令
| 命令 | 描述 |
| --- | --- |
| [HDEL](https://redis.com.cn/commands/hdel.html) | 删除一个或多个哈希表字段 |
| [HEXISTS](https://redis.com.cn/commands/hexists.html) | 查看哈希表 key 中，指定的字段是否存在 |
| [HGET](https://redis.com.cn/commands/hget.html) | 获取存储在哈希表中指定字段的值 |
| [HGETALL](https://redis.com.cn/commands/hgetall.html) | 获取在哈希表中指定 key 的所有字段和值 |
| [HINCRBY](https://redis.com.cn/commands/hincrby.html) | 为哈希表 key 中的指定字段的整数值加上增量 increment |
| [HINCRBYFLOAT](https://redis.com.cn/commands/hincrbyfloat.html) | 为哈希表 key 中的指定字段的浮点数值加上增量 increment |
| [HKEYS](https://redis.com.cn/commands/hkeys.html) | 获取所有哈希表中的字段 |
| [HLEN](https://redis.com.cn/commands/hlen.html) | 获取哈希表中字段的数量 |
| [HMGET](https://redis.com.cn/commands/hmget.html) | 获取所有给定字段的值 |
| [HMSET](https://redis.com.cn/commands/hmset.html) | 同时将多个 field-value (域-值)对设置到哈希表 key 中 |
| [HSET](https://redis.com.cn/commands/hset.html) | 将哈希表 key 中的字段 field 的值设为 value |
| [HSETNX](https://redis.com.cn/commands/hsetnx.html) | 只有在字段 field 不存在时，设置哈希表字段的值 |
| [HVALS](https://redis.com.cn/commands/hvals.html) | 获取哈希表中所有值 |
| [HSCAN](https://redis.com.cn/commands/hscan.html) | 迭代哈希表中的键值对 |
| [HSTRLEN](https://redis.com.cn/commands/hstrlen.html) | 返回哈希表 key 中， 与给定域 field 相关联的值的字符串长度 |

<a name="dtzcz"></a>
# Redis 列表 ( Lists )
<br /> Redis 列表是按插入顺序排序的字符串列表。可以在列表的头部（左边）或尾部（右边）添加元素。<br />列表可以包含超过 40 亿 个元素 ( 232 - 1 )。<br />**例**<br />用 LPUSH 命令将三个值插入了名为 language 的列表当中:
```
redis 127.0.0.1:6379> LPUSH javatpoint sql  
(integer) 1  
redis 127.0.0.1:6379> LPUSH javatpoint mysql  
(integer) 2  
redis 127.0.0.1:6379> LPUSH javatpoint cassandra  
(integer) 3  
redis 127.0.0.1:6379> LRANGE javatpoint 0 10  
1) "cassandra"  
2) "mysql"  
3) "sql"  
redis 127.0.0.1:6379>
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681786829933-d9f16867-b471-489d-a91a-6df2fad5b4e7.png#averageHue=%23292625&clientId=u4f3bb963-3ce3-4&from=paste&id=u4759467b&originHeight=263&originWidth=624&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=15684&status=done&style=none&taskId=u259c44d8-99a1-4b82-8e5c-1c2826e72b1&title=)

---

<a name="iOzlZ"></a>
## Redis 列表命令
下表列出了列表相关命令：

| 命令 | 描述 |
| --- | --- |
| [BLPOP](https://redis.com.cn/commands/blpop.html) | 移出并获取列表的第一个元素 |
| [BRPOP](https://redis.com.cn/commands/brpop.html) | 移出并获取列表的最后一个元素 |
| [BRPOPLPUSH](https://redis.com.cn/commands/brpoplpush.html) | 从列表中弹出一个值，并将该值插入到另外一个列表中并返回它 |
| [LINDEX](https://redis.com.cn/commands/lindex.html) | 通过索引获取列表中的元素 |
| [LINSERT](https://redis.com.cn/commands/linsert.html) | 在列表的元素前或者后插入元素 |
| [LLEN](https://redis.com.cn/commands/llen.html) | 获取列表长度 |
| [LPOP](https://redis.com.cn/commands/lpop.html) | 移出并获取列表的第一个元素 |
| [LPUSH](https://redis.com.cn/commands/lpush.html) | 将一个或多个值插入到列表头部 |
| [LPUSHX](https://redis.com.cn/commands/lpushx.html) | 将一个值插入到已存在的列表头部 |
| [LRANGE](https://redis.com.cn/commands/lrange.html) | 获取列表指定范围内的元素 |
| [LREM](https://redis.com.cn/commands/lrem.html) | 移除列表元素 |
| [LSET](https://redis.com.cn/commands/lset.html) | 通过索引设置列表元素的值 |
| [LTRIM](https://redis.com.cn/commands/ltrim.html) | 对一个列表进行修剪(trim) |
| [RPOP](https://redis.com.cn/commands/rpop.html) | 移除并获取列表最后一个元素 |
| [RPOPLPUSH](https://redis.com.cn/commands/rpoplpush.html) | 移除列表的最后一个元素，并将该元素添加到另一个列表并返回 |
| [RPUSH](https://redis.com.cn/commands/rpush.html) | 在列表中添加一个或多个值 |
| [RPUSHX](https://redis.com.cn/commands/rpushx.html) | 为已存在的列表添加值 |

 
<a name="ZrpWK"></a>
# Redis 集合 ( Sets )
Redis 的 Set 是 string 类型的无序集合。<br />集合成员是唯一的，这就意味着集合中没有重复的数据。<br />在 Redis 中，添加、删除和查找的时间复杂都是 O(1)（不管 Set 中包含多少元素）。<br />集合中最大的成员数为 232 – 1 (4294967295), 每个集合可存储 40 多亿个成员。
<a name="JSgnI"></a>
## 例
通过 SADD 命令向名为 rediscomcn 的集合插入的三个元素:
```
redis 127.0.0.1:6379> SADD rediscomcn db2  
(integer) 1  
redis 127.0.0.1:6379> SADD rediscomcn mongodb  
(integer) 1  
redis 127.0.0.1:6379> SADD rediscomcn db2  
(integer) 0  
redis 127.0.0.1:6379> SADD rediscomcn cassandra  
(integer) 1  
redis 127.0.0.1:6379> SMEMBERS rediscomcn  
1) "cassandra"  
2) "db2"  
3) "mongodb"
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681786855082-ba14198d-ccd4-4bf3-b24f-b640257f9075.png#averageHue=%23262422&clientId=u4f3bb963-3ce3-4&from=paste&id=ue544642c&originHeight=284&originWidth=690&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=18945&status=done&style=none&taskId=u959e4260-9134-44f8-83c5-77d9f9186c6&title=)<br />在上面的示例中，我们使用 SADD 命令在集合中添加了 4 个元素。但是，使用 SMEMBERS 命令只检索了 3 个元素，因为有一个元素是重复的，Redis 只集合只含唯一元素。

---

<a name="QsIot"></a>
## Redis 集合命令
下表列出了 Redis 集合相关命令：

| 命令 | 描述 |
| --- | --- |
| [SADD](https://redis.com.cn/commands/sadd.html) | 向集合添加一个或多个成员 |
| [SCARD](https://redis.com.cn/commands/scard.html) | 获取集合的成员数 |
| [SDIFF](https://redis.com.cn/commands/sdiff.html) | 返回给定所有集合的差集 |
| [SDIFFSTORE](https://redis.com.cn/commands/sdiffstore.html) | 返回给定所有集合的差集并存储在 destination 中 |
| [SINTER](https://redis.com.cn/commands/sinter.html) | 返回给定所有集合的交集 |
| [SINTERSTORE](https://redis.com.cn/commands/sinterstore.html) | 返回给定所有集合的交集并存储在 destination 中 |
| [SISMEMBER](https://redis.com.cn/commands/sismember.html) | 判断 member 元素是否是集合 key 的成员 |
| [SMEMBERS](https://redis.com.cn/commands/smembers.html) | 返回集合中的所有成员 |
| [SMOVE](https://redis.com.cn/commands/smove.html) | 将 member 元素从 source 集合移动到 destination 集合 |
| [SPOP](https://redis.com.cn/commands/spop.html) | 移除并返回集合中的一个随机元素 |
| [SRANDMEMBER](https://redis.com.cn/commands/srandmember.html) | 返回集合中一个或多个随机数 |
| [SREM](https://redis.com.cn/commands/srem.html) | 移除集合中一个或多个成员 |
| [SUNION](https://redis.com.cn/commands/sunion.html) | 返回所有给定集合的并集 |
| [SUNIONSTORE](https://redis.com.cn/commands/sunionstore.html) | 所有给定集合的并集存储在 destination 集合中 |
| [SSCAN](https://redis.com.cn/commands/sscan.html) | 迭代集合中的元素 |

<a name="AtqZd"></a>
# Redis 有序集合 ( Sorted Sets )
Redis 有序集合和集合一样也是 string 类型元素的集合，且不允许重复的成员。<br />不同的是每个元素都会关联一个 double 类型的分数。Redis 正是通过分数来为集合中的成员进行从小到大的排序。<br />有序集合的成员是唯一的,但分数 ( score ) 却可以重复。<br />集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)。<br />集合中最大的成员数为 232 – 1 ( 4294967295 ) , 每个集合可存储 40 多亿个成员。
<a name="oVtFz"></a>
## 例
通过 ZADD 命令向 Redis 的有序集合中添加了三个值并关联分数:
```
redis 127.0.0.1:6379> ZADD rediscomcn 1 redis  
(integer) 0  
redis 127.0.0.1:6379> ZADD rediscomcn 2 cassandra  
(integer) 1  
redis 127.0.0.1:6379> ZADD rediscomcn 3 cassandra  
(integer) 0  
redis 127.0.0.1:6379> ZADD rediscomcn 3 mysql  
(integer) 1  
redis 127.0.0.1:6379> ZADD rediscomcn 4 mysql  
(integer) 0  
redis 127.0.0.1:6379> ZRANGE rediscomcn 0 10 WITHSCORES  
1) "redis"  
2) "1"  
3) "cassandra"  
4) "3"  
5) "mysql"  
6) "4"
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681786874770-c3009504-0086-466c-b782-15fbd7073941.png#averageHue=%231e1d1c&clientId=u4f3bb963-3ce3-4&from=paste&id=ua9b79cf2&originHeight=371&originWidth=686&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=23989&status=done&style=none&taskId=ufc6baa34-d796-4581-ac35-541e9ecfdf3&title=)

---

<a name="RdDiS"></a>
## Redis 有序集合命令
下表列出了 Redis 有序集合的基本命令

| 命令 | 描述 |
| --- | --- |
| [ZADD](https://redis.com.cn/commands/zadd.html) | 向有序集合添加一个或多个成员，或者更新已存在成员的分数 |
| [ZCARD](https://redis.com.cn/commands/zcard.html) | 获取有序集合的成员数 |
| [ZCOUNT](https://redis.com.cn/commands/zcount.html) | 计算在有序集合中指定区间分数的成员数 |
| [ZINCRBY](https://redis.com.cn/commands/zincrby.html) | 有序集合中对指定成员的分数加上增量 increment |
| [ZINTERSTORE](https://redis.com.cn/commands/zinterstore.html) | 计算给定的一个或多个有序集的交集并将结果集存储在新的有序集合 key 中 |
| [ZLEXCOUNT](https://redis.com.cn/commands/zlexcount.html) | 在有序集合中计算指定字典区间内成员数量 |
| [ZRANGE](https://redis.com.cn/commands/zrange.html) | 通过索引区间返回有序集合成指定区间内的成员 |
| [ZRANGEBYLEX](https://redis.com.cn/commands/zrangebylex.html) | 通过字典区间返回有序集合的成员 |
| [ZRANGEBYSCORE](https://redis.com.cn/commands/zrangebyscore.html) | 通过分数返回有序集合指定区间内的成员 |
| [ZRANK](https://redis.com.cn/commands/zrank.html) | 返回有序集合中指定成员的索引 |
| [ZREM](https://redis.com.cn/commands/zrem.html) | 移除有序集合中的一个或多个成员 |
| [ZREMRANGEBYLEX](https://redis.com.cn/commands/zremrangebylex.html) | 移除有序集合中给定的字典区间的所有成员 |
| [ZREMRANGEBYRANK](https://redis.com.cn/commands/zremrangebyrank.html) | 移除有序集合中给定的排名区间的所有成员 |
| [ZREMRANGEBYSCORE](https://redis.com.cn/commands/zremrangebyscore.html) | 移除有序集合中给定的分数区间的所有成员 |
| [ZREVRANGE](https://redis.com.cn/commands/zrevrange.html) | 返回有序集中指定区间内的成员，通过索引，分数从高到底 |
| [ZREVRANGEBYSCORE](https://redis.com.cn/commands/zrevrangebyscore.html) | 返回有序集中指定分数区间内的成员，分数从高到低排序 |
| [ZREVRANK](https://redis.com.cn/commands/zrevrank.html) | 返回有序集合中指定成员的排名，有序集成员按分数值递减(从大到小)排序 |
| [ZSCORE](https://redis.com.cn/commands/zscore.html) | 返回有序集中，成员的分数值 |
| [ZUNIONSTORE](https://redis.com.cn/commands/zunionstore.html) | 计算一个或多个有序集的并集，并存储在新的 key 中 |
| [ZSCAN](https://redis.com.cn/commands/zscan.html) | 迭代有序集合中的元素（包括元素成员和元素分值） |

<a name="pU6Ql"></a>
# Redis发布与订阅

<a name="NZvRn"></a>
# Redis HyperLogLog
<br /> Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定 的、并且是很小的。<br />在 Redis 里面，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 264 个不同元素的基 数。这和计算基数时，元素越多耗费内存就越多的集合形成鲜明对比。<br />但是，因为 HyperLogLog 只会根据输入元素来计算基数，而不会储存输入元素本身，所以 HyperLogLog 不能像集合那样，返回输入的各个元素。
<a name="LOjNH"></a>
## 什么是基数?
比如数据集 {1, 3, 5, 7, 5, 7, 8}， 那么这个数据集的基数集为 {1, 3, 5 ,7, 8}, 基数(不重复元素)为5。 基数估计就是在误差可接受的范围内，快速计算基数。
<a name="bXxzQ"></a>
## 实例
以下实例演示了 HyperLogLog 的工作过程：
```
redis 127.0.0.1:6379> PFADD rediscomcn "redis"
1) (integer) 1
redis 127.0.0.1:6379> PFADD rediscomcn "mongodb"
1) (integer) 1
redis 127.0.0.1:6379> PFADD rediscomcn "mysql"
1) (integer) 1
redis 127.0.0.1:6379> PFCOUNT rediscomcn
(integer) 3
```
<a name="jLRex"></a>
## Redis HyperLogLog命令
下表列出了列表相关命令：

| 命令 | 描述 |
| --- | --- |
| [PFADD](https://redis.com.cn/commands/pfadd.html) | 添加指定元素到 HyperLogLog 中。 |
| [PFCOUNT](https://redis.com.cn/commands/pfcount.html) | 返回给定 HyperLogLog 的基数估算值。 |
| [PFMERGE](https://redis.com.cn/commands/pfmerge.html) | 将多个 HyperLogLog 合并为一个 HyperLogLog |

<a name="jQDmG"></a>
# Redis 地理信息GEO

Redis GEO 主要用于存储地理位置信息，并对存储的信息进行操作，该功能在 Redis 3.2 版本新增。<br />Redis GEO 操作方法有：

- geoadd：添加地理位置的坐标。
- geopos：获取地理位置的坐标。
- geodist：计算两个位置之间的距离。
- georadius：根据用户给定的经纬度坐标来获取指定范围内的地理位置集合。
- georadiusbymember：根据储存在位置集合里面的某个地点获取指定范围内的地理位置集合。
- geohash：返回一个或多个位置对象的 geohash 值。
<a name="Aw965"></a>
### geoadd
geoadd 用于存储指定的地理空间位置，可以将一个或多个经度(longitude)、纬度(latitude)、位置名称(member)添加到指定的 key 中。<br />geoadd 语法格式如下：
```
GEOADD key longitude latitude member [longitude latitude member ...]
```
以下实例中 key 为 Sicily、Catania 为位置名称 ：
<a name="zibNw"></a>
## 实例
```
redis> GEOADD Sicily 13.361389 38.115556 "Palermo" 15.087269 37.502669 "Catania"
(integer) 2
redis> GEODIST Sicily Palermo Catania
"166274.1516"
redis> GEORADIUS Sicily 15 37 100 km
1) "Catania"
redis> GEORADIUS Sicily 15 37 200 km
1) "Palermo"
2) "Catania"
redis>
```
<a name="BFnCC"></a>
### geopos
geopos 用于从给定的 key 里返回所有指定名称(member)的位置（经度和纬度），不存在的返回 nil。<br />geopos 语法格式如下：
```
GEOPOS key member [member ...]
```
<a name="QAiQv"></a>
## 实例
```
redis> GEOADD Sicily 13.361389 38.115556 "Palermo" 15.087269 37.502669 "Catania"
(integer) 2
redis> GEOPOS Sicily Palermo Catania NonExisting
1) 1) "13.36138933897018433"
  2) "38.11555639549629859"
2) 1) "15.08726745843887329"
  2) "37.50266842333162032"
3) (nil)
redis>
```
<a name="osjUB"></a>
### geodist
geodist 用于返回两个给定位置之间的距离。<br />geodist 语法格式如下：
```
GEODIST key member1 member2 [m|km|ft|mi]
```
member1 member2 为两个地理位置。<br />最后一个距离单位参数说明：

- m ：米，默认单位。
- km ：千米。
- mi ：英里。
- ft ：英尺。
<a name="JSg67"></a>
## 计算 Palermo 与 Catania 之间的距离实例
```
redis> GEOADD Sicily 13.361389 38.115556 "Palermo" 15.087269 37.502669 "Catania"
(integer) 2
redis> GEODIST Sicily Palermo Catania
"166274.1516"
redis> GEODIST Sicily Palermo Catania km
"166.2742"
redis> GEODIST Sicily Palermo Catania mi
"103.3182"
redis> GEODIST Sicily Foo Bar
(nil)
redis>
```
<a name="faCgS"></a>
### georadius、georadiusbymember
georadius 以给定的经纬度为中心， 返回键包含的位置元素当中， 与中心的距离不超过给定最大距离的所有位置元素。<br />georadiusbymember 和 GEORADIUS 命令一样， 都可以找出位于指定范围内的元素， 但是 georadiusbymember 的中心点是由给定的位置元素决定的， 而不是使用经度和纬度来决定中心点。<br />georadius 与 georadiusbymember 语法格式如下：
```
GEORADIUS key longitude latitude radius m|km|ft|mi [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count] [ASC|DESC] [STORE key] [STOREDIST key]
GEORADIUSBYMEMBER key member radius m|km|ft|mi [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count] [ASC|DESC] [STORE key] [STOREDIST key]
```
参数说明：

- m ：米，默认单位。
- km ：千米。
- mi ：英里。
- ft ：英尺。
- WITHDIST: 在返回位置元素的同时， 将位置元素与中心之间的距离也一并返回。
- WITHCOORD: 将位置元素的经度和维度也一并返回。
- WITHHASH: 以 52 位有符号整数的形式， 返回位置元素经过原始 geohash 编码的有序集合分值。 这个选项主要用于底层应用或者调试， 实际中的作用并不大。
- COUNT 限定返回的记录数。
- ASC: 查找结果根据距离从近到远排序。
- DESC: 查找结果根据从远到近排序。
<a name="mn0Au"></a>
## georadius实例
```
redis> GEOADD Sicily 13.361389 38.115556 "Palermo" 15.087269 37.502669 "Catania"
(integer) 2
redis> GEORADIUS Sicily 15 37 200 km WITHDIST
1) 1) "Palermo"
  2) "190.4424"
2) 1) "Catania"
  2) "56.4413"
redis> GEORADIUS Sicily 15 37 200 km WITHCOORD
1) 1) "Palermo"
  2) 1) "13.36138933897018433"
   2) "38.11555639549629859"
2) 1) "Catania"
  2) 1) "15.08726745843887329"
   2) "37.50266842333162032"
redis> GEORADIUS Sicily 15 37 200 km WITHDIST WITHCOORD
1) 1) "Palermo"
  2) "190.4424"
  3) 1) "13.36138933897018433"
   2) "38.11555639549629859"
2) 1) "Catania"
  2) "56.4413"
  3) 1) "15.08726745843887329"
   2) "37.50266842333162032"
redis>
```
<a name="ciY8y"></a>
## georadiusbymember实例
```
redis> GEOADD Sicily 13.583333 37.316667 "Agrigento"
(integer) 1
redis> GEOADD Sicily 13.361389 38.115556 "Palermo" 15.087269 37.502669 "Catania"
(integer) 2
redis> GEORADIUSBYMEMBER Sicily Agrigento 100 km
1) "Agrigento"
2) "Palermo"
redis>
```
<a name="fn59M"></a>
### geohash
Redis GEO 使用 geohash 来保存地理位置的坐标。<br />geohash 用于获取一个或多个位置元素的 geohash 值。<br />geohash 语法格式如下：
```
GEOHASH key member [member ...]
```
<a name="gEbGe"></a>
## geohash实例
```
redis> GEOADD Sicily 13.361389 38.115556 "Palermo" 15.087269 37.502669 "Catania"
(integer) 2
redis> GEOHASH Sicily Palermo Catania
1) "sqc8b49rny0"
2) "sqdtr74hyu0"
redis>
```
<a name="Rg3k9"></a>
# Redis Stream
<br /> Redis Stream 是 Redis 5.0 版本新增加的数据结构。<br />Redis Stream 主要用于消息队列（MQ，Message Queue），Redis 本身是有一个 Redis 发布订阅 (pub/sub) 来实现消息队列的功能，但它有个缺点就是消息无法持久化，如果出现网络断开、Redis 宕机等，消息就会被丢弃。<br />简单来说发布订阅 (pub/sub) 可以分发消息，但无法记录历史消息。<br />而 Redis Stream 提供了消息的持久化和主备复制功能，可以让任何客户端访问任何时刻的数据，并且能记住每一个客户端的访问位置，还能保证消息不丢失。<br />Redis Stream 的结构如下所示，它有一个消息链表，将所有加入的消息都串起来，每个消息都有一个唯一的 ID 和对应的内容：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681787149901-941fa913-f611-4345-89c1-196048492fe5.png#averageHue=%23d9c18c&clientId=u4f3bb963-3ce3-4&from=paste&id=u6820966a&originHeight=397&originWidth=610&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=22568&status=done&style=none&taskId=u3a96debd-4b5c-4c75-b8a8-c62180abd11&title=)<br />每个 Stream 都有唯一的名称，它就是 Redis 的 key，在我们首次使用 xadd 指令追加消息时自动创建。<br />上图解析：

- Consumer Group ：消费组，使用 XGROUP CREATE 命令创建，一个消费组有多个消费者(Consumer)。
- last_delivered_id ：游标，每个消费组会有个游标 last_delivered_id，任意一个消费者读取了消息都会使游标 last_delivered_id 往前移动。
- pending_ids ：消费者(Consumer)的状态变量，作用是维护消费者的未确认的 id。 pending_ids 记录了当前已经被客户端读取的消息，但是还没有 ack (Acknowledge character：确认字符）。

消息队列相关命令：

- XADD - 添加消息到末尾
- XTRIM - 对流进行修剪，限制长度
- XDEL - 删除消息
- XLEN - 获取流包含的元素数量，即消息长度
- XRANGE - 获取消息列表，会自动过滤已经删除的消息
- XREVRANGE - 反向获取消息列表，ID 从大到小
- XREAD - 以阻塞或非阻塞方式获取消息列表

消费者组相关命令：

- XGROUP CREATE - 创建消费者组
- XREADGROUP GROUP - 读取消费者组中的消息
- XACK - 将消息标记为"已处理"
- XGROUP SETID - 为消费者组设置新的最后递送消息ID
- XGROUP DELCONSUMER - 删除消费者
- XGROUP DESTROY - 删除消费者组
- XPENDING - 显示待处理消息的相关信息
- XCLAIM - 转移消息的归属权
- XINFO - 查看流和消费者组的相关信息；
- XINFO GROUPS - 打印消费者组的信息；
- XINFO STREAM - 打印流信息
<a name="C8mGx"></a>
### XADD
使用 [XADD](https://redis.com.cn/commands/xadd.html) 向队列添加消息，如果指定的队列不存在，则创建一个队列，XADD 语法格式：
```
XADD key ID field value [field value ...]
```

- key ：队列名称，如果不存在就创建
- ID ：消息 id，我们使用 * 表示由 redis 生成，可以自定义，但是要自己保证递增性。
- field value ： 记录。
<a name="DtKTe"></a>
## XADD实例
```
redis> XADD mystream * name Sara surname OConnor
"1601372323627-0"
redis> XADD mystream * field1 value1 field2 value2 field3 value3
"1601372323627-1"
redis> XLEN mystream
(integer) 2
redis> XRANGE mystream - +
1) 1) "1601372323627-0"
  2) 1) "name"
   2) "Sara"
   3) "surname"
   4) "OConnor"
2) 1) "1601372323627-1"
  2) 1) "field1"
   2) "value1"
   3) "field2"
   4) "value2"
   5) "field3"
   6) "value3"
redis>
```
<a name="LzRQA"></a>
### XTRIM
使用 [XTRIM](https://redis.com.cn/commands/xtrim.html) 对流进行修剪，限制长度， 语法格式：
```
XTRIM key MAXLEN [~] count
```

- key ：队列名称
- MAXLEN ：长度
- count ：数量
<a name="QkL1d"></a>
## XTRIM实例
```
127.0.0.1:6379> XADD mystream * field1 A field2 B field3 C field4 D
"1601372434568-0"
127.0.0.1:6379> XTRIM mystream MAXLEN 2
(integer) 0
127.0.0.1:6379> XRANGE mystream - +
1) 1) "1601372434568-0"
  2) 1) "field1"
   2) "A"
   3) "field2"
   4) "B"
   5) "field3"
   6) "C"
   7) "field4"
   8) "D"
127.0.0.1:6379>

redis>
```
<a name="WXbcJ"></a>
### XDEL
使用 [XDEL](https://redis.com.cn/commands/xdel.html) 删除消息，语法格式：
```
XDEL key ID [ID ...]
```

- key：队列名称
- ID ：消息 ID
<a name="pbFbd"></a>
## XDEL实例
```
127.0.0.1:6379> XADD mystream * a 1
1538561698944-0
127.0.0.1:6379> XADD mystream * b 2
1538561700640-0
127.0.0.1:6379> XADD mystream * c 3
1538561701744-0
127.0.0.1:6379> XDEL mystream 1538561700640-0
(integer) 1
127.0.0.1:6379> XRANGE mystream - +
1) 1) 1538561698944-0
   2) 1) "a"
      2) "1"
2) 1) 1538561701744-0
   2) 1) "c"
      2) "3"
```
<a name="FZ3pw"></a>
### XLEN
使用 [XLEN](https://redis.com.cn/commands/xlen.html) 获取流包含的元素数量，即消息长度，语法格式：
```
XLEN key
```

- key：队列名称
<a name="ZJSq9"></a>
## XLEN实例
```
redis> XADD mystream * item 1
"1601372563177-0"
redis> XADD mystream * item 2
"1601372563178-0"
redis> XADD mystream * item 3
"1601372563178-1"
redis> XLEN mystream
(integer) 3
redis>
```
<a name="OQWIh"></a>
### XRANGE
使用 XRANGE 获取消息列表，会自动过滤已经删除的消息 ，语法格式：
```
XRANGE key start end [COUNT count]
```

- key ：队列名
- start ：开始值， - 表示最小值
- end ：结束值， + 表示最大值
- count ：数量
<a name="r5ufU"></a>
## 实例
```
redis> XADD writers * name Virginia surname Woolf
"1601372577811-0"
redis> XADD writers * name Jane surname Austen
"1601372577811-1"
redis> XADD writers * name Toni surname Morrison
"1601372577811-2"
redis> XADD writers * name Agatha surname Christie
"1601372577812-0"
redis> XADD writers * name Ngozi surname Adichie
"1601372577812-1"
redis> XLEN writers
(integer) 5
redis> XRANGE writers - + COUNT 2
1) 1) "1601372577811-0"
   2) 1) "name"
   2) "Virginia"
   3) "surname"
   4) "Woolf"
2) 1) "1601372577811-1"
   2) 1) "name"
   2) "Jane"
   3) "surname"
   4) "Austen"
redis>
```
<a name="DfbBz"></a>
### XREVRANGE
使用 [XREVRANGE](https://redis.com.cn/commands/xrevrange.html) 获取消息列表，会自动过滤已经删除的消息 ，语法格式：
```
XREVRANGE key end start [COUNT count]
```

- key ：队列名
- end ：结束值， + 表示最大值
- start ：开始值， - 表示最小值
- count ：数量
<a name="VSZLD"></a>
## XREVRANGE实例
```
redis> XADD writers * name Virginia surname Woolf
"1601372731458-0"
redis> XADD writers * name Jane surname Austen
"1601372731459-0"
redis> XADD writers * name Toni surname Morrison
"1601372731459-1"
redis> XADD writers * name Agatha surname Christie
"1601372731459-2"
redis> XADD writers * name Ngozi surname Adichie
"1601372731459-3"
redis> XLEN writers
(integer) 5
redis> XREVRANGE writers + - COUNT 1
1) 1) "1601372731459-3"
   2) 1) "name"
   2) "Ngozi"
   3) "surname"
   4) "Adichie"
redis>
```
<a name="NUjOI"></a>
### XREAD
使用 XREAD 以阻塞或非阻塞方式获取消息列表 ，语法格式：
```
XREAD [COUNT count] [BLOCK milliseconds] STREAMS key [key ...] id [id ...]
```

- count ：数量
- milliseconds ：可选，阻塞毫秒数，没有设置就是非阻塞模式
- key ：队列名
- id ：消息 ID
<a name="aDkCo"></a>
## 实例
_# 从 Stream 头部读取两条消息_
```
redis> XREAD COUNT 2 STREAMS mystream writers 0-0 0-0
1) 1) "mystream"
  2) 1) 1) 1526984818136-0
     2) 1) "duration"
      2) "1532"
      3) "event-id"
      4) "5"
      5) "user-id"
      6) "7782813"
   2) 1) 1526999352406-0
     2) 1) "duration"
      2) "812"
      3) "event-id"
      4) "9"
      5) "user-id"
      6) "388234"
2) 1) "writers"
  2) 1) 1) 1526985676425-0
     2) 1) "name"
      2) "Virginia"
      3) "surname"
      4) "Woolf"
   2) 1) 1526985685298-0
     2) 1) "name"
      2) "Jane"
      3) "surname"
      4) "Austen"
```
<a name="G7ScG"></a>
### XGROUP CREATE
使用 XGROUP CREATE 创建消费者组，语法格式：
```
XGROUP [CREATE key groupname id-or-$] [SETID key groupname id-or-$] [DESTROY key groupname] [DELCONSUMER key groupname consumername]
```

- key ：队列名称，如果不存在就创建
- groupname ：组名。
- $ ： 表示从尾部开始消费，只接受新消息，当前 Stream 消息会全部忽略。

从头开始消费:
```
XGROUP CREATE mystream consumer-group-name 0-0
```
从尾部开始消费:
```
XGROUP CREATE mystream consumer-group-name $
```
<a name="Cn6Gg"></a>
### XREADGROUP GROUP
使用 XREADGROUP GROUP 读取消费组中的消息，语法格式：
```
XREADGROUP GROUP group consumer [COUNT count] [BLOCK milliseconds] [NOACK] STREAMS key [key ...] ID [ID ...]
```

- group ：消费组名
- consumer ：消费者名。
- count ： 读取数量。
- milliseconds ： 阻塞毫秒数。
- key ： 队列名。
- ID ： 消息 ID。
```
XREADGROUP GROUP consumer-group-name consumer-name COUNT 1 STREAMS mystream >
```
<a name="n2A7G"></a>
# Redis 事务
<br /> 事务是指**一个完整的动作，要么全部执行，要么什么也没有做**。<br />Redis 事务不是严格意义上的事务，只是用于帮助用户在一个步骤中执行多个命令。单个 Redis 命令的执行是原子性的，但 Redis 没有在事务上增加任何维持原子性的机制，所以 Redis 事务的执行并不是原子性的。<br />Redis 事务可以理解为一个打包的批量执行脚本，但批量指令并非原子化的操作，中间某条指令的失败不会导致前面已做指令的回滚，也不会造成后续的指令不做。<br />Redis 事务可以一次执行多个命令， 并且带有以下三个重要的保证：

- 批量操作在发送 EXEC 命令前被放入队列缓存。
- 收到 EXEC 命令后进入事务执行，事务中任意命令执行失败，其余的命令依然被执行。
- 在事务执行过程，其他客户端提交的命令请求不会插入到事务执行命令序列中。

一个事务从开始到执行会经历以下三个阶段：

- 开始事务。
- 命令入队。
- 执行事务。

MULTI、EXEC、DISCARD、WATCH 这四个指令构成了 redis 事务处理的基础。

1. MULTI 用来组装一个事务；
2. EXEC 用来执行一个事务；
3. DISCARD 用来取消一个事务；
4. WATCH 用来监视一些 key，一旦这些 key 在事务执行之前被改变，则取消事务的执行。

在 Redis 中，通过使用**MULTI**命令启动事务，然后需要传递应在事务中执行的命令列表，之后整个事务由**EXEC**命令执行。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681787211145-545557c4-3372-451f-8368-7ecf6e1e9d80.png#averageHue=%23423f3d&clientId=u4f3bb963-3ce3-4&from=paste&id=u19420121&originHeight=137&originWidth=585&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=6075&status=done&style=none&taskId=ue12282a9-c4c0-4cc5-8a29-a0eafe3195b&title=)\<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681787211130-3f4c86a2-78be-4a3f-b71c-9488b92ae645.png#averageHue=%2343403f&clientId=u4f3bb963-3ce3-4&from=paste&id=u75d29a11&originHeight=137&originWidth=585&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=7743&status=done&style=none&taskId=u4b5bdb95-246d-49b3-b2a9-a66c8317464&title=)
<a name="ZczKq"></a>
## 例子
如何启动和执行 Redis 事务。
```
redis 127.0.0.1:6379> MULTI  
OK  
redis 127.0.0.1:6379> EXEC  
(empty list or set)  
redis 127.0.0.1:6379> MULTI  
OK  
redis 127.0.0.1:6379> SET rediscomcn redis  
QUEUED  
redis 127.0.0.1:6379> GET rediscomcn  
QUEUED  
redis 127.0.0.1:6379> INCR visitors  
QUEUED  
redis 127.0.0.1:6379> EXEC  
1) OK  
2) "redis"  
3) (integer) 1
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681787211112-2f44bd99-4da5-46a2-8efc-f85899d08c1d.png#averageHue=%231f1e1e&clientId=u4f3bb963-3ce3-4&from=paste&id=udb0314b7&originHeight=346&originWidth=620&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=19291&status=done&style=none&taskId=u275db27e-cf8d-49de-b209-4245eec1d2b&title=)<br />在上面的例子中，我们看到了 QUEUED 的字样，这表示我们在用 MULTI 组装事务时，每一个命令都会进入到内存队列中缓存起来，如果出现 QUEUED 则表示我们这个命令成功插入了缓存队列，在将来执行 EXEC 时，这些被 QUEUED 的命令都会被组装成一个事务来执行。<br />对于事务的执行来说，如果 redis 开启了 AOF 持久化的话，那么一旦事务被成功执行，事务中的命令就会通过 write 命令一次性写到磁盘中去，如果在向磁盘中写的过程中恰好出现断电、硬件故障等问题，那么就可能出现只有部分命令进行了 AOF 持久化，这时 AOF 文件就会出现不完整的情况，这时，我们可以使用 redis-check-aof 工具来修复这一问题，这个工具会将 AOF 文件中不完整的信息移除，确保 AOF 文件完整可用。

---

<a name="TI4WI"></a>
## Redis 事务错误
有关事务，大家经常会遇到的是两类错误：

1. 调用 EXEC 之前的错误
2. 调用 EXEC 之后的错误

**调用 EXEC 之前的错误**，有可能是由于语法有误导致的，也可能时由于内存不足导致的。只要出现某个命令无法成功写入缓冲队列的情况，redis 都会进行记录，在客户端调用 EXEC 时，redis 会拒绝执行这一事务。（这是 2.6.5 版本之后的策略。在 2.6.5 之前的版本中，redis 会忽略那些入队失败的命令，只执行那些入队成功的命令）。我们来看一个这样的例子：
```
127.0.0.1:6379> multi
OK
127.0.0.1:6379> haha //一个明显错误的指令
(error) ERR unknown command 'haha'
127.0.0.1:6379> ping
QUEUED
127.0.0.1:6379> exec
//redis无情的拒绝了事务的执行，原因是“之前出现了错误”
(error) EXECABORT Transaction discarded because of previous errors.
```
而对于**调用 EXEC 之后的错误**，redis 则采取了完全不同的策略，即 redis 不会理睬这些错误，而是继续向下执行事务中的其他命令。这是因为，对于应用层面的错误，并不是 redis 自身需要考虑和处理的问题，所以一个事务中如果某一条命令执行失败，并不会影响接下来的其他命令的执行。我们也来看一个例子：
```
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set age 23
QUEUED
//age不是集合，所以如下是一条明显错误的指令
127.0.0.1:6379> sadd age 15 
QUEUED
127.0.0.1:6379> set age 29
QUEUED
127.0.0.1:6379> exec //执行事务时，redis不会理睬第2条指令执行错误
1) OK
2) (error) WRONGTYPE Operation against a key holding the wrong kind of value
3) OK
127.0.0.1:6379> get age
"29" //可以看出第3条指令被成功执行了
```
最后，我们来说说最后一个指令**WATCH**，这是一个很好用的指令，它可以帮我们实现类似于“乐观锁”的效果，即CAS（check and set）。<br />WATCH 本身的作用是**监视 key 是否被改动过**，而且支持同时监视多个 key，只要还没真正触发事务，WATCH 都会尽职尽责的监视，一旦发现某个 key 被修改了，在执行 EXEC 时就会返回 nil，表示事务无法触发。
```
127.0.0.1:6379> set age 23
OK
127.0.0.1:6379> watch age //开始监视age
OK
127.0.0.1:6379> set age 24 //在EXEC之前，age的值被修改了
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set age 25
QUEUED
127.0.0.1:6379> get age
QUEUED
127.0.0.1:6379> exec //触发EXEC
(nil) //事务无法被执行
```
<a name="F2tGy"></a>
## [*](https://redis.com.cn/redis-transaction.html#redis-%E4%BA%8B%E5%8A%A1%E5%91%BD%E4%BB%A4)Redis 事务命令
下表列出了 Redis 事务的相关命令

| 命令 | 描述 |
| --- | --- |
| [DISCARD](https://redis.com.cn/commands/discard.html) | 取消事务，放弃执行事务块内的所有命令 |
| [EXEC](https://redis.com.cn/commands/exec.html) | 执行所有事务块内的命令 |
| [MULTI](https://redis.com.cn/commands/multi.html) | 标记一个事务块的开始 |
| [UNWATCH](https://redis.com.cn/commands/unwatch.html) | 取消 WATCH 命令对所有 key 的监视 |
| [WATCH](https://redis.com.cn/commands/watch.html) | 监视一个(或多个) key |

<a name="DveGG"></a>
# Redis脚本
<br /> Redis 脚本使用 Lua 解释器来执行脚本。<br />自版本 2.6.0 开始内嵌于 Redis 中。<br />用于编写脚本的命令是 EVAL。<br />**句法**
```
redis 127.0.0.1:6379> EVAL script numkeys key [key ...] arg [arg ...]
```
**例**<br />让我们举一个例子来看看 Redis 脚本的工作原理：
```
redis 127.0.0.1:6379> EVAL "return " 2 key1 key2 first second    
1) "key1"   
2) "key2"   
3) "first"   
4) "second"
```
<a name="nNTz4"></a>
# Redis连接
<br /> Redis 连接命令用于控制和管理到 Redis Server 的客户端连接。
<a name="U8mF1"></a>
### 例
以下示例说明客户端如何向 Redis 服务器验证自身并检查服务器是否正在运行。
```
redis 127.0.0.1:6379> AUTH "password"  
(error) ERR Client sent AUTH, but no password is set  
redis 127.0.0.1:6379>  
redis 127.0.0.1:6379> PING  
PONG  
redis 127.0.0.1:6379>
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681787258271-a312b26c-57d0-4799-92f8-cc0a3821e209.png#averageHue=%23333231&clientId=u4f3bb963-3ce3-4&from=paste&id=ua6ad0d18&originHeight=182&originWidth=638&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=11327&status=done&style=none&taskId=u69664238-1e7d-4def-8ace-d387e55b586&title=)
<a name="ZCHJv"></a>
#### [*](https://redis.com.cn/redis-connection.html#%E6%B3%A8%E6%84%8F%E5%9C%A8%E8%BF%99%E9%87%8C%E6%82%A8%E5%8F%AF%E4%BB%A5%E7%9C%8B%E5%88%B0%E6%9C%AA%E8%AE%BE%E7%BD%AE%E5%AF%86%E7%A0%81%E5%9B%A0%E6%AD%A4%E6%82%A8%E5%8F%AF%E4%BB%A5%E7%9B%B4%E6%8E%A5%E8%AE%BF%E9%97%AE%E4%BB%BB%E4%BD%95%E5%91%BD%E4%BB%A4)注意：在这里您可以看到未设置“密码”，因此您可以直接访问任何命令。

---

<a name="RkCyg"></a>
## Redis 连接命令
下表列出了用于 Redis 连接相关的命令

| 命令 | 描述 |
| --- | --- |
| [AUTH password](https://redis.com.cn/commands/auth.html) | 验证密码是否正确 |
| [ECHO message](https://redis.com.cn/commands/echo.html) | 打印字符串 |
| [PING](https://redis.com.cn/commands/ping.html) | 查看服务是否运行 |
| [QUIT](https://redis.com.cn/commands/quit.html) | 关闭当前连接 |
| [SELECT index](https://redis.com.cn/commands/select.html) | 切换到指定的数据库 |

<a name="GuE4r"></a>
# Redis 服务器
<br /> Redis Server 命令用于管理 Redis 服务器。<br />有不同的服务器命令可用于获取服务器信息，统计信息和其他特征。
<a name="BnL5I"></a>
## 例
我们举一个例子来看看如何获取有关服务器的所有统计信息和信息。
```
redis 127.0.0.1:6379> ping  
PONG  
redis 127.0.0.1:6379> AUTH "password"  
(error) ERR Client sent AUTH, but no password is set  
redis 127.0.0.1:6379> PING  
PONG  
redis 127.0.0.1:6379> ECHO "Welcome to rediscomcn"  
"Welcome to rediscomcn"  
redis 127.0.0.1:6379> INFO  
redis_version:2.4.6  
redis_git_sha1:26cdd13a  
redis_git_dirty:0  
arch_bits:64  
multiplexing_api:winsock2  
gcc_version:4.6.1  
process_id:6360  
uptime_in_seconds:4442  
uptime_in_days:0  
lru_clock:1716856  
used_cpu_sys:1.80  
used_cpu_user:0.42  
used_cpu_sys_children:0.00  
used_cpu_user_children:0.00  
connected_clients:1  
connected_slaves:0  
client_longest_output_list:0  
client_biggest_input_buf:0  
blocked_clients:0  
used_memory:1188152  
used_memory_human:1.13M  
used_memory_rss:1188152  
used_memory_peak:1188112  
used_memory_peak_human:1.13M  
mem_fragmentation_ratio:1.00  
mem_allocator:libc  
loading:0  
aof_enabled:0  
changes_since_last_save:0  
bgsave_in_progress:0  
last_save_time:1506142039  
bgrewriteaof_in_progress:0  
total_connections_received:1  
total_commands_processed:4  
expired_keys:0  
evicted_keys:0  
keyspace_hits:0  
keyspace_misses:0  
pubsub_channels:0  
pubsub_patterns:0  
latest_fork_usec:0  
vm_enabled:0  
role:master
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681787284013-7cb71d81-16ca-4215-a1bf-7e7f8af8e40b.png#averageHue=%23141312&clientId=u4f3bb963-3ce3-4&from=paste&id=u0d5022db&originHeight=712&originWidth=624&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=190942&status=done&style=none&taskId=u46407f92-4bcd-4f3e-af14-b2a15468965&title=)

---

<a name="Bla53"></a>
## Redis 管理 redis 服务相关命令
下表列出了管理 redis 服务相关的命令

| 命令 | 描述 |
| --- | --- |
| [BGREWRITEAOF](https://redis.com.cn/commands/bgrewriteaof.html) | 异步执行一个 AOF（AppendOnly File） 文件重写操作 |
| [BGSAVE](https://redis.com.cn/commands/bgsave.html) | 在后台异步保存当前数据库的数据到磁盘 |
| [CLIENT](https://redis.com.cn/commands/client-kill.html) | 关闭客户端连接 |
| [CLIENT LIST](https://redis.com.cn/commands/client-list.html) | 获取连接到服务器的客户端连接列表 |
| [CLIENT GETNAME](https://redis.com.cn/commands/client-getname.html) | 获取连接的名称 |
| [CLIENT PAUSE](https://redis.com.cn/commands/client-pause.html) | 在指定时间内终止运行来自客户端的命令 |
| [CLIENT SETNAME](https://redis.com.cn/commands/client-setname.html) | 设置当前连接的名称 |
| [CLUSTER SLOTS](https://redis.com.cn/commands/cluster-slots.html) | 获取集群节点的映射数组 |
| [COMMAND](https://redis.com.cn/commands/command.html) | 获取 Redis 命令详情数组 |
| [COMMAND COUNT](https://redis.com.cn/commands/command-count.html) | 获取 Redis 命令总数 |
| [COMMAND GETKEYS](https://redis.com.cn/commands/command-getkeys.html) | 获取给定命令的所有键 |
| [TIME](https://redis.com.cn/commands/time.html) | 返回当前服务器时间 |
| [COMMAND INFO](https://redis.com.cn/commands/command-info.html) | 获取指定 Redis 命令描述的数组 |
| [CONFIG GET](https://redis.com.cn/commands/config-get.html) | 获取指定配置参数的值 |
| [CONFIG REWRITE](https://redis.com.cn/commands/config-rewrite.html) | 修改 redis.conf 配置文件 |
| [CONFIG SET](https://redis.com.cn/commands/config-set.html) | 修改 redis 配置参数，无需重启 |
| [CONFIG RESETSTAT](https://redis.com.cn/commands/config-resetstat.html) | 重置 INFO 命令中的某些统计数据 |
| [DBSIZE](https://redis.com.cn/commands/dbsize.html) | 返回当前数据库的 key 的数量 |
| [DEBUG OBJECT](https://redis.com.cn/commands/debug-object.html) | 获取 key 的调试信息 |
| [DEBUG SEGFAULT](https://redis.com.cn/commands/debug-segfault.html) | 让 Redis 服务崩溃 |
| [FLUSHALL](https://redis.com.cn/commands/flushall.html) | 删除所有数据库的所有 key |
| [FLUSHDB](https://redis.com.cn/commands/flushdb.html) | 删除当前数据库的所有 key |
| [INFO](https://redis.com.cn/commands/info.html) | 获取 Redis 服务器的各种信息和统计数值 |
| [LASTSAVE](https://redis.com.cn/commands/lastsave.html) | 返回最近一次 Redis 成功将数据保存到磁盘上的时间 |
| [MONITOR](https://redis.com.cn/commands/monitor.html) | 实时打印出 Redis 服务器接收到的命令，调试用 |
| [ROLE](https://redis.com.cn/commands/role.html) | 返回主从实例所属的角色 |
| [SAVE](https://redis.com.cn/commands/save.html) | 异步保存数据到硬盘 |
| [SHUTDOWN](https://redis.com.cn/commands/shutdown.html) | 异步保存数据到硬盘，并关闭服务器 |
| [SLAVEOF](https://redis.com.cn/commands/slaveof.html) | 将当前服务器转变从属服务器(slave server) |
| [SLOWLOG](https://redis.com.cn/commands/showlog.html) | 管理 redis 的慢日志 |
| [SYNC](https://redis.com.cn/commands/sync.html) | 用于复制功能 ( replication ) 的内部命令 |

<br /> 

 <br /> <br /> 
