<a name="W4JW9"></a>
# redis和memcached比较
| 项目 | Memcached | Redis |
| --- | --- | --- |
| 定义 | Memcached 是内存中的键值存储，最初用于缓存目的。 | Redis 是内存中的数据结构存储，用作数据库，缓存和消息代理。 |
| 描述 | MemcacheD 很简单，设计简单但功能强大。其简单的设计促进了快速部署，易于夸大，并解决了与大数据缓存相关的许多问题。它有内置的 API，提供分布在多台机器上的非常大的哈希表，并使用内部内存管理来提供更高的效率。MemcacheD 仅支持 String 数据类型，它是存储只读数据的理想选择。Memcached 是一个易失性的内存中键值来源。它是多线程的，主要用于缓存对象。 | Redis 是一个开源的内存数据结构存储，也可以用作数据库和缓存。它支持几乎所有类型的数据结构，如字符串，散列，列表，集合，带有范围查询的排序集，位图，超级日志和通过半径查询的地理空间索引。Redis 还可用于用作 pub / sub 的消息传递系统。 |
| 主数据库模型 | Memcached 遵循键值存储数据库模型。 | Redis 还遵循键值存储数据库模型。 |
| 开发者 | Memcached 由 Danga Interactive 开发。 | Redis 由 Salvatore Sanfilippo 开发。 |
| 初始发行 | Memcached 最初于 2003 年发布。 | Redis 最初于 2009 年发布。 |
| 当前版本 | Memcached 当前版本是 1.5.1，于 2017 年 8 月发布。 | Redis 当前版本是 4.0.2，于 2017 年 9 月发布。 |
| 许可 | Memcached 是免费和开源的。 | Redis 也是免费和开源的。 |
| 基于云 | 没有 | 没有 |
| 实现语言 | Memcached 是用 C 语言实现的。 | Redis 也用 C 语言实现。 |
| 支持的操作系统 | FreeBSD Linux OS X Unix Windows | BSD Linux OS X Windows |
| 支持的编程语言 | .Net，C，C ++，ColdFusion，Erlang，Java，Lisp，Lua，OCaml，Perl，PHP，Python，Ruby | C，C＃，C ++，Clojure，Crystal，D，Dart，Elixir，Erlang，Fancy，Go，Haskell，Haxe，Java，JavaScript（Node.js），Lisp，Lua，MatLab，Objective-C，OCaml info，Perl， PHP，Prolog，Python，R，Rebol，Ruby，Rust，Scala，Scheme，Smalltalk，Tcl |
| 服务器端脚本 | 没有 | LUA |
| 触发器 | 没有 | 没有 |
| 分区方法 | 没有 | 拆分 |
| 复制方法 | 没有 | 主从复制 |
| MapReduce 的 | 没有 | 没有 |
| 外键 | 没有 | 没有 |
| 交易概念 | 没有 | 乐观锁定，命令块和脚本的原子执行 |
| 并发 | 是 | 是 |
| 耐久力 | 没有 | 是 |
| 用户权限 | 是 | 简单的基于密码的访问控制 |
| 安装 | Memcached 安装和运行有点复杂。 | 安装 Redis 非常简单。不需要依赖项。 |
| 内存使用情况 | MemcacheD 比 Redis 更节省内存，因为它消耗的元数据内存资源相对较少。 | 只有在使用 Redis 哈希后，Redis 才能提高内存效率。 |
| 持久化 | Memcached 不使用持久数据。使用 Memcached 时，数据可能会在重新启动时丢失，重建缓存是一个代价高昂的过程。 | Redis 可以处理持久数据。默认情况下，它至少每 2 秒将数据同步到磁盘，提供可选的可调数据持久性，用于在计划关闭或意外故障后引导缓存。虽然我们倾向于将缓存中的数据视为易失性和瞬态，但将持久化数据保存到磁盘在缓存方案中非常有价值。 |
| 复制 | Memcached 不支持复制。 | Redis 支持主从复制。 |
| 存储类型 | MemcacheD 将变量存储在其内存中，并直接从服务器内存中检索任何信息，而不是再次访问数据库。 | Redis 就像一个驻留在内存中的数据库。它从其数据库执行（读取和写入）键/值对以返回结果集。这就是 develepors 用于实时指标和分析的原因。 |
| 执行速度和性能 | MemcacheD 非常适合处理高流量网站。它可以一次读取许多信息，并在很长的响应时间内返回。 | Redis 既不能处理读取时的高流量，也不能处理繁重的写入。 |
| 数据结构 | MemcacheD 在其数据结构中仅使用字符串和整数。因此，您保存的所有内容都可以是字符串或整数。它很复杂，因为对于整数，您可以做的唯一数据操作是添加或减去它们。如果需要保存数组或对象，则必须先将它们序列化然后保存。要阅读它们，您需要取消序列化。 | Redis 具有更强大的数据结构，它不仅可以处理字符串整数，还可以处理二进制安全字符串，二进制安全字符串列表，二进制安全字符串集和有序集。 |
| key 长度 | Memcached 的 key 长度最多为 250 个字节。 | Redis 的 key 长度最大为 2GB。 |

