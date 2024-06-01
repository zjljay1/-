<a name="IGjvf"></a>
# redis备份和恢复
SAVE 命令用于创建当前 Redis 数据库的备份。此命令将通过执行同步 SAVE 在 Redis 目录中创建 dump.rdb 文件。<br />**语法**<br />SAVE<br />**返回值**<br />执行成功后，SAVE 命令返回 OK。

---

<a name="QLGNr"></a>
## Redis备份示例
使用 SAVE 命令创建当前数据库的备份。<br />SAVE<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681787469155-7e261a16-3fe0-404f-a007-f7b3df35fc54.png#averageHue=%23300a24&clientId=ud0fbdd2f-be3a-4&from=paste&id=u71a827c5&originHeight=217&originWidth=732&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=26619&status=done&style=none&taskId=ud58acd61-5c56-4278-8c6a-9034dbe4215&title=)<br />它将在 Redis 目录中创建 dump.rdb 文件。<br />可以看到 dump.rdb 文件已创建。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681787469233-f6c0ee2c-01d8-4e61-9cda-04533a131d1a.png#averageHue=%23586c9b&clientId=ud0fbdd2f-be3a-4&from=paste&id=ueabad05b&originHeight=244&originWidth=732&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=43821&status=done&style=none&taskId=u91a6bd82-a8ee-49b9-b825-b6160859b52&title=)
<a name="lxhQC"></a>
## 还原Redis数据
将 Redis 备份文件（dump.rdb）移动到 Redis 目录中并启动服务器以恢复 Redis 数据。<br />查找 Redis 的安装目录，使用 Redis 的 CONFIG 命令，如下所示。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681787469250-5fd4a1db-3f21-48ab-91e2-603dd78e8370.png#averageHue=%235a6893&clientId=ud0fbdd2f-be3a-4&from=paste&id=u8c4a5617&originHeight=227&originWidth=732&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=49575&status=done&style=none&taskId=u2ca3a416-6288-4571-97c1-7cb3b9e9988&title=)<br />Redis 服务器安装在以下目录中。<br />“/var/lib/redis”

---

