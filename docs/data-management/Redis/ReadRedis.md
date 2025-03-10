![](https://img.starfish.ink/redis/redis-readme-banner.jpg)



> 作为一名后端开发工程师，工作中肯定会用到 Redis，面试中八成也会被问到 Redis 相关的问题。所以系统学习，深入学习还是很有必要的。
>
> 建立属于自己的完整的 Reids 知识框架。
>
> 带着问题去系统学习，有一个自己的问题画像，最后梳理成自己的“武功秘籍”
>
> 看下极客时间中的一个 Redis 问题画像图：
>
> ![img](https://static001.geekbang.org/resource/image/70/b4/70a5bc1ddc9e3579a2fcb8a5d44118b4.jpeg)



## Redis 简介

Redis: **REmote DIctionary Server**(远程字典服务器)。

Redis 是一个全开源免费（BSD许可）的，内存中的数据结构存储系统，它可以用作**数据库、缓存和消息中间件**。一般作为一个高性能的(key/value)分布式内存数据库，基于**内存**运行并支持持久化的 NoSQL 数据库，是当前最热门的 NoSql 数据库之一，也被人们称为**数据结构服务器**



## Redis 介绍

Redis 是一个开源的、使用 C 语言编写的、支持网络交互的、可基于内存也可持久化的 Key-Value 数据库。

Redis 是一个 key-value 存储系统。和 Memcached 类似，它支持存储的 value 类型相对更多，包括**string(字符串)、list(链表)、set(集合)、zset(sorted set --有序集合)和 hash（哈希类型）**。这些数据类型都支持 push/pop、add/remove 及取交集并集和差集及更丰富的操作，而且这些操作都是原子性的。在此基础上，redis 支持各种不同方式的排序。与 memcached 一样，为了保证效率，数据都是缓存在内存中。区别的是 redis 会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现了 master-slave(主从)同步，所以 Redis 也可以被看成是一个数据结构服务器。



Redis 支持主从同步。数据可以从主服务器向任意数量的从服务器上同步，从服务器可以是关联其他从服务器的主服务器。这使得 Redis 可执行单层树复制。存盘可以有意无意的对数据进行写操作。由于完全实现了发布/订阅机制，使得从数据库在任何地方同步树时，可订阅一个频道并接收主服务器完整的消息发布记录。同步对读取操作的可扩展性和数据冗余很有帮助。

Redis 的官网地址，非常好记，是 redis.io。（域名后缀io属于国家域名，是 british Indian Ocean territory，即英属印度洋领地）目前，Vmware 在资助着 Redis 项目的开发和维护。

Redis 的所有数据都是保存在内存中，然后不定期的通过异步方式保存到磁盘上(这称为“半持久化模式”)；也可以把每一次数据变化都写入到一个 append only file(aof)里面(这称为“全持久化模式”)。这就是 redis 提供的两种持久化的方式，RDB（Redis DataBase）和 AOF（Append Only File）。



## Redis 特点

Redis 是一个开源，先进的 key-value 存储，并用于构建高性能，可扩展的 Web 应用程序的完美解决方案。

Redis 从它的许多竞争继承来的三个主要特点：

- Redis数据库完全在内存中，使用磁盘仅用于持久性。

- 相比许多键值数据存储，Redis 拥有一套较为丰富的数据类型。

- Redis可以将数据复制到任意数量的从服务器。

  

## Redis 优势

- 异常快速：Redis 的速度非常快，每秒能执行约 11 万集合，每秒约 81000+ 条记录。SET 操作每秒钟 110000 次，GET 操作每秒钟 81000 次，网站一般使用 Redis 作为**缓存服务器**。

- 支持**丰富的数据类型**：Redis 支持大多数开发人员已经知道像列表，集合，有序集合，散列数据类型。这使得它非常容易解决各种各样的问题，因为我们知道哪些问题是可以处理通过它的数据类型更好。

- 操作都是**原子性**：所有 Redis 操作是原子的，这保证了如果两个客户端同时访问的 Redis 服务器将获得更新后的值。

- MultiUtility 工具：Redis 是一个多功能实用工具，可以在很多如：缓存，消息传递队列中使用（Redis 原生支持发布/订阅），在应用程序中，如：Web应用程序会话，网站页面点击数等任何短暂的数据；

  

#### Redis 使用场景

- 取最新 N 个数据的操作
- 排行榜应用,取 TOP N 操作
- 需要精确设定过期时间的应用
- 定时器、计数器应用
- Uniq 操作,获取某段时间所有数据排重值
- 实时系统,反垃圾系统
- Pub/Sub 构建实时消息系统
- 构建队列系统
- 缓存



**具体以某一论坛为例：**

- 记录帖子的点赞数、评论数和点击数 (hash)。
-  记录用户的帖子 ID 列表 (排序)，便于快速显示用户的帖子列表 (zset)。 
- 记录帖子的标题、摘要、作者和封面信息，用于列表页展示 (hash)。 
- 记录帖子的点赞用户 ID 列表，评论 ID 列表，用于显示和去重计数 (zset)。 
- 缓存近期热帖内容 (帖子内容空间占用比较大)，减少数据库压力 (hash)。 
- 记录帖子的相关文章 ID，根据内容推荐相关帖子 (list)。 
- 如果帖子 ID 是整数自增的，可以使用 Redis 来分配帖子 ID(计数器)。 
- 收藏集和帖子之间的关系 (zset)。 
- 记录热榜帖子 ID 列表，总热榜和分类热榜 (zset)。 
- 缓存用户行为历史，进行恶意行为过滤 (zset,hash)。



**安装**

```
$ wget http://download.redis.io/releases/redis-5.0.6.tar.gz
$ tar xzf redis-5.0.6.tar.gz
$ cd redis-5.0.6
$ make
```

新版本的编译文件在src中（之前在bin目录），启动server

```
$ src/redis-server
```

启动客户端

```
$ src/redis-cli
redis> set foo bar
OK
redis> get foo
"bar"
```



## Redis 知识全景

![](https://static001.geekbang.org/resource/image/79/e7/79da7093ed998a99d9abe91e610b74e7.jpg)

“两大维度”就是指系统维度和应用维度，“三大主线”也就是指高性能、高可靠和高可扩展（可以简称为“三高”）。

Redis 作为庞大的键值数据库，可以说遍地都是知识，一抓一大把，我们怎么能快速地知道该学哪些呢？别急，接下来就要看“三大主线”的魔力了。

别看技术点是零碎的，其实你完全可以按照这三大主线，给它们分下类，就像图片中展示的那样，具体如下：

**高性能主线**，包括线程模型、数据结构、持久化、网络框架；

**高可靠主线**，包括主从复制、哨兵机制；

**高可扩展主线**，包括数据分片、负载均衡。



## 推荐阅读

[《我是如何学习Redis的？高效学习Redis的路径和方法分享》](http://kaito-kidd.com/2020/09/09/how-i-learned-redis/)