<a name="T8jvg"></a>
# redis 和 mongodb 比较
| 项目 | Redis | MongoDB |
| --- | --- | --- |
| 介绍 | Redis 是内存中的数据结构存储，用作数据库，缓存和消息代理。 | MongoDB 是遵循文档存储结构的最流行的 NoSQL 数据库之一。 |
| 主数据库模型 | Redis 遵循键值存储模型。 | MongoDB 遵循文档存储模型。 |
| 官方网站 | redis.io | www.mongodb.com |
| 技术文档 | 您可以在 redis.io/documentation 上获取 Redis 的技术文档 | 您可以在 docs.mongodb.com/manual 上获取 MongoDB 的技术文档 |
| 由开发 | Redis 由 Salvatore Sanfilippo 开发。 | MongoDB 由 MongoDB Inc.开发。 |
| 初始发行 | Redis 最初于 2009 年发布。 | MongoDB 最初也于 2009 年发布。 |
| 许可 | Redis 是基于订阅和开源的。 | MongoDB 可以免费使用和开源。 |
| 基于云 | 没有 | 没有 |
| 实现语言 | Redis 是用 C 语言编写和实现的。 | MongoDB 是用 C ++语言编写和实现的。 |
| 服务器操作系统 | BSD，Linux，OS X，Windows | Linux，OS X，Solaris，Windows |
| 数据 Scheme | 无 | 无 |
| 二级索引 | 没有 | 是 |
| SQL 支持 | 没有 | 没有 |
| API 和其他访问方法 | Redis 遵循专有协议。 | MongoDB 遵循使用 JSON 的专有协议。 |
| 支持的编程语言 | C，C＃，C ++，Clojure，Crystal，D，Dart，Elixir，Erlang，Fancy，Go，Haskell，Haxe，Java，JavaScript（Node.js），Lisp，Lua，MatLab，Objective-C，OCaml，Perl，PHP ，Prolog，纯数据，Python，R，Rebol，Ruby，Rust，Scala，Scheme，Smalltalk，Tcl | Actionscript，C，C＃，C ++，Clojure，ColdFusion，D，Dart，Delphi，Erlang，Go，Groovy，Haskell，Java，JavaScript，Lisp，Lua，MatLab Perl，PHP，PowerShell，Prolog，Python，R，Ruby，Scala ， 短暂聊天 |
| 服务器端脚本 | LUA | JavaScript 的 |
| 触发器 | 没有 | 没有 |
| 分区方法 | Redis 使用 Sharding 进行分区。 | MongoDB 也使用 Sharding 进行分区。 |
| 复制方法 | Redis 遵循主从复制。 | MongoDB 也遵循主从复制。 |
| MapReduce | 没有 | 是 |
| 一致性概念 | 最终的一致性和即时一致性 | 最终的一致性 |
| 外键 | 没有 | 没有 |
| 交易概念 | 乐观锁定，命令块和脚本的原子执行。 | 没有 |
| 并发 | 是 | 是 |
| 持久化 | 是 | 是 |
| 在内存中能力 | 是 | 是 |
| 用户权限 | 简单的基于密码的访问控制。 | 用户和角色的访问权限。 |
| 特色 | Redis 被评为世界上最快的数据库。它降低了应用程序的复杂性，简化了开发，加快了产品上市时间，并通过其有远见的数据结构和模块为开发人员提供了前所未有的灵 | MongoDB 被认为是下一代数据库。它成功地帮助许多企业通过提供大数据来改变他们的行业。世界上最先进的组织，从最前沿的创业公司到最大的公司，使用 MongoDB 以极低的成本创建前所未有的应用程序。 |
| 比较优势 | Redis 是一种内存数据库平台，支持各种数据结构，如字符串，散列，集合，列表，有序集，位图，超级日志和地理空间索引。Redis 通过监督分片，重新分片，迁移的所有操作，以完全自动化的方式提供轻松扩展。它还包括持久性，即时自动故障检测，备份和恢复以及跨机架，区域，数据中心，区域和云平台的内存复制。 | MongoDB 提供了当今最好的传统数据库以及当今应用程序所需的灵活性，扩展性和性能。MongoDB 是一个巨大的想法数据库。MongoDB 保留了 Relational 数据库最有价值的特性，即强一致性，表达式查询语言和二级索引。它有助于开发人员比 NoSQL 数据库更快地构建功能强大的应用程序 |
| 主要客户 | Redis 的主要客户有：Verizon，Vodafone，Atlassian，Trip Advisor，Jet.com，诺基亚，三星，HTC，Docker，Staples，Intuit，Groupon，Shutterfly，KPMG，TD Bank，UnitedHealthcare，RingCentral，The Motley Fool，Bleacher Report ，HipChat，Salesforce，Hotel Tonight，Cirruspath，Itslearning.com，Xignite，Chargify，Rumble Entertainment，Scopely，Havas Digital，Revmob，MSN，Bleacher Report，Mobli，TMZ，Klarna，Shopify 等。 | MongoDB 的主要客户有：ADP，Adobe，阿斯利康，BBVA，博世，思科，欧洲核子研究中心，退伍军人事务部，eBay，eHarmony，电子艺界，Expedia，Facebook，Parse，福布斯，Foursquare，Genentech，MetLife，Pearson，Sage ，Salesforce，天气频道，Ticketmaster，Under Armour，Verizon Wireless 等。 |
| 市场指标 | Redis Labs 由全球 60000 多家客户组成，在 NoSQL，内存和运营数据库方面的顶级分析报告中一直名列前茅。Redis 被评为否。1 个云数据库，Docker 中的 1 号数据库，1 号 NoSQL 数据存储区，容器中最流行的 NoSQL 数据库。 | 下载量达到 2000 万（每天下载量达到数千次）。超过 2,000 名客户，包括超过三分之一的财富 100 强。在 Forrester Wave 中被评为领导者：大数据 NoSQL，2016 年第 3 季度。在数据库引擎排名中排名最高的非关系型数据库 |