<a name="MLkNX"></a>
## BGSAVE命令
BGSAVE 是创建 Redis 备份的备用命令。<br />此命令将启动备份过程并在后台运行。<br />**语法**<br />BGSAVE<br />**例**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681787469067-7eae39db-a634-44d5-b5c9-022f23bd573a.png#averageHue=%236e82a6&clientId=ud0fbdd2f-be3a-4&from=paste&id=u78b00229&originHeight=176&originWidth=732&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=44783&status=done&style=none&taskId=u99ad194b-ad03-4c61-a8e3-30136a1cf99&title=)<br /> 
<a name="Euh9s"></a>
# Redis 持久化
<br /> redis 提供了两种持久化的方式，分别是**RDB**（Redis DataBase）和**AOF**（Append Only File）。<br />RDB，简而言之，就是在不同的时间点，将 redis 存储的数据生成快照并存储到磁盘等介质上；<br />AOF，则是换了一个角度来实现持久化，那就是将 redis 执行过的所有写指令记录下来，在下次 redis 重新启动时，只要把这些写指令从前到后再重复执行一遍，就可以实现数据恢复了。<br />其实 RDB 和 AOF 两种方式也可以同时使用，在这种情况下，如果 redis 重启的话，则会优先采用 AOF 方式来进行数据恢复，这是因为 AOF 方式的数据恢复完整度更高。<br />如果你没有数据持久化的需求，也完全可以关闭 RDB 和 AOF 方式，这样的话，redis 将变成一个纯内存数据库，就像 memcache 一样。
<a name="jXmQc"></a>
## redis持久化RDB
RDB 方式，是将 redis 某一时刻的数据持久化到磁盘中，是一种快照式的持久化方法。<br />redis 在进行数据持久化的过程中，会先将数据写入到一个临时文件中，待持久化过程都结束了，才会用这个临时文件替换上次持久化好的文件。正是这种特性，让我们可以随时来进行备份，因为快照文件总是完整可用的。<br />对于 RDB 方式，redis 会单独创建（fork）一个子进程来进行持久化，而主进程是不会进行任何 IO 操作的，这样就确保了 redis 极高的性能。<br />如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那 RDB 方式要比 AOF 方式更加的高效。<br />虽然 RDB 有不少优点，但它的缺点也是不容忽视的。如果你对数据的完整性非常敏感，那么 RDB 方式就不太适合你，因为即使你每 5 分钟都持久化一次，当 redis 故障时，仍然会有近 5 分钟的数据丢失。所以，redis 还提供了另一种持久化方式，那就是 AOF。
<a name="f3XWS"></a>
## redis持久化  AOF
AOF，英文是 Append Only File，即只允许追加不允许改写的文件。<br />如前面介绍的，AOF 方式是将执行过的写指令记录下来，在数据恢复时按照从前到后的顺序再将指令都执行一遍，就这么简单。<br />我们通过配置 redis.conf 中的 appendonly yes 就可以打开 AOF 功能。如果有写操作（如 SET 等），redis 就会被追加到 AOF 文件的末尾。<br />默认的 AOF 持久化策略是每秒钟 fsync 一次（fsync 是指把缓存中的写指令记录到磁盘中），因为在这种情况下，redis 仍然可以保持很好的处理性能，即使 redis 故障，也只会丢失最近 1 秒钟的数据。<br />如果在追加日志时，恰好遇到磁盘空间满、inode 满或断电等情况导致日志写入不完整，也没有关系，redis 提供了 redis-check-aof 工具，可以用来进行日志修复。<br />因为采用了追加方式，如果不做任何处理的话，AOF 文件会变得越来越大，为此，redis 提供了 AOF 文件重写（rewrite）机制，即当 AOF 文件的大小超过所设定的阈值时，redis 就会启动 AOF 文件的内容压缩，只保留可以恢复数据的最小指令集。举个例子或许更形象，假如我们调用了 100 次 INCR 指令，在 AOF 文件中就要存储 100 条指令，但这明显是很低效的，完全可以把这 100 条指令合并成一条 SET 指令，这就是重写机制的原理。<br />在进行 AOF 重写时，仍然是采用先写临时文件，全部完成后再替换的流程，所以断电、磁盘满等问题都不会影响 AOF 文件的可用性，这点大家可以放心。<br />AOF 方式的另一个好处，我们通过一个“场景再现”来说明。某同学在操作 redis 时，不小心执行了 FLUSHALL，导致 redis 内存中的数据全部被清空了，这是很悲剧的事情。不过这也不是世界末日，只要 redis 配置了 AOF 持久化方式，且 AOF 文件还没有被重写（rewrite），我们就可以用最快的速度暂停 redis 并编辑 AOF 文件，将最后一行的 FLUSHALL 命令删除，然后重启 redis，就可以恢复 redis 的所有数据到 FLUSHALL 之前的状态了。是不是很神奇，这就是 AOF 持久化方式的好处之一。但是如果 AOF 文件已经被重写了，那就无法通过这种方法来恢复数据了。<br />虽然优点多多，但 AOF 方式也同样存在缺陷，比如在同样数据规模的情况下，AOF 文件要比 RDB 文件的体积大。而且，AOF 方式的恢复速度也要慢于 RDB 方式。<br />如果你直接执行 BGREWRITEAOF 命令，那么 redis 会生成一个全新的 AOF 文件，其中便包括了可以恢复现有数据的最少的命令集。<br />如果运气比较差，AOF 文件出现了被写坏的情况，也不必过分担忧，redis 并不会贸然加载这个有问题的 AOF 文件，而是报错退出。这时可以通过以下步骤来修复出错的文件：<br />1.备份被写坏的 AOF 文件\ 2.运行 redis-check-aof –fix 进行修复\ 3.用 diff -u 来看下两个文件的差异，确认问题点\ 4.重启 redis，加载修复后的 AOF 文件
<a name="PzGWs"></a>
## redis持久化 – AOF重写
AOF 重写的内部运行原理，我们有必要了解一下。<br />在重写即将开始之际，redis 会创建（fork）一个“重写子进程”，这个子进程会首先读取现有的 AOF 文件，并将其包含的指令进行分析压缩并写入到一个临时文件中。<br />与此同时，主工作进程会将新接收到的写指令一边累积到内存缓冲区中，一边继续写入到原有的 AOF 文件中，这样做是保证原有的 AOF 文件的可用性，避免在重写过程中出现意外。<br />当“重写子进程”完成重写工作后，它会给父进程发一个信号，父进程收到信号后就会将内存中缓存的写指令追加到新 AOF 文件中。<br />当追加结束后，redis 就会用新 AOF 文件来代替旧 AOF 文件，之后再有新的写指令，就都会追加到新的 AOF 文件中了。
<a name="lQ3W6"></a>
## redis持久化 – 如何选择RDB和AOF
对于我们应该选择 RDB 还是 AOF，官方的建议是两个同时使用。这样可以提供更可靠的持久化方案。
<a name="w2nux"></a>
# redis安全
 对于数据库来说，安全性是非常必要的，以确保数据的安全性。它提供身份验证，因此如果客户端想要建立连接，则需要在执行命令之前进行身份验证。<br />您需要在配置文件中设置密码以保护 Redis 数据库。
<a name="tO5Uq"></a>
### 例
我们来看看如何保护您的 Redis 实例。<br />使用“config get command”<br />config get requirepass<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681787543833-a4785821-cf8d-4be7-b397-08bd8c121f39.png#averageHue=%235e6c92&clientId=ud0fbdd2f-be3a-4&from=paste&id=u0d3ecacb&originHeight=98&originWidth=732&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=23463&status=done&style=none&taskId=u2d28515c-8937-4bd6-9e54-f45dde49ce2&title=)<br />您可以看到上面的属性为空，表示我们没有此实例的任何密码。您可以通过执行以下命令来更改此属性并为此实例设置密码。<br />config set requirepass “javatpoint123”<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681787543703-3b19dea9-1ca8-4db2-a650-11c9f1cf10f2.png#averageHue=%235e6e9b&clientId=ud0fbdd2f-be3a-4&from=paste&id=u2250cfb9&originHeight=98&originWidth=732&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=22127&status=done&style=none&taskId=u1a9523fb-547c-4f0f-9322-3ead1252d35&title=)<br />CONFIG get requirepass<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681787543777-773c57b2-dd68-43d6-ac3a-ab10ec5fa293.png#averageHue=%235f709f&clientId=ud0fbdd2f-be3a-4&from=paste&id=u5a92df28&originHeight=98&originWidth=732&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=24991&status=done&style=none&taskId=u0d865a61-2cd8-4cef-9ee9-36c31d872c1&title=)<br />当您设置此密码时，如果客户端在未经身份验证的情况下运行该命令，则会收到错误“NOAUTH Authentication required。”。因此，客户端需要使用 AUTH 命令来验证自己。

---