<a name="p7Ulq"></a>
# redis 和 Elasticsearch 比较
<br /> 

| 项目 | Redis | Elasticsearch |
| --- | --- | --- |
| 介绍 | Redis 是内存中的数据结构存储，用作数据库，缓存和消息代理 | Elasticsearch 是一个基于 Apache Lucene 的现代搜索和分析引擎 |
| 主数据库模型 | 键值存储 | 搜索引擎 |
| DB-Engines 排名 | 得分 120.41 总排名第 9，key－value 存储排名第 7 | 得分 120.00 总排名第 10，搜索引擎排名第 1 |
| 网站 | redis.io | www.elastic.co/cn/elasticsearch |
| 技术文档 | redis.io/documentation | www.elastic.co/cn/elasticsearch/features |
| 开发者 | Salvatore Sanfilippo | Elastic |
| 创建时间 | 2009 | 2010 |
| 当前版本 | 6.0.8，2020 年 11 月 | 7.6.1，2020 年 3 月 |
| 许可证信息 | 开源 | 开源 |
| 基于云的信息 | 没有 | 没有 |
| 实现语言 | C | Java |
| 支持的操作系统 | BSD Linux OS X Windows | 所有带有 Java VM 的操作系统 |
| 数据 scheme | 无 scheme | 无 scheme |
| XML 支持 |  | 没有 |
| 二级索引 | 没有 | 是 |
| SQL | 没有 | 没有 |
| API 和其他访问方法 | 专有协议 | Java API RESTful HTTP / JSON API |
| 支持的编程语言 | C C＃C ++ Clojure Crystal D Dart Elixir Erlang Fancy Go Haskell Haxe Java JavaScript（Node.js）Lisp Lua MatLab Objective-C OCaml Perl PHP Prolog Python R Rebol Ruby Rust Scala Smalltalk Tcl | .Net Clojure Erlang Go Groovy Haskell Java JavaScript Lua Perl PHP Python Ruby Scala |
| 服务器端脚本 | Lua | 是 |
| 触发器 | 没有 | 是 |
| 分区方法 | 拆分 | 拆分 |
| 复制方法 | 主从复制 | 是 |
| MapReduce | 没有 | 没有 |
| 一致性概念 | 最终的一致性 | 最终的一致性 |
| 外键 | 没有 | 没有 |

如果对数据的读写要求极高，选redis；如果需要构造一个搜索引擎或者数据有一定的分析价值想搞高大上的数据可视化平台，选ElasticSearch。