<a name="IaL5R"></a>
## AUTH命令的用法
```
127.0.0.1:6379> AUTH "javatpoint123"  
OK  
127.0.0.1:6379> SET mykey "hindi100" 
OK  
127.0.0.1:6379> GET mykey  
"hindi100"  
127.0.0.1:6379>
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681787543793-d617963e-d82e-478d-b56b-805c7242cebd.png#averageHue=%233f3773&clientId=ud0fbdd2f-be3a-4&from=paste&id=ua7a344f0&originHeight=166&originWidth=732&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=30383&status=done&style=none&taskId=u2fb80f75-a67c-4a22-8a0f-c66c433c626&title=)
<a name="SEBof"></a>
# Redis 基准测试
<br /> Redis 基准测试 redis-benchmark 是一种实用工具，用于通过同时使用 multiple(n) 命令来检查 Redis 的性能。<br />**句法**
```
redis-benchmark [option] [option value]
```
<a name="J4kqS"></a>
### 例
调用 Redis Benchmark 命令：<br />redis-benchmark -n 100000<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681787587034-8af45b36-aaf0-4ace-93a4-d3b5dc82dff7.png#averageHue=%23310b25&clientId=ud0fbdd2f-be3a-4&from=paste&id=u74c869df&originHeight=438&originWidth=732&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=69307&status=done&style=none&taskId=u6c757d73-21f9-455d-a4bc-713d47ef1dc&title=)\<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681787587105-41e7040b-2a6b-41b6-961a-7ddb34600465.png#averageHue=%23300a24&clientId=ud0fbdd2f-be3a-4&from=paste&id=u51d1d27f&originHeight=438&originWidth=732&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=87473&status=done&style=none&taskId=ub116faa2-2c1b-45d4-b026-cfd76df7491&title=)
<a name="vEoZU"></a>
### redis 性能测试工具可选参数如下所示：
| 选项 | 描述 | 默认值 |
| --- | --- | --- |
| -h | 指定服务器主机名 | 127.0.0.1 |
| -p | 指定服务器端口 | 6379 |
| -s | 指定服务器 socket |  |
| -c | 指定并发连接数 | 50 |
| -n | 指定请求数 | 10000 |
| -d | 以字节的形式指定 SET/GET 值的数据大小 | 2 |
| -k | 1=keep alive 0=reconnect | 1 |
| -r | SET/GET/INCR 使用随机 key, SADD 使用随机值 |  |
| -P | 通过管道传输 <numreq> 请求 | 1 |
| -q | 强制退出 redis。仅显示 query/sec 值 |  |
| --csv | 以 CSV 格式输出 |  |
| -l | 生成循环，永久执行测试 |  |
| -t | 仅运行以逗号分隔的测试命令列表 |  |
| -I | Idle 模式。仅打开 N 个 idle 连接并等待 |  |

<a name="DkgDA"></a>
### 实例
以下实例我们使用了多个参数来测试 redis 性能：主机为 127.0.0.1，端口号为 6379，执行的命令为 set,lpush，请求数为 10000，通过 -q 参数让结果只显示每秒执行的请求数。
```
redis-benchmark -h 127.0.0.1 -p 6379 -t set,lpush -n 100000 -q
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681787586804-bd83b613-9b16-4a41-bac6-97adabce28f0.png#averageHue=%23453f73&clientId=ud0fbdd2f-be3a-4&from=paste&id=ue8dcc73e&originHeight=166&originWidth=732&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=36759&status=done&style=none&taskId=u2f233300-c82f-46cc-80c6-6a5f727cdb1&title=)
<a name="mPdAY"></a>
# Redis客户端连接
<br /> Redis 可以在配置的监听 TCP 端口和 Unix 套接字上接受不同类型的客户端连接。<br />接受新客户端连接时，它将执行以下操作：

- 由于 Redis 使用多路复用和非阻塞 I/O，因此客户端套接字处于非阻塞状态。
- 设置 TCP_NODELAY 选项是为了确保我们的连接没有延迟。
- 创建可读文件事件，以便一旦可以在套接字上读取新数据，Redis 就能够收集客户端查询。

---

<a name="bfqnH"></a>
## 最大客户端数
在 Redis config（redis.conf）中，有一个名为 maxclients 的属性，它指定可以连接到 Redis 的客户端数量。<br />以下是命令的基本语法。
```
Config get maxclients  
"maxclients"  
"4064"
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681787614286-2d66fd9d-9e66-4c89-9156-2ab8a39f1dfc.png#averageHue=%235d6e9c&clientId=ud0fbdd2f-be3a-4&from=paste&id=ud9de3ca4&originHeight=98&originWidth=732&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=19165&status=done&style=none&taskId=u9e10877c-3076-44a3-9182-c8126be9cc0&title=)<br />最大客户端数取决于 OS 的最大文件描述符数限制。它的默认值为 10000，但您可以更改此属性。
<a name="tCw0z"></a>
### 例
我们举一个例子，在启动服务器时将最大客户端数设置为 100000。
```
redis-server --maxclients 100000
```

---

<a name="BpH3e"></a>
## [*](https://redis.com.cn/redis-client-connection.html#redis-%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%91%BD%E4%BB%A4)Redis 客户端命令
| 命令 | 描述 |
| --- | --- |
| [CLIENT LIST](https://redis.com.cn/commands/client-list.html) | 返回连接到 redis 服务的客户端列表 |
| [CLIENT SETNAME](https://redis.com.cn/commands/client-setname.html) | 设置当前连接的名称 |
| [CLIENT GETNAME](https://redis.com.cn/commands/client-getname.html) | 获取通过 CLIENT SETNAME 命令设置的服务名称 |
| [CLIENT PAUSE](https://redis.com.cn/commands/client-pause.html) | 挂起客户端连接，指定挂起的时间以毫秒计 |
| [CLIENT KILL](https://redis.com.cn/commands/client-kill.html) | 关闭客户端连接 |

<br /> 
<a name="OJVe4"></a>
# Redis Pipelining 流水线
<br /> 在了解流水线之前，首先要了解 Redis 的概念：<br />Redis 是一个支持请求/响应协议的 TCP 服务器。在 Redis 中，请求分两步完成：

- 客户端通常以阻塞方式向服务器发送命令。
- 服务器处理该命令并将响应发送回客户端。

---

<a name="JpL40"></a>
## 什么是流水线
流水线操作有助于客户端向服务器发送多个请求，而无需等待回复，最后只需一步即可读取回复。<br />**例**<br />让我们看一下 Redis 流水线的例子。在这个例子中，我们将向 Redis 提交一次命令，Redis 将在一个步骤中提供所有命令的输出。<br />打开 Redis 终端并使用以下命令：
```
（echo -en  “PING \ r \ n SET sssit javatraining \ r \ n GET sssit \ r \ n INCR visitor \ r \ n INCR visitor \ r \ n INCR visitor \ r \ n” ; sleep  10 ）|  
 nc localhost  6379
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681787629041-3c8efdfb-714b-44f0-a847-0bd9e0dcd3f7.png#averageHue=%23300a24&clientId=ud0fbdd2f-be3a-4&from=paste&id=u645dc22a&originHeight=387&originWidth=732&originalType=url&ratio=1.375&rotation=0&showTitle=false&size=63827&status=done&style=none&taskId=uaff79282-9545-464f-b4c0-42cc15abf22&title=)<br />这里：

- PING 命令用于检查 Redis 连接。
- 设置名为“sssit”的字符串，其值为“javatraining”。
- 获得了 key 值并将访问者数量增加了三倍。

每次增加值时都可以看到。

---

<a name="VLAjx"></a>
## 流水线的优势
Redis 流水线操作的主要优点是提高了 Redis 的性能。由于多个命令同时执行，它极大地提高了协议性能。
<a name="lsyCG"></a>
## Pipelining vs Scripting
Redis Scripting 可在 Redis 2.6 或更高版本中使用。<br />脚本的主要优点是它可以以最小的延迟同时读取和写入数据。它使读取，计算，写入等操作变得非常快。<br />在流水线操作中，客户端在调用 write 命令之前需要 read 命令的回复。
<a name="xtlTd"></a>
# redis分区
<br /> 分区用于将 Redis 数据拆分为多个 Redis 实例，以便每个实例仅包含一部分 key。<br />它通常用于大型数据库。
<a name="NU20s"></a>
## 分区类型
redis 中有两种类型的分区：

- 范围分区
- 哈希分区

---

<a name="ZUcG3"></a>
## 范围分区
范围分区是执行分区的最简单方法之一。它通过将对象的范围映射到特定的 Redis 实例来完成。<br />**例如：**<br />假设您有 3000 个用户。因此，您可以说从 ID 0 到 ID 1000 的用户将进入实例 R0，而用户表单 ID 1001 到 ID 2000 将进入实例 R1，用户表单 ID 2001 到 ID 3000 将进入实例 R2，依此类推。
<a name="fmYWU"></a>
## 哈希分区
散列分区是 Range 分区的替代方法。在散列分区中，散列函数用于将 key 转换为数字，然后将数据存储在不同的 Redis 实例中。
<a name="gHOD7"></a>
## Redis分区的优点

- 分区有助于您使用多台计算机的集体内存。例如：对于较大的数据库，您需要大量内存，因此分区可以提供来自不同计算机的内存总和。如果不进行分区，则只能使用单台计算机可以支持的有限内存量。
- 分区还用于将计算能力扩展到多个核心和多个计算机，以及网络带宽扩展到多个计算机和网络适配器。
<a name="h8Es3"></a>
## Redis分区的缺点
分区存在一些缺点，因为 Redis 的某些功能受到分区的阻碍。

- 分区通常不支持具有多个键的操作。例如，如果两个集合存储在映射到不同 Redis 实例的键中，则无法执行它们之间的交集。
- 分区不支持具有多个 key 的事务。
- 分区粒度是关键，因此不可能使用单个巨大的 key（如非常大的有序集）对数据集进行分片。
- 使用分区时，数据处理更复杂，例如，您必须处理多个 RDB / AOF 文件，并且需要从多个实例和主机聚合持久性文件来备份数据。
- 添加和删除容量可能很复杂。例如，Redis Cluster 支持大多数透明的数据重新平衡，能够在运行时添加和删除节点，但客户端分区和代理等其他系统不支持此功能。然而，一种称为预分片的技术在这方面有所帮助。
<a name="qfDOt"></a>
# Java 使用 Redis
<br /> 开始在 Java 中使用 Redis 前， 我们需要确保已经安装了 redis 服务及 Java redis 驱动，且你的机器上能正常使用 Java。
<a name="t121t"></a>
## 安装
首先，安装 Redis 的 java 驱动。

- 首先你需要下载驱动包 [下载 jedis.jar](https://mvnrepository.com/artifact/redis.clients/jedis)，确保下载最新驱动包。
- 在你的 classpath 中包含该驱动包。
<a name="tMcCt"></a>
## 连接到 Redis 服务
```
import redis.clients.jedis.Jedis; 

public class RedisJava { 
   public static void main(String[] args) { 
      //Connecting to Redis server on localhost 
      Jedis jedis = new Jedis("localhost"); 
      System.out.println("Connection to server sucessfully"); 
      //check whether server is running or not 
      System.out.println("Server is running: "+jedis.ping()); 
   } 
}
```
编译并运行程序。可以根据需要修改你的路径。我们假设 **jedis.jar** 在当前路径中。
```
$javac RedisJava.java 
$java RedisJava 
Connection to server sucessfully 
Server is running: PONG
```
<a name="TLo9v"></a>
## Redis Java String 实例
```
import redis.clients.jedis.Jedis; 

public class RedisStringJava { 
   public static void main(String[] args) { 
      //Connecting to Redis server on localhost 
      Jedis jedis = new Jedis("localhost"); 
      System.out.println("Connection to server sucessfully"); 
      //set the data in redis string 
      jedis.set("tutorial-name", "Redis tutorial"); 
      // Get the stored data and print it 
      System.out.println("Stored string in redis:: "+ jedis.get("tutorial-name")); 
   } 
}
```
编译和运行上面的程序
```
$javac RedisStringJava.java 
$java RedisStringJava 
Connection to server sucessfully 
Stored string in redis:: Redis tutorial
```
<a name="emUYP"></a>
## Redis Java List 实例
```
import redis.clients.jedis.Jedis; 

public class RedisListJava { 
   public static void main(String[] args) { 

      //Connecting to Redis server on localhost 
      Jedis jedis = new Jedis("localhost"); 
      System.out.println("Connection to server sucessfully"); 

      //store data in redis list 
      jedis.lpush("tutorial-list", "Redis"); 
      jedis.lpush("tutorial-list", "Mongodb"); 
      jedis.lpush("tutorial-list", "Mysql"); 
      // Get the stored data and print it 
      List<String> list = jedis.lrange("tutorial-list", 0 ,5); 

      for(int i = 0; i<list.size(); i++) { 
         System.out.println("Stored string in redis:: "+list.get(i)); 
      } 
   } 
}
```
编译和运行程序
```
$javac RedisListJava.java 
$java RedisListJava 
Connection to server sucessfully 
Stored string in redis:: Redis 
Stored string in redis:: Mongodb 
Stored string in redis:: Mysql
```
<a name="j1Smm"></a>
## Redis Java Keys 实例
```
import redis.clients.jedis.Jedis; 

public class RedisKeyJava { 
   public static void main(String[] args) { 

      //Connecting to Redis server on localhost 
      Jedis jedis = new Jedis("localhost"); 
      System.out.println("Connection to server sucessfully"); 
      //store data in redis list 
      // Get the stored data and print it 
      List<String> list = jedis.keys("*"); 

      for(int i = 0; i<list.size(); i++) { 
         System.out.println("List of stored keys:: "+list.get(i)); 
      } 
   } 
}
```
编译和运行程序
```
$javac RedisKeyJava.java 
$java RedisKeyJava 
Connection to server sucessfully 
List of stored keys:: tutorial-name 
List of stored keys:: tutorial-list
```

