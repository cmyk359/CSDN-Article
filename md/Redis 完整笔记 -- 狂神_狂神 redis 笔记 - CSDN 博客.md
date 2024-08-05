> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_45433031/article/details/120422544)

Redis
-----

#### 一、Nosql

#### 为什么使用 Nosql

> 1、单机 Mysql 时代

![](https://i-blog.csdnimg.cn/blog_migrate/18c714b212f6d33d9eeb9348a3bc0b05.png)

90 年代, 一个网站的访问量一般不会太大，单个数据库完全够用。随着用户增多，网站出现以下问题

1.  数据量增加到一定程度，单机数据库就放不下了
2.  数据的索引（B+ Tree）, 一个机器内存也存放不下
3.  访问量变大后（读写混合），一台服务器承受不住。

> 2、Memcached(缓存) + Mysql + 垂直拆分（读写分离） 

网站 80% 的情况都是在读，每次都要去查询数据库的话就十分的麻烦！所以说我们希望减轻数据库的压力，我们可以使用缓存来保证效率！![](https://i-blog.csdnimg.cn/blog_migrate/fcc368c4e8888ddc1186e07ccf7fbe26.png)

优化过程经历了以下几个过程：

    1、优化数据库的数据结构和索引 (难度大)

    2、文件缓存，通过 IO 流获取比每次都访问数据库效率略高，但是流量爆炸式增长时候，IO 流也承受不了  
    3、MemCache, 当时最热门的技术，通过在数据库和数据库访问层之间加上一层缓存，第一次访问时查询数据库，将结果保存到缓存，后续的查询先检查缓存，若有直接拿去使用，效率显著提升。

> 3、分库分表 + 水平拆分 + Mysql 集群

![](https://i-blog.csdnimg.cn/blog_migrate/34428691fd54fc4b699ce0e97d07e91e.png)

> 4、如今最近的年代

 ​ 如今信息量井喷式增长，各种各样的数据出现（用户定位数据，图片数据等），大数据的背景下关系型数据库（RDBMS）无法满足大量数据要求。Nosql 数据库就能轻松解决这些问题。

> 目前一个基本的互联网项目

![](https://i-blog.csdnimg.cn/blog_migrate/32586b46cf3f7c987ead99f1add78c94.png)

> 为什么要用 NoSQL ？

 用户的个人信息，社交网络，地理位置。用户自己产生的数据，用户日志等等爆发式增长！  
这时候我们就需要使用 NoSQL 数据库的，Nosql 可以很好的处理以上的情况！

####   
什么是 Nosql

**Not Only SQL、** 不仅仅是数据库。泛指非关系型数据库。

关系型数据库：列 + 行，同一个表下数据的结构是一样的。

非关系型数据库：数据存储没有固定的格式，并且可以进行横向扩展。

####   
Nosql 特点

1.  数据之间没有关系，方便扩展
2.  大数据量下，性能高。（一秒写入 8W 次 读取 11W 次）
3.  数据类型是多样性（不需要事先设计数据库，感受一下 Mysql 设计表和库的痛苦）
4.  存储方式
    *   键值对存储
    *   列存储
    *   文档存储
    *   图形数据库
    *   …
5.  没有固定的查询语言
6.  CAP 定理和 BASE
7.  高可用、高性能、高可扩

推荐阅读：阿里云的这群疯子[阿里云的这群疯子 - 阿里云开发者社区](https://yq.aliyun.com/articles/653511 "阿里云的这群疯子-阿里云开发者社区")

```
# 商品信息
- 一般存放在关系型数据库：Mysql,阿里巴巴使用的Mysql都是经过内部改动的。
 
# 商品描述、评论(文字居多)
- 文档型数据库：MongoDB
 
# 图片
- 分布式文件系统 FastDFS
- 淘宝：TFS
- Google: GFS
- Hadoop: HDFS
- 阿里云: oss
 
# 商品关键字 用于搜索
- 搜索引擎：solr,elasticsearch
- 阿里：Isearch 多隆
 
# 商品热门的波段信息
- 内存数据库：Redis，Memcache
 
# 商品交易，外部支付接口
- 第三方应用
```

所以企业中使用数据库，都是关系型数据库 + Nosql 一起使用。

####   
NoSQL 的四大分类

KV 键值对

*   sina：Redis
*   美团：Redis+Tair
*   alibaba、baidu：Redis+Memcache

文档型数据库（bson 格式）

*   MongoDB(掌握)
    
    > 基于分布式文件存储的数据库。C++ 编写，用于处理大量文档。
    > 
    > [SpringBoot 整合 MongoDB（一）_spring mongodb_保护我方胖虎的博客 - CSDN 博客](https://blog.csdn.net/leilei1366615/article/details/104340891 "SpringBoot整合MongoDB（一）_spring mongodb_保护我方胖虎的博客-CSDN博客")
    > 
    > [什么是文档数据库_文档型数据库_文档存储数据库 - AWS 云服务](https://aws.amazon.com/cn/nosql/document/ "什么是文档数据库_文档型数据库_文档存储数据库-AWS云服务")
    
    MongoDB 是 RDBMS 和 NoSQL 的中间产品。NoSQL 中最像关系型数据库的数据库
    
*   ConchDB
    

列存储数据库

*   HBase(大数据必学)
*   分布式文件系统

图关系数据库

用于广告推荐，社交网络

*   Neo4j、InfoGrid

<table><thead><tr><th><strong>分类</strong></th><th><strong>Examples 举例</strong></th><th><strong>典型应用场景</strong></th><th><strong>数据模型</strong></th><th><strong>优点</strong></th><th><strong>缺点</strong></th></tr></thead><tbody><tr><td><strong>键值对（key-value）</strong></td><td>Tokyo Cabinet/Tyrant, Redis, Voldemort, Oracle BDB</td><td>内容缓存，主要用于处理大量数据的高访问负载，也用于一些日志系统等等。</td><td>Key 指向 Value 的键值对，通常用 hash table 来实现</td><td>查找速度快</td><td>数据无结构化，通常只被当作字符串或者二进制数据</td></tr><tr><td><strong>列存储数据库</strong></td><td>Cassandra, HBase, Riak</td><td>分布式的文件系统</td><td>以列簇式存储，将同一列数据存在一起</td><td>查找速度快，可扩展性强，更容易进行分布式扩展</td><td>功能相对局限</td></tr><tr><td><strong>文档型数据库</strong></td><td>CouchDB, MongoDb</td><td>Web 应用（与 Key-Value 类似，Value 是结构化的，不同的是数据库能够了解 Value 的内容）</td><td>Key-Value 对应的键值对，Value 为结构化数据</td><td>数据结构要求不严格，表结构可变，不需要像关系型数据库一样需要预先定义表结构</td><td>查询性能不高，而且缺乏统一的查询语法。</td></tr><tr><td><strong>图形 (Graph) 数据库</strong></td><td>Neo4J, InfoGrid, Infinite Graph</td><td>社交网络，推荐系统等。专注于构建关系图谱</td><td>图结构</td><td>利用图结构相关算法。比如最短路径寻址，N 度关系查找等</td><td>很多时候需要对整个图做计算才能得出需要的信息，而且这种结构不太好做分布式的集群</td></tr></tbody></table>

###   
二、Redis 入门

![](https://i-blog.csdnimg.cn/blog_migrate/85f82d1770ba79525dec9edff840861a.jpeg)

#### Redis 是什么？

> Redis（Remote Dictionary Server )，即远程字典服务
> 
> 一个开源的使用 ANSI C 语言编写、支持网络、可基于内存亦可持久化的日志型、**Key-Value 数据库**，并提供多种语言的 API。
> 
> 与 memcached 一样，为了保证效率，**数据都是缓存在内存中**。区别的是 redis 会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现了 master-slave(主从) 同步。

####   
Redis 的应用场景？

[Redis 之常用的十几种使用场景_菜鸟编程 98K 的博客 - CSDN 博客_redis 使用场景](https://blog.csdn.net/qq_39938758/article/details/105577370 "Redis之常用的十几种使用场景_菜鸟编程98K的博客-CSDN博客_redis使用场景")

**1、缓存**

> [String 类](https://so.csdn.net/so/search?q=String%E7%B1%BB&spm=1001.2101.3001.7020 "String类")型

*   例如：热点数据缓存（例如报表、明星出轨），对象缓存、全页缓存、可以提升热点数据的访问数据。

**2、数据共享[分布式](https://so.csdn.net/so/search?q=%E5%88%86%E5%B8%83%E5%BC%8F&spm=1001.2101.3001.7020 "分布式")**

> String 类型，因为 Redis 是分布式的独立服务，可以在多个应用之间共享

*   例如：分布式 Session，token
    
    ```
    <dependency> 
    	<groupId>org.springframework.session</groupId> 
    	<artifactId>spring-session-data-redis</artifactId> 
    </dependency>
    ```
    

**3、分布式锁**

> String 类型 setnx 方法，只有 key 不存在时才能添加成功，返回 true
> 
> #### 实例
> 
> ```
> public static boolean getLock(String key) {
>     Long flag = jedis.setnx(key, "1");
>     if (flag == 1) {
>         jedis.expire(key, 10);
>     }
>     return flag == 1;
> }
>  
> public static void releaseLock(String key) {
>     jedis.del(key);
> }
> ```

```
set k1 a
setbit k1 6 1
setbit k1 7 0
get k1 
/* 6 7 代表的a的二进制位的修改
a 对应的ASCII码是97，转换为二进制数据是01100001
b 对应的ASCII码是98，转换为二进制数据是01100010
 
因为bit非常节省空间（1 MB=8388608 bit），可以用来做大数据量的统计。
```

**4、全局 ID**

> int 类型，incrby，利用原子性

`incrby userid 1000`

分库分表的场景，一次性拿一段

**5、计数器**

> int 类型，incr 方法

例如：文章的阅读量、微博点赞数、允许一定的延迟，先写入 Redis 再定时同步到数据库

**6、限流**

> int 类型，incr 方法

以访问者的 ip 和其他信息作为 key，访问一次增加一次计数，超过次数则返回 false

**7、位统计**

> String 类型的 bitcount（1.6.6 的 bitmap 数据结构介绍）
> 
> 字符是以 8 位二进制存储的

```
BITOPANDdestkeykey[key...] ，对一个或多个 key 求逻辑并，并将结果保存到 destkey 。 	     
BITOPORdestkeykey[key...] ，对一个或多个 key 求逻辑或，并将结果保存到 destkey 。 
BITOPXORdestkeykey[key...] ，对一个或多个 key 求逻辑异或，并将结果保存到 destkey 。 
BITOPNOTdestkeykey ，对给定 key 求逻辑非，并将结果保存到 destkey 。
```

*   例如：在线用户统计，留存用户统计
    
*   支持按位与、按位或等等操作
    
    ```
    // 获取差集
    sdiff set1 set2
    // 获取交集（intersection ）
    sinter set1 set2
    // 获取并集
    sunion set1 set2
    ```
    
*   计算出 7 天都在线的用户
    
    ```
    redis-benchmark -h localhost -p 6379 -c 100 -n 100000
    > 表示向localhost 6379端口也就是我们的redis-server 以100的并发数发送10,0000个请求进行性能测试
    ```
    

**8、购物车**

> String 或 hash。所有 String 可以做的 hash 都可以做

![](https://i-blog.csdnimg.cn/blog_migrate/74f914f7179bbb971b28539040471781.png)

key：用户 id；field：商品 id；value：商品数量。  
+1：hincr。-1：hdecr。删除：hdel。全选：hgetall。商品数：hlen。

**9、用户消息时间线 [timeline](https://so.csdn.net/so/search?q=timeline&spm=1001.2101.3001.7020 "timeline")**

> list，双向链表，直接作为 timeline 就好了。插入有序

**10、消息队列**

> List 提供了两个阻塞的弹出操作：blpop/brpop，可以设置超时时间

blpop：blpop key1 timeout 移除并获取列表的第一个元素，如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。

brpop：brpop key1 timeout 移除并获取列表的最后一个元素，如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。

上面的操作。其实就是 java 的阻塞队列。学习的东西越多。学习成本越低

队列：先进先除：rpush blpop，左头右尾，右边进入队列，左边出队列

栈：先进后出：rpush brpop

**11、抽奖**

> 自带一个随机获得值

`spop myset`

**12、点赞、签到、打卡**

![](https://i-blog.csdnimg.cn/blog_migrate/6c22b7b58268cdffac46404414fff5d8.png)

假如上面的微博 ID 是 t1001，用户 ID 是 u3001

用 like:t1001 来维护 t1001 这条微博的所有点赞用户

点赞了这条微博：sadd like:t1001 u3001

取消点赞：srem like:t1001 u3001

是否点赞：sismember like:t1001 u3001

点赞的所有用户：smembers like:t1001

点赞数：scard like:t1001

​ 是不是比数据库简单多了。哈哈

**13、商品标签**

![](https://i-blog.csdnimg.cn/blog_migrate/3a2163683fd7f767bb3aa2d6724e0f4e.png)

老规矩，用 tags:i5001 来维护商品所有的标签。

sadd tags:i5001 画面清晰细腻

sadd tags:i5001 真彩清晰显示屏

sadd tags:i5001 流程至极

**14、商品筛选**

```
127.0.0.1:6379> config get databases # 命令行查看数据库数量databases
1) "databases"
2) "16"
 
127.0.0.1:6379> select 8 # 切换数据库 DB 8
OK
127.0.0.1:6379[8]> dbsize # 查看数据库大小
(integer) 0
 
# 不同数据库之间 数据是不能互通的，并且dbsize 是根据库中key的个数。
127.0.0.1:6379> set name sakura 
OK
127.0.0.1:6379> SELECT 8
OK
127.0.0.1:6379[8]> get name # db8中并不能获取db0中的键值对。
(nil)
127.0.0.1:6379[8]> DBSIZE
(integer) 0
127.0.0.1:6379[8]> SELECT 0
OK
127.0.0.1:6379> keys *
1) "counter:__rand_int__"
2) "mylist"
3) "name"
4) "key:__rand_int__"
5) "myset:__rand_int__"
127.0.0.1:6379> DBSIZE # size和key个数相关
(integer) 5
```

![](https://i-blog.csdnimg.cn/blog_migrate/6d9c68643c1cf91e2956ac76048ff487.png)

假如：iPhone11 上市了

`sadd brand:apple iPhone11`

`sadd brand:ios iPhone11`

`sad screensize:6.0-6.24 iPhone11`

`sad screentype:lcd iPhone 11`

赛选商品，苹果的、ios 的、屏幕在 6.0-6.24 之间的，屏幕材质是 LCD 屏幕

`sinter brand:apple brand:ios screensize:6.0-6.24 screentype:lcd`

**15、用户关注、推荐模型**

> follow 关注 fans 粉丝

*   相互关注：
    *   sadd 1:follow 2
    *   sadd 2:fans 1
    *   sadd 1:fans 2
    *   sadd 2:follow 1
*   我关注的人也关注了他 (取交集)：
    *   sinter 1:follow 2:fans
*   可能认识的人：
    *   用户 1 可能认识的人 (差集)：sdiff 2:follow 1:follow
    *   用户 2 可能认识的人：sdiff 1:follow 2:follow

**16、排行榜**

id 为 6001 的新闻点击数加 1：zincrby hotNews:20190926 1 n6001

获取今天点击最多的 15 条：zrevrange hotNews:20190926 0 15 withscores

![](https://i-blog.csdnimg.cn/blog_migrate/19d0da0b72054ea0c2219533b511f5a9.png)

redis 用来干什么？

1.  内存存储，需要持久化到数据库（RDB、AOF）
2.  高效率、用于高速缓冲
3.  发布订阅系统
4.  地图信息分析
5.  计时器、计数器 (eg：浏览量)
6.  …

####   
环境搭建

官网：https://redis.io/

推荐使用 Linux 服务器学习。

windows 版本的 Redis 已经停更很久了…

**window 搭建**

下载压缩包后解压：

![](https://i-blog.csdnimg.cn/blog_migrate/afe1ad23d1848871f7c87e8d4ab1c7cf.png)

**此时运行 redis-server.exe 会创建一个临时服务**，当窗口关闭，服务也会随之关闭。

![](https://i-blog.csdnimg.cn/blog_migrate/c9172377514c72892299a95cb13e2abf.png)

然后我们启动运行 redis-cli 就可以连接到 redis-server，Redis 的默认端口 6379

输入 ping 命令，就会得到 PONG 的消息，证明连接成功。

![](https://i-blog.csdnimg.cn/blog_migrate/4d09a7fcd2e9065cc2e7bf6e5d741920.png)

Redis 可以设置密码的噢：（这里主要讲解使用命令行设置。还可以通过配置文件设置。）

1.  `config get requirepass =>`![](https://i-blog.csdnimg.cn/blog_migrate/9e73754cb4431f03559eb30638c8ecb0.png)
2.  默认是`“”`，`config set requirepass 123456`设置密码为 123456，执行成功返回消息 OK
3.  现在进行 ping，就要求身份认证![](https://i-blog.csdnimg.cn/blog_migrate/c75aa03c3da9ca4560331e65f004ef74.png)
4.  auth + 密码就可以完成身份认证![](https://i-blog.csdnimg.cn/blog_migrate/9449b7ba547999e7e1c7167282adeb39.png)
5.  然后就可以进行使用了，由于是键值对存储，使用简单的 get/set 就可以

![](https://i-blog.csdnimg.cn/blog_migrate/826962c44dfcd1ad5b6a6bb7d8c1e1d6.png)

**我们之前说到是使用的临时服务，那么肯定也能像 mysql 一样安装常驻服务。**

1.  进入 Redis 解压目录，输入 cmd 命令：
    
    `redis-server.exe --service-install redis.windows.conf --service-name 服务名 --loglevel verbose`
    
    就可以安装 Redis 服务了，这里使用 Redis-server 作为服务名
    
    ![](https://i-blog.csdnimg.cn/blog_migrate/2978b776a62074e3839441f8f779e234.png)
    
2.  然后我们启动服务，输入 cmd 命令：
    
    `redis-server.exe --service-start --service-name 服务名`
    

![](https://i-blog.csdnimg.cn/blog_migrate/e145ecb0b1b1c61d9a19b8237793d9af.png)

1.  现在我们直接启动 redis-cli.exe 也能进行连接
    
2.  如果不需要这个服务了，可以停止服务：
    
    `redis-server.exe --service-stop --service-name 服务名`
    
3.  或者卸载服务 (需要先停止服务)
    
    `redis-server.exe --service-uninstall --service-name 服务名`
    

> 参考博客：涉及 redis 安装，前后台启动配置，可视化工具 RedisDesktopManager 使用
> 
> [Windows 中 Redis 的下载安装与修改密码并启动_BADAO_LIUMANG_QIZHI 的博客 - CSDN 博客](https://blog.csdn.net/BADAO_LIUMANG_QIZHI/article/details/107486313 "Windows中Redis的下载安装与修改密码并启动_BADAO_LIUMANG_QIZHI的博客-CSDN博客")

**Linux 搭建**

解压：

`tar -zxvf redis_xxx.tar.gz`

![](https://i-blog.csdnimg.cn/blog_migrate/c194b551a851fda2ffd5f9dcd6f007f9.png)

`yum install gcc-c`  安装 gcc 环境，（安装 c 环境, 因为 redis 是 c 写的）

然后进入 redis 目录下执行`make`命令

然后执行`make install`确认执行完毕

redis 默认安装路径`/usr/local/bin`

![](https://i-blog.csdnimg.cn/blog_migrate/7872bf5dde325274d6d0bbe8e4846baf.png)

配置 redis

将解压文件中的 redis.conf 复制一份即可，在 / usr/local/bin 下创建一个 myconf 目录存放配置文件。  
![](https://i-blog.csdnimg.cn/blog_migrate/1ae2bbe1e22c65cf623c0da61678225c.png)

Redis 默认不是后台启动，修改配置文件：

![](https://i-blog.csdnimg.cn/blog_migrate/0166763b2ddb65468dbdbe5ea2861caf.png)

将 daemonize 改为 yes，就改为了后台启动。

**回到 / usr/local/bin 目录 启动 redis，并设置配置文件（启动 redis 服务器）**

`redis-server myconf/redis.conf`

**然后启动客户端测试连接（启动 redis 客户端）**

`redis-cli -p 6379` (这里可以使用 -h 指定主机号， -p 指定端口号)![](https://i-blog.csdnimg.cn/blog_migrate/85e2a8843317733b822c0ffa814204ed.png)

**客户端中使用`shutdown`关闭 server，`exit`退出客户端，此时客户端和 server 就完全退出了。**

（linux 命令查看 redis 服务端和客户端进程关闭前后是否存在）

![](https://i-blog.csdnimg.cn/blog_migrate/24469d4a035be3c8b9bd7f9ee3d0ed1d.png)![](https://i-blog.csdnimg.cn/blog_migrate/0263440c2c779c95d3169b55b7f7ba41.png)

 ![](https://i-blog.csdnimg.cn/blog_migrate/e1bb8186b62e14512ee4ed8e79367237.png)

#### Redis **可视化**客户端

> **Redis 可视化工具** Redis Desktop Manager**（重要）**
> 
> [Redis Desktop Manager(Redis 可视化工具) 安装及使用教程_redisdesktopmanager-CSDN 博客](https://blog.csdn.net/m0_55070913/article/details/123677891 "Redis Desktop Manager(Redis可视化工具)安装及使用教程_redisdesktopmanager-CSDN博客")

**性能测试工具—Redis-benchmark**

Redis 官方提供的性能测试工具，参数选项如下：

![](https://i-blog.csdnimg.cn/blog_migrate/895fc0d51456a262d7fadd0d2c91ee09.png)

简单测试

```
线程上下文切换，也就是CPU不再执行当前的线程，而去执行其他的线程。
那有哪些原因会导致线程的上下文切换呢？
 
1 线程的时间片用完
2 垃圾回收（会暂停当前工作的线程，先进行垃圾回收）
3 更高优先级的线程运行
4 线程主动调用了某些方法，如sleep，yeild，wait，join，synchronized，lock等
 
当发生上下文切换时，操作系统会保存当前线程的状态，恢复另一个线程的状态，
此时程序计数器会记住下一条jvm指令的执行地址，同时上文记录，程序计数器是线程私有的。
```

结果截图，像是对每个命令进行了测试包括（SET、GET、INCR、LPUSH 等）

![](https://i-blog.csdnimg.cn/blog_migrate/e3c951b27fcf974655407eeaca430d69.png)

那么这些测试结果代表什么呢？  
![](https://i-blog.csdnimg.cn/blog_migrate/5ed383555feffaca629656b6264bffb5.png)

####   
基础知识

> #### redis 的 key 的命名规范：用: 分开，并大写，key 不能太长也不能太短, 键名越长越占资源，太短可读性太差；
> 
> Redis 使用的时候注意[命名空间](https://so.csdn.net/so/search?q=%E5%91%BD%E5%90%8D%E7%A9%BA%E9%97%B4&spm=1001.2101.3001.7020 "命名空间")，一个项目一个命名空间，项目内业务不同命名空间也不同。
> 
> 一般情况下：
> 
>   1) 第一段放置项目名或缩写 如 project
> 
>   1) 第二段把表名转换为 key 前缀 如, user:
> 
>   2) 第三段放置用于区分区 key 的字段, 对应 mysql 中的主键的列名, 如 userid
> 
>   3) 第四段放置主键值, 如 18,16
> 
> 结合起来  PRO:USER:UID:18  是不是很清晰
> 
> 常见的设置登录 token
> 
> key：  PRO:USER:LOGINNAME:373166324   
> 
> value：12kd-dsj5ce-d4445-h4sd472
> 
> ![](https://i-blog.csdnimg.cn/blog_migrate/ecc13b124d26ec2910aa2cdf3d1a280f.png)
> 
> #### [redis key 命名规范_易雪寒的博客 - CSDN 博客_redis 命名规范](https://blog.csdn.net/u013521220/article/details/107640977 "redis key命名规范_易雪寒的博客-CSDN博客_redis命名规范")

> **Redis 默认有 16 个数据库**

![](https://i-blog.csdnimg.cn/blog_migrate/7b3e0548b8f936bc50162ded373cd697.png)

16 个数据库为：DB 0~DB 15  
默认使用 DB 0 ，可以使用`select n`切换到 DB n，`dbsize`可以查看当前数据库的大小，与 key 数量相关。。

```
127.0.0.1:6379> keys * # 查看当前数据库所有key
(empty list or set)
127.0.0.1:6379> set name sakura # set key
OK
127.0.0.1:6379> set age 20
OK
127.0.0.1:6379> keys *
1) "age"
2) "name"
127.0.0.1:6379> move age 1 # 将键值对移动到指定数据库
(integer) 1
127.0.0.1:6379> EXISTS age # 判断键是否存在
(integer) 0 # 不存在
127.0.0.1:6379> EXISTS name
(integer) 1 # 存在
127.0.0.1:6379> SELECT 1
OK
127.0.0.1:6379[1]> keys *
1) "age"
127.0.0.1:6379[1]> del age # 删除键值对
(integer) 1 # 删除个数
 
 
127.0.0.1:6379> set age 20
OK
127.0.0.1:6379> EXPIRE age 15 # 设置键值对的过期时间
 
(integer) 1 # 设置成功 开始计数
127.0.0.1:6379> ttl age # 查看key的过期剩余时间
(integer) 13
127.0.0.1:6379> ttl age
(integer) 11
127.0.0.1:6379> ttl age
(integer) 9
127.0.0.1:6379> ttl age
(integer) -2 # -2 表示key过期，-1表示key未设置过期时间
 
127.0.0.1:6379> get age # 过期的key 会被自动delete
(nil)
127.0.0.1:6379> keys *
1) "name"
 
127.0.0.1:6379> type name # 查看value的数据类型
string
```

`keys *` ：查看当前数据库中所有的 key。

`flushdb`：清空当前数据库中的键值对。

`flushall`：清空所有数据库的键值对。

> **Redis 是单线程的，Redis 是基于内存操作的。**  
> 所以 Redis 的性能瓶颈不是 CPU, 而是机器内存和网络带宽。
> 
> 那么为什么 Redis 的速度如此快呢，性能这么高呢？QPS 达到 10W+

1.  首先 Redis 的底层是使用 C 语言编写，所以性能有优势
2.  我们需要打破两个认知误区
    *   多线程与高性能之间 不是对等的。（多线程不一定高性能）
    *   多线程不一定比单线程效率高。由于多线程之间切换会导致 CPU 的上下文进行切换，也是费时操作，并出现线程争用，加上多线程的锁的消耗。
3.  Redis 将所有数据存放在内存中，且是单线程的，CPU 不用频繁切换上下文。

```
---------------------
APPEND key value: 向指定的key的value后追加字符串
 
127.0.0.1:6379> set msg hello 
OK 
127.0.0.1:6379> append msg " world" 
(integer) 11 
127.0.0.1:6379> get msg 
“hello world”
----------------------
DECR/INCR key: 将指定key的value数值进行+1/-1(仅对于数字)
 
127.0.0.1:6379> set age 20 
OK 
127.0.0.1:6379> incr age 
(integer) 21 
127.0.0.1:6379> decr age 
(integer) 20
----------------------------
INCRBY/DECRBY key n: 按指定的步长对数值进行加减
 
127.0.0.1:6379> INCRBY age 5
(integer) 25 
127.0.0.1:6379> DECRBY age 10 
(integer) 15
--------------------------
INCRBYFLOAT key n: 为数值加上浮点型数值
 
127.0.0.1:6379> INCRBYFLOAT age 5.2 
“20.2”
------------------------------
STRLEN key: 获取key保存值的字符串长度
 
127.0.0.1:6379> get msg 
“hello world” 
127.0.0.1:6379> STRLEN msg 
(integer) 11
----------------------------------
GETRANGE key start end: 按起止位置获取字符串（闭区间，起止位置都取）
 
127.0.0.1:6379> get msg 
“hello world” 
127.0.0.1:6379> GETRANGE msg 3 9 
“lo worl”
-----------------------------------
SETRANGE key offset value:用指定的value 替换key中 offset开始的值
 
127.0.0.1:6379> set msg hello
OK
127.0.0.1:6379> setrange msg 2 hello
(integer) 7
127.0.0.1:6379> get msg
"hehello"
127.0.0.1:6379> set msg2 world
OK
127.0.0.1:6379> setrange msg2 2 ww
(integer) 5
127.0.0.1:6379> get msg2
"wowwd"
-------------------------------------
GETSET key value: 将给定 key 的值设为 value ，并返回 key 的旧值(old value)。
 
127.0.0.1:6379> GETSET msg test 
“hello world”
------------------------------------
SETNX key value: 仅当key不存在时进行set
 
127.0.0.1:6379> SETNX msg test 
(integer) 0 
127.0.0.1:6379> SETNX name sakura 
(integer) 1
---------------------------------------
SETEX key seconds value: set 键值对并设置过期时间
 
127.0.0.1:6379> setex name 10 root 
OK 
127.0.0.1:6379> get name 
(nil)
-------------------------------------
MSET key1 value1 [key2 value2..]: 批量set键值对
 
127.0.0.1:6379> MSET k1 v1 k2 v2 k3 v3 
OK
----------------------------------
MSETNX key1 value1 [key2 value2..]: 批量设置键值对，仅当参数中所有的key都不存在时执行
 
127.0.0.1:6379> MSETNX k1 v1 k4 v4 
(integer) 0
------------------------------------
MGET key1 [key2..]: 批量获取多个key保存的值
 
127.0.0.1:6379> MGET k1 k2 k3 
1) “v1” 
2) “v2” 
3) “v3”
----------------------------------------
PSETEX key milliseconds value: 和 SETEX 命令相似，但它以毫秒为单位设置 key 的生存时间
```

> **Redis 的 key 不能重复，会覆盖:**

1、redis key 相同是会覆盖的。redis 本身就是以 key 为主键的，key 相同肯定覆盖。如果是要避免使用同一个 KEY。可以在不同的系统生成 GUID 的方式做 key。也可以让 redis 产生 key 给不同的系统使用，因为 redis 是单线程的，这样就能避免同 key。

2、如果两个系统需要用到同一个 key，为了避免一致性问题，那么可以使用事务的方式 MULTI/EXEC，MULTI，EXEC 中间的指令会执行完后才继续执行后面的指令，另外，还可以使用 lua 脚本的方式调用，一个 lua 脚本里面的指令也是原子级别的，执行完后才会继续执行其他指令。

![](https://i-blog.csdnimg.cn/blog_migrate/7d13ff286794fef457a9c26e24b595b1.png)

###   
三、五大数据类型

> Redis 是一个开源（BSD 许可），内存存储的数据结构服务器，可用作数据库，高速缓存和消息队列代理。它支持[字符串](https://www.redis.net.cn/tutorial/3508.html "字符串")、[哈希表](https://www.redis.net.cn/tutorial/3509.html "哈希表")、[列表](https://www.redis.net.cn/tutorial/3510.html "列表")、[集合](https://www.redis.net.cn/tutorial/3511.html "集合")、[有序集合](https://www.redis.net.cn/tutorial/3512.html "有序集合")，[位图](https://www.redis.net.cn/tutorial/3508.html "位图")，[hyperloglogs](https://www.redis.net.cn/tutorial/3513.html "hyperloglogs") 等数据类型。内置复制、[Lua 脚本](https://www.redis.net.cn/tutorial/3516.html "Lua脚本")、LRU 收回、[事务](https://www.redis.net.cn/tutorial/3515.html "事务")以及不同级别磁盘持久化功能，同时通过 Redis Sentinel 提供高可用，通过 Redis Cluster 提供自动[分区](https://www.redis.net.cn/tutorial/3524.html "分区")。

#### Redis-key

> 在 redis 中无论什么数据类型，在数据库中都是也 key-value 形式保存，通过进行对 Redis-key 的操作，来完成对数据库中数据的操作。

下面学习的命令：

*   `exists key`
*   `del key`
*   `move key db`
*   `expire key second`
*   `type key`

```
---------------------------LPUSH---RPUSH---LRANGE--------------------------------
 
127.0.0.1:6379> LPUSH mylist k1 # LPUSH mylist=>{1}
(integer) 1
127.0.0.1:6379> LPUSH mylist k2 # LPUSH mylist=>{2,1}
(integer) 2
127.0.0.1:6379> RPUSH mylist k3 # RPUSH mylist=>{2,1,3}
(integer) 3
127.0.0.1:6379> get mylist # 普通的get是无法获取list值的
(error) WRONGTYPE Operation against a key holding the wrong kind of value
127.0.0.1:6379> LRANGE mylist 0 4 # LRANGE 获取起止位置范围内的元素
1) "k2"
2) "k1"
3) "k3"
127.0.0.1:6379> LRANGE mylist 0 2
1) "k2"
2) "k1"
3) "k3"
127.0.0.1:6379> LRANGE mylist 0 1
1) "k2"
2) "k1"
127.0.0.1:6379> LRANGE mylist 0 -1 # 获取全部元素
1) "k2"
2) "k1"
3) "k3"
 
---------------------------LPUSHX---RPUSHX-----------------------------------
 
127.0.0.1:6379> LPUSHX list v1 # list不存在 LPUSHX失败
(integer) 0
127.0.0.1:6379> LPUSHX list v1 v2  
(integer) 0
127.0.0.1:6379> LPUSHX mylist k4 k5 # 向mylist中 左边 PUSH k4 k5
(integer) 5
127.0.0.1:6379> LRANGE mylist 0 -1
1) "k5"
2) "k4"
3) "k2"
4) "k1"
5) "k3"
 
---------------------------LINSERT--LLEN--LINDEX--LSET----------------------------
 
127.0.0.1:6379> LINSERT mylist after k2 ins_key1 # 在k2元素后 插入ins_key1
(integer) 6
127.0.0.1:6379> LRANGE mylist 0 -1
1) "k5"
2) "k4"
3) "k2"
4) "ins_key1"
5) "k1"
6) "k3"
127.0.0.1:6379> LLEN mylist # 查看mylist的长度
(integer) 6
127.0.0.1:6379> LINDEX mylist 3 # 获取下标为3的元素
"ins_key1"
127.0.0.1:6379> LINDEX mylist 0
"k5"
127.0.0.1:6379> LSET mylist 3 k6 # 将下标3的元素 set值为k6
OK
127.0.0.1:6379> LRANGE mylist 0 -1
1) "k5"
2) "k4"
3) "k2"
4) "k6"
5) "k1"
6) "k3"
 
---------------------------LPOP--RPOP--------------------------
 
127.0.0.1:6379> LPOP mylist # 左侧(头部)弹出
"k5"
127.0.0.1:6379> RPOP mylist # 右侧(尾部)弹出
"k3"
 
---------------------------RPOPLPUSH--------------------------
 
127.0.0.1:6379> LRANGE mylist 0 -1
1) "k4"
2) "k2"
3) "k6"
4) "k1"
127.0.0.1:6379> RPOPLPUSH mylist newlist # 将mylist的最后一个值(k1)弹出，加入到newlist的头部
"k1"
127.0.0.1:6379> LRANGE newlist 0 -1
1) "k1"
127.0.0.1:6379> LRANGE mylist 0 -1
1) "k4"
2) "k2"
3) "k6"
 
---------------------------LTRIM--------------------------
 
127.0.0.1:6379> LTRIM mylist 0 1 # 截取mylist中的 0~1部分
OK
127.0.0.1:6379> LRANGE mylist 0 -1
1) "k4"
2) "k2"
 
# 初始 mylist: k2,k2,k2,k2,k2,k2,k4,k2,k2,k2,k2
---------------------------LREM--------------------------
 
127.0.0.1:6379> LREM mylist 3 k2 # 从头部开始搜索 至多删除3个 k2
(integer) 3
# 删除后：mylist: k2,k2,k2,k4,k2,k2,k2,k2
 
127.0.0.1:6379> LREM mylist -2 k2 #从尾部开始搜索 至多删除2个 k2
(integer) 2
# 删除后：mylist: k2,k2,k2,k4,k2,k2
 
 
---------------------------BLPOP--BRPOP--------------------------
 
mylist: k2,k2,k2,k4,k2,k2
newlist: k1
 
127.0.0.1:6379> BLPOP newlist mylist 30 # 从newlist中弹出第一个值，mylist作为候选
1) "newlist" # 弹出
2) "k1"
127.0.0.1:6379> BLPOP newlist mylist 30
1) "mylist" # 由于newlist空了 从mylist中弹出
2) "k2"
127.0.0.1:6379> BLPOP newlist 30
(30.10s) # 超时了
 
127.0.0.1:6379> BLPOP newlist 30 # 我们连接另一个客户端向newlist中push了test, 阻塞被解决。
1) "newlist"
2) "test"
(12.54s)
```

关于`TTL`命令

Redis 的 key，通过 TTL 命令返回 key 的过期时间，一般来说有 3 种：

1.  当前 key 没有设置过期时间，所以会返回 - 1.
2.  当前 key 有设置过期时间，而且 key 已经过期，所以会返回 - 2.
3.  当前 key 有设置过期时间，且 key 还没有过期，故会返回 key 的正常剩余时间.

关于重命名`RENAME`和`RENAMENX`

*   `RENAME key newkey`修改 key 的名称
*   `RENAMENX key newkey`仅当 newkey 不存在时，将 key 改名为 newkey 。

更多：查阅官网教程：https://www.redis.net.cn/tutorial/3507.html![](https://i-blog.csdnimg.cn/blog_migrate/e61dc8b9ed217690f1693d0457fd296d.png)

####   
String(字符串)

普通的 set、get 直接略过。

<table><thead><tr><th>命令</th><th>描述</th><th>示例</th></tr></thead><tbody><tr><td><code>APPEND key value</code></td><td>向指定的 key 的 value 后追加字符串</td><td>127.0.0.1:6379&gt; set msg hello<br>OK<br>127.0.0.1:6379&gt; append msg "world"<br>(integer) 11<br>127.0.0.1:6379&gt; get msg<br>"hello world"</td></tr><tr><td><code>DECR/INCR key</code></td><td>将指定 key 的 value 数值进行 + 1/-1(仅对于数字)</td><td>127.0.0.1:6379&gt; set age 20<br>OK<br>127.0.0.1:6379&gt; incr age<br>(integer) 21<br>127.0.0.1:6379&gt; decr age<br>(integer) 20</td></tr><tr><td><code>INCRBY/DECRBY key n</code></td><td>按指定的步长对数值进行加减</td><td>127.0.0.1:6379&gt; INCRBY age 5<br>(integer) 25<br>127.0.0.1:6379&gt; DECRBY age 10<br>(integer) 15</td></tr><tr><td><code>INCRBYFLOAT key n</code></td><td>为数值加上浮点型数值</td><td>127.0.0.1:6379&gt; INCRBYFLOAT age 5.2<br>"20.2"</td></tr><tr><td><code>STRLEN key</code></td><td>获取 key 保存值的字符串长度</td><td>127.0.0.1:6379&gt; get msg<br>"hello world"<br>127.0.0.1:6379&gt; STRLEN msg<br>(integer) 11</td></tr><tr><td><code>GETRANGE key start end</code></td><td>按起止位置获取字符串（闭区间，起止位置都取）</td><td>127.0.0.1:6379&gt; get msg<br>"hello world"<br>127.0.0.1:6379&gt; GETRANGE msg 3 9<br>"lo worl"</td></tr><tr><td><code>SETRANGE key offset value</code></td><td>用指定的 value 替换 key 中 offset 开始的值</td><td>127.0.0.1:6379&gt; SETRANGE msg 2 hello<br>(integer) 7<br>127.0.0.1:6379&gt; get msg<br>"tehello"</td></tr><tr><td><code>GETSET key value</code></td><td>将给定 key 的值设为 value ，并返回 key 的旧值 (old value)。</td><td>127.0.0.1:6379&gt; GETSET msg test<br>"hello world"</td></tr><tr><td><code>SETNX key value</code></td><td><strong>仅当 key 不存在时进行 set</strong></td><td>127.0.0.1:6379&gt; SETNX msg test<br>(integer) 0<br>127.0.0.1:6379&gt; SETNX name sakura<br>(integer) 1</td></tr><tr><td><code>SETEX key seconds value</code></td><td>set 键值对并设置过期时间</td><td>127.0.0.1:6379&gt; setex name 10 root<br>OK<br>127.0.0.1:6379&gt; get name<br>(nil)</td></tr><tr><td><code>MSET key1 value1 [key2 value2..]</code></td><td>批量 set 键值对</td><td>127.0.0.1:6379&gt; MSET k1 v1 k2 v2 k3 v3<br>OK</td></tr><tr><td><code>MSETNX key1 value1 [key2 value2..]</code></td><td>批量设置键值对，仅当参数中所有的 key 都不存在时执行</td><td>127.0.0.1:6379&gt; MSETNX k1 v1 k4 v4<br>(integer) 0</td></tr><tr><td><code>MGET key1 [key2..]</code></td><td>批量获取多个 key 保存的值</td><td>127.0.0.1:6379&gt; MGET k1 k2 k3<br>1) "v1"<br>2) "v2"<br>3) “v3”</td></tr><tr><td><code>PSETEX key milliseconds value</code></td><td>和 SETEX 命令相似，但它以毫秒为单位设置 key 的生存时间，</td><td></td></tr></tbody></table>

**常用命令及其示例：**

```
---------------SADD--SCARD--SMEMBERS--SISMEMBER--------------------
 
127.0.0.1:6379> SADD myset m1 m2 m3 m4 # 向myset中增加成员 m1~m4
(integer) 4
127.0.0.1:6379> SCARD myset # 获取集合的成员数目
(integer) 4
127.0.0.1:6379> smembers myset # 获取集合中所有成员
1) "m4"
2) "m3"
3) "m2"
4) "m1"
127.0.0.1:6379> SISMEMBER myset m5 # 查询m5是否是myset的成员
(integer) 0 # 不是，返回0
127.0.0.1:6379> SISMEMBER myset m2
(integer) 1 # 是，返回1
127.0.0.1:6379> SISMEMBER myset m3
(integer) 1
 
---------------------SRANDMEMBER--SPOP----------------------------------
 
127.0.0.1:6379> SRANDMEMBER myset 3 # 随机返回3个成员
1) "m2"
2) "m3"
3) "m4"
127.0.0.1:6379> SRANDMEMBER myset # 随机返回1个成员
"m3"
127.0.0.1:6379> SPOP myset 2 # 随机移除并返回2个成员
1) "m1"
2) "m4"
# 将set还原到{m1,m2,m3,m4}
 
---------------------SMOVE--SREM----------------------------------------
 
127.0.0.1:6379> SMOVE myset newset m3 # 将myset中m3成员移动到newset集合
(integer) 1
127.0.0.1:6379> SMEMBERS myset
1) "m4"
2) "m2"
3) "m1"
127.0.0.1:6379> SMEMBERS newset
1) "m3"
127.0.0.1:6379> SREM newset m3 # 从newset中移除m3元素
(integer) 1
127.0.0.1:6379> SMEMBERS newset
(empty list or set)
 
# 下面开始是多集合操作,多集合操作中若只有一个参数默认和自身进行运算
# setx=>{m1,m2,m4,m6}, sety=>{m2,m5,m6}, setz=>{m1,m3,m6}
 
-----------------------------SDIFF------------------------------------
 
127.0.0.1:6379> SDIFF setx sety setz # 等价于setx-sety-setz
1) "m4"
127.0.0.1:6379> SDIFF setx sety # setx - sety
1) "m4"
2) "m1"
127.0.0.1:6379> SDIFF sety setx # sety - setx
1) "m5"
 
 
-------------------------SINTER---------------------------------------
# 共同关注（交集）
 
127.0.0.1:6379> SINTER setx sety setz # 求 setx、sety、setx的交集
1) "m6"
127.0.0.1:6379> SINTER setx sety # 求setx sety的交集
1) "m2"
2) "m6"
 
-------------------------SUNION---------------------------------------
 
127.0.0.1:6379> SUNION setx sety setz # setx sety setz的并集
1) "m4"
2) "m6"
3) "m3"
4) "m2"
5) "m1"
6) "m5"
127.0.0.1:6379> SUNION setx sety # setx sety 并集
1) "m4"
2) "m6"
3) "m2"
4) "m1"
5) "m5"
```

![](https://i-blog.csdnimg.cn/blog_migrate/8472d1ee93f6e97bbc6ca0bd621bbb99.png)![](https://i-blog.csdnimg.cn/blog_migrate/654fd96c20b4894791eb6c2a44a2e3cc.png)

#### **  
List(列表)**

> Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素导列表的头部（左边）或者尾部（右边）
> 
> 一个列表最多可以包含 232 - 1 个元素 (4294967295, 每个列表超过 40 亿个元素)。

首先我们列表，可以经过规则定义将其变为队列、栈、双端队列等  
![](https://i-blog.csdnimg.cn/blog_migrate/9e57435607eb9652e0bca23d61b6ebd4.png)

正如图 Redis 中 List 是可以进行双端操作的，所以命令也就分为了 LXXX 和 RLLL 两类，有时候 L 也表示 List 例如 LLEN

<table><thead><tr><th>命令</th><th>描述</th></tr></thead><tbody><tr><td><code>LPUSH/RPUSH key value1[value2..]</code></td><td>从左边 / 右边向列表中 PUSH 值 (一个或者多个)。</td></tr><tr><td><code>LRANGE key start end</code></td><td>获取 list 起止元素 ==（索引从左往右 递增）==</td></tr><tr><td><code>LPUSHX/RPUSHX key value</code></td><td>向已存在的列名中 push 值（一个或者多个）</td></tr><tr><td><code>LINSERT key BEFORE|AFTER pivot value</code></td><td>在指定列表元素的前 / 后 插入 value</td></tr><tr><td><code>LLEN key</code></td><td>查看列表长度</td></tr><tr><td><code>LINDEX key index</code></td><td>通过索引获取列表元素</td></tr><tr><td><code>LSET key index value</code></td><td>通过索引为元素设值</td></tr><tr><td><code>LPOP/RPOP key</code></td><td>从最左边 / 最右边移除值 并返回</td></tr><tr><td><code>RPOPLPUSH source destination</code></td><td>将列表的尾部 (右) 最后一个值弹出，并返回，然后加到另一个列表的头部</td></tr><tr><td><code>LTRIM key start end</code></td><td>截取指定范围内的列表</td></tr><tr><td><code>LREM key count value</code></td><td>List 中是允许 value 重复的<br><code>count &gt; 0</code>：从头部开始搜索 然后删除指定的 value 至多删除 count 个<br><code>count &lt; 0</code>：从尾部开始搜索…<br><code>count = 0</code>：删除列表中所有的指定 value。</td></tr><tr><td><code>BLPOP/BRPOP key1[key2] timout</code></td><td>移出并获取列表的第一个 / 最后一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。</td></tr><tr><td><code>BRPOPLPUSH source destination timeout</code></td><td>和<code>RPOPLPUSH</code>功能相同，如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。</td></tr></tbody></table>

```
------------------------HSET--HMSET--HSETNX----------------
127.0.0.1:6379> HSET studentx name sakura # 将studentx哈希表作为一个对象，设置name为sakura
(integer) 1
127.0.0.1:6379> HSET studentx name gyc # 重复设置field进行覆盖，并返回0
(integer) 0
127.0.0.1:6379> HSET studentx age 20 # 设置studentx的age为20
(integer) 1
127.0.0.1:6379> HMSET studentx sex 1 tel 15623667886 # 设置sex为1，tel为15623667886
OK
127.0.0.1:6379> HSETNX studentx name gyc # HSETNX 设置已存在的field
(integer) 0 # 失败
127.0.0.1:6379> HSETNX studentx email 12345@qq.com
(integer) 1 # 成功
 
----------------------HEXISTS--------------------------------
127.0.0.1:6379> HEXISTS studentx name # name字段在studentx中是否存在
(integer) 1 # 存在
127.0.0.1:6379> HEXISTS studentx addr
(integer) 0 # 不存在
 
-------------------HGET--HMGET--HGETALL-----------
127.0.0.1:6379> HGET studentx name # 获取studentx中name字段的value
"gyc"
127.0.0.1:6379> HMGET studentx name age tel # 获取studentx中name、age、tel字段的value
1) "gyc"
2) "20"
3) "15623667886"
127.0.0.1:6379> HGETALL studentx # 获取studentx中所有的field及其value
 1) "name"
 2) "gyc"
 3) "age"
 4) "20"
 5) "sex"
 6) "1"
 7) "tel"
 8) "15623667886"
 9) "email"
10) "12345@qq.com"
 
 
--------------------HKEYS--HLEN--HVALS--------------
127.0.0.1:6379> HKEYS studentx # 查看studentx中所有的field
1) "name"
2) "age"
3) "sex"
4) "tel"
5) "email"
127.0.0.1:6379> HLEN studentx # 查看studentx中的字段数量
(integer) 5
127.0.0.1:6379> HVALS studentx # 查看studentx中所有的value
1) "gyc"
2) "20"
3) "1"
4) "15623667886"
5) "12345@qq.com"
 
-------------------------HDEL--------------------------
127.0.0.1:6379> HDEL studentx sex tel # 删除studentx 中的sex、tel字段
(integer) 2
127.0.0.1:6379> HKEYS studentx
1) "name"
2) "age"
3) "email"
 
-------------HINCRBY--HINCRBYFLOAT------------------------
127.0.0.1:6379> HINCRBY studentx age 1 # studentx的age字段数值+1
(integer) 21
127.0.0.1:6379> HINCRBY studentx name 1 # 非整数字型字段不可用
(error) ERR hash value is not an integer
127.0.0.1:6379> HINCRBYFLOAT studentx weight 0.6 # weight字段增加0.6
"90.8"
```

####   
Set(集合)

> **Redis 的 Set 是 string 类型的无序集合。集合成员是唯一的，这就意味着集合中不能出现重复的数据。**
> 
> Redis 中 集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)。
> 
> 集合中最大的成员数为 232 - 1 (4294967295, 每个集合可存储 40 多亿个成员)。

<table><thead><tr><th>命令</th><th>描述</th></tr></thead><tbody><tr><td><code>SADD key member1[member2..]</code></td><td>向集合中无序增加一个 / 多个成员</td></tr><tr><td><code>SCARD key</code></td><td>获取集合的成员数</td></tr><tr><td><code>SMEMBERS key</code></td><td>返回集合中所有的成员</td></tr><tr><td><code>SISMEMBER key member</code></td><td>查询 member 元素是否是集合的成员, 结果是无序的</td></tr><tr><td><code>SRANDMEMBER key [count]</code></td><td>随机返回集合中 count 个成员，count 缺省值为 1</td></tr><tr><td><code>SPOP key [count]</code></td><td>随机移除并返回集合中 count 个成员，count 缺省值为 1</td></tr><tr><td><code>SMOVE source destination member</code></td><td>将 source 集合的成员 member 移动到 destination 集合</td></tr><tr><td><code>SREM key member1[member2..]</code></td><td>移除集合中一个 / 多个成员</td></tr><tr><td><code>SDIFF key1[key2..]</code></td><td>返回所有集合的差集 key1- key2 - …</td></tr><tr><td><code>SDIFFSTORE destination key1[key2..]</code></td><td>在 SDIFF 的基础上，将结果保存到集合中 ==(覆盖)==。不能保存到其他类型 key 噢！</td></tr><tr><td><code>SINTER key1 [key2..]</code></td><td>返回所有集合的交集</td></tr><tr><td><code>SINTERSTORE destination key1[key2..]</code></td><td>在 SINTER 的基础上，存储结果到集合中。覆盖</td></tr><tr><td><code>SUNION key1 [key2..]</code></td><td>返回所有集合的并集</td></tr><tr><td><code>SUNIONSTORE destination key1 [key2..]</code></td><td>在 SUNION 的基础上，存储结果到及和张。覆盖</td></tr><tr><td><code>SSCAN KEY [MATCH pattern] [COUNT count]</code></td><td>在大量数据环境下，使用此命令变量集合中元素，每次遍历部分</td></tr></tbody></table>

```
-------------------ZADD--ZCARD--ZCOUNT--------------
127.0.0.1:6379> ZADD myzset 1 m1 2 m2 3 m3 # 向有序集合myzset中添加成员m1 score=1 以及成员m2 score=2..
(integer) 2
127.0.0.1:6379> ZCARD myzset # 获取有序集合的成员数
(integer) 2
127.0.0.1:6379> ZCOUNT myzset 0 1 # 获取score在 [0,1]区间的成员数量
(integer) 1
127.0.0.1:6379> ZCOUNT myzset 0 2
(integer) 2
 
----------------ZINCRBY--ZSCORE--------------------------
127.0.0.1:6379> ZINCRBY myzset 5 m2 # 将成员m2的score +5
"7"
127.0.0.1:6379> ZSCORE myzset m1 # 获取成员m1的score
"1"
127.0.0.1:6379> ZSCORE myzset m2
"7"
 
--------------ZRANK--ZRANGE-----------------------------------
127.0.0.1:6379> ZRANK myzset m1 # 获取成员m1的索引，索引按照score排序，score相同索引值按字典顺序顺序增加
(integer) 0
127.0.0.1:6379> ZRANK myzset m2
(integer) 2
127.0.0.1:6379> ZRANGE myzset 0 1 # 获取索引在 0~1的成员
1) "m1"
2) "m3"
127.0.0.1:6379> ZRANGE myzset 0 -1 # 获取全部成员
1) "m1"
2) "m3"
3) "m2"
 
#testset=>{abc,add,amaze,apple,back,java,redis} score均为0
------------------ZRANGEBYLEX---------------------------------
127.0.0.1:6379> ZRANGEBYLEX testset - + # 返回所有成员
1) "abc"
2) "add"
3) "amaze"
4) "apple"
5) "back"
6) "java"
7) "redis"
127.0.0.1:6379> ZRANGEBYLEX testset - + LIMIT 0 3 # 分页 按索引显示查询结果的 0,1,2条记录
1) "abc"
2) "add"
3) "amaze"
127.0.0.1:6379> ZRANGEBYLEX testset - + LIMIT 3 3 # 显示 3,4,5条记录
1) "apple"
2) "back"
3) "java"
127.0.0.1:6379> ZRANGEBYLEX testset (- [apple # 显示 (-,apple] 区间内的成员
1) "abc"
2) "add"
3) "amaze"
4) "apple"
127.0.0.1:6379> ZRANGEBYLEX testset [apple [java # 显示 [apple,java]字典区间的成员
1) "apple"
2) "back"
3) "java"
 
-----------------------ZRANGEBYSCORE---------------------
127.0.0.1:6379> ZRANGEBYSCORE myzset 1 10 # 返回score在 [1,10]之间的的成员
1) "m1"
2) "m3"
3) "m2"
127.0.0.1:6379> ZRANGEBYSCORE myzset 1 5
1) "m1"
2) "m3"
 
--------------------ZLEXCOUNT-----------------------------
127.0.0.1:6379> ZLEXCOUNT testset - +
(integer) 7
127.0.0.1:6379> ZLEXCOUNT testset [apple [java
(integer) 3
 
------------------ZREM--ZREMRANGEBYLEX--ZREMRANGBYRANK--ZREMRANGEBYSCORE--------------------------------
127.0.0.1:6379> ZREM testset abc # 移除成员abc
(integer) 1
127.0.0.1:6379> ZREMRANGEBYLEX testset [apple [java # 移除字典区间[apple,java]中的所有成员
(integer) 3
127.0.0.1:6379> ZREMRANGEBYRANK testset 0 1 # 移除排名0~1的所有成员
(integer) 2
127.0.0.1:6379> ZREMRANGEBYSCORE myzset 0 3 # 移除score在 [0,3]的成员
(integer) 2
 
 
# testset=> {abc,add,apple,amaze,back,java,redis} score均为0
# myzset=> {(m1,1),(m2,2),(m3,3),(m4,4),(m7,7),(m9,9)}
----------------ZREVRANGE--ZREVRANGEBYSCORE--ZREVRANGEBYLEX-----------
127.0.0.1:6379> ZREVRANGE myzset 0 3 # 按score递减排序，然后按索引，返回结果的 0~3
1) "m9"
2) "m7"
3) "m4"
4) "m3"
127.0.0.1:6379> ZREVRANGE myzset 2 4 # 返回排序结果的 索引的2~4
1) "m4"
2) "m3"
3) "m2"
127.0.0.1:6379> ZREVRANGEBYSCORE myzset 6 2 # 按score递减顺序 返回集合中分数在[2,6]之间的成员
1) "m4"
2) "m3"
3) "m2"
127.0.0.1:6379> ZREVRANGEBYLEX testset [java (add # 按字典倒序 返回集合中(add,java]字典区间的成员
1) "java"
2) "back"
3) "apple"
4) "amaze"
 
-------------------------ZREVRANK------------------------------
127.0.0.1:6379> ZREVRANK myzset m7 # 按score递减顺序，返回成员m7索引
(integer) 1
127.0.0.1:6379> ZREVRANK myzset m2
(integer) 4
 
 
# mathscore=>{(xm,90),(xh,95),(xg,87)} 小明、小红、小刚的数学成绩
# enscore=>{(xm,70),(xh,93),(xg,90)} 小明、小红、小刚的英语成绩
-------------------ZINTERSTORE--ZUNIONSTORE-----------------------------------
127.0.0.1:6379> ZINTERSTORE sumscore 2 mathscore enscore # 将mathscore enscore进行合并 结果存放到sumscore
(integer) 3
127.0.0.1:6379> ZRANGE sumscore 0 -1 withscores # 合并后的score是之前集合中所有score的和
1) "xm"
2) "160"
3) "xg"
4) "177"
5) "xh"
6) "188"
 
127.0.0.1:6379> ZUNIONSTORE lowestscore 2 mathscore enscore AGGREGATE MIN # 取两个集合的成员score最小值作为结果的
(integer) 3
127.0.0.1:6379> ZRANGE lowestscore 0 -1 withscores
1) "xm"
2) "70"
3) "xg"
4) "87"
5) "xh"
6) "93"
```

####   
Hash

> Redis hash 是一个 string 类型的 field 和 value 的映射表，hash 特别适合用于存储对象。
> 
> Set 就是一种简化的 Hash, 只变动 key, 而 value 使用默认值填充。可以将一个 Hash 表作为一个对象进行存储，表中存放对象的信息。

<table><thead><tr><th>命令</th><th>描述</th></tr></thead><tbody><tr><td><code>HSET key field value</code></td><td>将哈希表 key 中的字段 field 的值设为 value 。重复设置同一个 field 会覆盖, 返回 0</td></tr><tr><td><code>HMSET key field1 value1 [field2 value2..]</code></td><td>同时将多个 field-value (域 - 值) 对设置到哈希表 key 中。</td></tr><tr><td><code>HSETNX key field value</code></td><td>只有在字段 field 不存在时，设置哈希表字段的值。</td></tr><tr><td><code>HEXISTS key field value</code></td><td>查看哈希表 key 中，指定的字段是否存在。</td></tr><tr><td><code>HGET key field value</code></td><td>获取存储在哈希表中指定字段的值</td></tr><tr><td><code>HMGET key field1 [field2..]</code></td><td>获取所有给定字段的值</td></tr><tr><td><code>HGETALL key</code></td><td>获取在哈希表 key 的所有字段和值</td></tr><tr><td><code>HKEYS key</code></td><td>获取哈希表 key 中所有的字段</td></tr><tr><td><code>HLEN key</code></td><td>获取哈希表中字段的数量</td></tr><tr><td><code>HVALS key</code></td><td>获取哈希表中所有值</td></tr><tr><td><code>HDEL key field1 [field2..]</code></td><td>删除哈希表 key 中一个 / 多个 field 字段</td></tr><tr><td><code>HINCRBY key field n</code></td><td>为哈希表 key 中的指定字段的整数值加上增量 n，并返回增量后结果&nbsp;一样只适用于整数型字段</td></tr><tr><td><code>HINCRBYFLOAT key field n</code></td><td>为哈希表 key 中的指定字段的浮点数值加上增量 n。</td></tr><tr><td><code>HSCAN key cursor [MATCH pattern] [COUNT count]</code></td><td>迭代哈希表中的键值对。</td></tr></tbody></table>

```
-----------------geoadd--------------------
127.0.0.1:6379> geoadd china:city 111.4 30.5 yichang # 将经纬度(111.4,30.5)的地理坐标加入china:city 并命名yichang
(integer) 1
127.0.0.1:6379> geoadd china:city 121.4 31.4 shanghai # ...
(integer) 1
127.0.0.1:6379> geoadd china:city 120.2 30.2 hangzhou
(integer) 1
127.0.0.1:6379> geoadd china:city 104.1 30.6 chengdu
(integer) 1
 
-----------------geopos--------------------
127.0.0.1:6379> geopos china:city shanghai
1) 1) "121.40000134706497" # 经度
   2) "31.400000253193539" # 纬度
   
-----------------geodist--------------------
127.0.0.1:6379> GEODIST china:city yichang shanghai # 获取yichang和shanghai两个地理坐标的距离(单位:m)
"958793.6834"
127.0.0.1:6379> GEODIST china:city shanghai yichang km # 单位：km
"958.7937"
```

[Redis 可以直接存储对象吗（redis 能直接存对象吗）- 数据库运维技术服务](https://www.dbs724.com/307650.html "Redis可以直接存储对象吗（redis能直接存对象吗）-数据库运维技术服务")

1、存对象的二进制，通过 string 类型    

2、存对象的 json 字符串，通过 string 类型    

3、对象转 map，通过 hash 类型存进去 addHash(string key Map map) ）

####   
Zset(有序集合)

> Redis 有序集合 zset 与普通集合 set 非常相似。
> 
> Zset 是一个没有重复元素的字符串集合。不同之处是有序集合的每个成员都关联了一个评分 (score) , 这个评分( score) 被用来按照从最低分到最高分的方式排序集合中的成员。集合的成员是唯一的，但是评分可以是重复了。
> 
> 不同的是每个元素都会关联一个 double 类型的分数。redis 正是通过分数来为集合中的成员进行从小到大的排序。
> 
> 如果 score 相同：按字典顺序排序
> 
> 有序集合的成员是唯一的, 但分数 (score) 却可以重复。

<table><thead><tr><th>命令</th><th>描述</th></tr></thead><tbody><tr><td><code>ZADD key score member1 [score2 member2]</code></td><td>向有序集合添加一个或多个成员，或者更新已存在成员的分数</td></tr><tr><td><code>ZCARD key</code></td><td>获取有序集合的成员数</td></tr><tr><td><code>ZCOUNT key min max</code></td><td>计算在有序集合中指定区间 score 的成员数</td></tr><tr><td><code>ZINCRBY key n member</code></td><td>有序集合中对指定成员的分数加上增量 n</td></tr><tr><td><code>ZSCORE key member</code></td><td>返回有序集中，成员的分数值</td></tr><tr><td><code>ZRANK key member</code></td><td>返回有序集合中指定成员的索引</td></tr><tr><td><code>ZRANGE key start end</code></td><td>通过索引区间返回有序集合成指定区间内的成员</td></tr><tr><td><code>ZRANGEBYLEX key min max</code></td><td>通过字典区间返回有序集合的成员</td></tr><tr><td><code>ZRANGEBYSCORE key min max</code></td><td>通过分数返回有序集合指定区间内的成员 ==-inf 和 +inf 分别表示最小最大值，只支持开区间 ()==</td></tr><tr><td><code>ZLEXCOUNT key min max</code></td><td>在有序集合中计算指定字典区间内成员数量</td></tr><tr><td><code>ZREM key member1 [member2..]</code></td><td>移除有序集合中一个 / 多个成员</td></tr><tr><td><code>ZREMRANGEBYLEX key min max</code></td><td>移除有序集合中给定的字典区间的所有成员</td></tr><tr><td><code>ZREMRANGEBYRANK key start stop</code></td><td>移除有序集合中给定的排名区间的所有成员</td></tr><tr><td><code>ZREMRANGEBYSCORE key min max</code></td><td>移除有序集合中给定的分数区间的所有成员</td></tr><tr><td><code>ZREVRANGE key start end</code></td><td>返回有序集中指定区间内的成员，通过索引，分数从高到底</td></tr><tr><td><code>ZREVRANGEBYSCORRE key max min</code></td><td>返回有序集中指定分数区间内的成员，分数从高到低排序</td></tr><tr><td><code>ZREVRANGEBYLEX key max min</code></td><td>返回有序集中指定字典区间内的成员，按字典顺序倒序</td></tr><tr><td><code>ZREVRANK key member</code></td><td>返回有序集合中指定成员的排名，有序集成员按分数值递减 (从大到小) 排序</td></tr><tr><td><code>ZINTERSTORE destination numkeys key1 [key2 ..]</code></td><td>计算给定的一个或多个有序集的交集并将结果集存储在新的有序集合 key 中，numkeys：表示参与运算的集合数，将 score 相加作为结果的 score</td></tr><tr><td><code>ZUNIONSTORE destination numkeys key1 [key2..]</code></td><td>计算给定的一个或多个有序集的交集并将结果集存储在新的有序集合 key 中</td></tr><tr><td><code>ZSCAN key cursor [MATCH pattern\] [COUNT count]</code></td><td>迭代有序集合中的元素（包括元素成员和元素分值）</td></tr></tbody></table>

**关于`ZRANGEBYLEX key min max [LIMIT offset count]`命令**

> **注意点：**
> 
> *   确保要搜索的成员 score 相同，否者影响结果。
> *   成员字符串作为二进制数组的字节数进行比较，按照 ASCII 字符集进行排序。
> *   默认情况下, “max” 和 “min” 参数前必须加 “`[`或`(`” 符号作为开头 ==(表示开闭和)==。后跟集合中成员，返回字典区间内的成员。
> *   min,max 也分别可以使用 `-` `+` 表示最小最大值
> *   LIMIT 是可选参数，可以用于对结果分页。

**所有获取成员的命令都可以在末尾加上 withscores 来附带显示 score**

**关于 ZINTERSTORE 和 ZUNIONSTORE 的参数 WEIGHTS 和 AGGREGATE`[WEIGHTS weight][AGGREGATE SUM|MIN|MAX]`**

> **注意点：**
> 
> *   WEIGHTS 参数后接 n 个 float 参数作为 n 个集合的**权重** 默认都是 1
> *   AGGREGATE 有三个可选参数：SUM|MIN|MAX 缺省值是 SUM
> 
> 这两个参数就是决定结果的 score 如何得出，每个结合的 score 权重占多少。
> 
> 例如：ZINTERSTORE setC 2 zsetA zsetB [WEIGHTS 1,2][AGGREGATE MIN]
> 
> 例中命令表示：zsetA 中 score 权重为 1 zsetB 权重为 2 加权后，取最小值作为结果的成员 score

```
----------------georadius---------------------
127.0.0.1:6379> GEORADIUS china:city 120 30 500 km withcoord withdist # 查询经纬度(120,30)坐标500km半径内的成员
1) 1) "hangzhou"
   2) "29.4151"
   3) 1) "120.20000249147415"
      2) "30.199999888333501"
2) 1) "shanghai"
   2) "205.3611"
   3) 1) "121.40000134706497"
      2) "31.400000253193539"
     
------------geohash---------------------------
127.0.0.1:6379> geohash china:city yichang shanghai # 获取成员经纬坐标的geohash表示
1) "wmrjwbr5250"
2) "wtw6ds0y300"
```

###   
四、三种特殊数据类型

#### Geospatial(地理位置)

> 使用经纬度定位地理坐标并用一个**有序集合 zset 保存**，所以 zset 命令也可以使用

<table><thead><tr><th>命令</th><th>描述</th></tr></thead><tbody><tr><td><code>geoadd key longitud(经度) latitude(纬度) member [..]</code></td><td>将具体经纬度的坐标存入一个有序集合</td></tr><tr><td><code>geopos key member [member..]</code></td><td>获取集合中的一个 / 多个成员坐标</td></tr><tr><td><code>geodist key member1 member2 [unit]</code></td><td>返回两个给定位置之间的距离。默认以米作为单位。</td></tr><tr><td><code>georadius key longitude latitude radius m|km|mi|ft [WITHCOORD][WITHDIST] [WITHHASH] [COUNT count]</code></td><td>以给定的经纬度为中心， 返回集合包含的位置元素当中， 与中心的距离不超过给定最大距离的所有位置元素。</td></tr><tr><td><code>GEORADIUSBYMEMBER key member radius...</code></td><td>功能与 GEORADIUS 相同，只是中心位置不是具体的经纬度，而是使用结合中已有的成员作为中心点。</td></tr><tr><td><code>geohash key member1 [member2..]</code></td><td>返回一个或多个位置元素的 Geohash 表示。使用 Geohash 位置 52 点整数编码。</td></tr></tbody></table>

**有效经纬度**

> *   有效的经度从 - 180 度到 180 度。
> *   有效的纬度从 - 85.05112878 度到 85.05112878 度。

指定单位的参数 **unit** 必须是以下单位的其中一个：

*   **m** 表示单位为米。
*   **km** 表示单位为千米。
*   **mi** 表示单位为英里。
*   **ft** 表示单位为英尺。

```
----------PFADD--PFCOUNT---------------------
127.0.0.1:6379> PFADD myelemx a b c d e f g h i j k # 添加元素
(integer) 1
127.0.0.1:6379> type myelemx # hyperloglog底层使用String
string
127.0.0.1:6379> PFCOUNT myelemx # 估算myelemx的基数
(integer) 11
127.0.0.1:6379> PFADD myelemy i j k z m c b v p q s
(integer) 1
127.0.0.1:6379> PFCOUNT myelemy
(integer) 11
 
----------------PFMERGE-----------------------
127.0.0.1:6379> PFMERGE myelemz myelemx myelemy # 合并myelemx和myelemy 成为myelemz
OK
127.0.0.1:6379> PFCOUNT myelemz # 估算基数
(integer) 17
```

测试: 1：![](https://i-blog.csdnimg.cn/blog_migrate/5ee5e64f5a3b55183c7307400995d23e.png)看来距离计算还是比较靠谱，还不是那么精确。

测试 2：957921.48 m

![](https://i-blog.csdnimg.cn/blog_migrate/525c0b767d2d3ab46415dc3b729f4fd1.png)

测试 3：958524.6022502825 m

![](https://i-blog.csdnimg.cn/blog_migrate/12589cce8679bf4ac8b8ddeafff487e3.png)

**关于 GEORADIUS 的参数**

> 通过`georadius`就可以完成 **附近的人**功能
> 
> withcoord: 带上坐标
> 
> withdist: 带上距离，单位与半径单位相同
> 
> COUNT n : 只显示前 n 个 (按距离递增排序)

```
------------setbit--getbit--------------
127.0.0.1:6379> setbit sign 0 1 # 设置sign的第0位为 1 
(integer) 0
127.0.0.1:6379> setbit sign 2 1 # 设置sign的第2位为 1  不设置默认 是0
(integer) 0
127.0.0.1:6379> setbit sign 3 1
(integer) 0
127.0.0.1:6379> setbit sign 5 1
(integer) 0
127.0.0.1:6379> type sign
string
 
127.0.0.1:6379> getbit sign 2 # 获取第2位的数值
(integer) 1
127.0.0.1:6379> getbit sign 3
(integer) 1
127.0.0.1:6379> getbit sign 4 # 未设置默认是0
(integer) 0
 
-----------bitcount----------------------------
127.0.0.1:6379> BITCOUNT sign # 统计sign中为1的位数
(integer) 4
```

####   
Hyperloglog(基数统计)

> Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定的、并且是很小的。
> 
> 花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基数。
> 
> 因为 HyperLogLog 只会根据输入元素来计算基数，而不会储存输入元素本身，所以 HyperLogLog 不能像集合那样，返回输入的各个元素。
> 
> 其底层使用 string 数据类型

**什么是基数？**

> 数据集中不重复的元素的个数。

**应用场景**

网页的访问量（UV）：一个用户多次访问，也只能算作一个人。

> 传统实现，存储用户的 id, 然后每次进行比较。当用户变多之后这种方式及其浪费空间，而我们的目的只是**计数**，Hyperloglog 就能帮助我们利用最小的空间完成。

<table><thead><tr><th>命令</th><th>描述</th></tr></thead><tbody><tr><td><code>PFADD key element1 [elememt2..]</code></td><td>添加指定元素到 HyperLogLog 中</td></tr><tr><td><code>PFCOUNT key [key]</code></td><td>返回给定 HyperLogLog 的基数估算值。</td></tr><tr><td><code>PFMERGE destkey sourcekey [sourcekey..]</code></td><td>将多个 HyperLogLog 合并为一个 HyperLogLog</td></tr></tbody></table>

```
127.0.0.1:6379> multi # 开启事务
OK
127.0.0.1:6379> set k1 v1 # 命令入队
QUEUED
127.0.0.1:6379> set k2 v2 # ..
QUEUED
127.0.0.1:6379> get k1
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED
127.0.0.1:6379> keys *
QUEUED
127.0.0.1:6379> exec # 事务执行
1) OK
2) OK
3) "v1"
4) OK
5) 1) "k3"
   2) "k2"
   3) "k1"
```

虽然其底层使用 String，但是 get 出来的值…  
![](https://i-blog.csdnimg.cn/blog_migrate/b98a0c8bb3515edd3644e7177a928c7a.png)

####   
BitMaps(位图)

> 使用位存储，信息状态只有 0 和 1
> 
> Bitmap 是一串连续的 2 进制数字（0 或 1），每一位所在的位置为偏移 (offset)，在 bitmap 上可执行 AND,OR,XOR,NOT 以及其它位操作。

**应用场景**

签到统计、状态统计

<table><thead><tr><th>命令</th><th>描述</th></tr></thead><tbody><tr><td><code>setbit key offset value</code></td><td>为指定 key 的 offset 位设置值</td></tr><tr><td><code>getbit key offset</code></td><td>获取 offset 位的值</td></tr><tr><td><code>bitcount key [start end]</code></td><td>统计字符串被设置为 1 的 bit 数，也可以指定统计范围按字节</td></tr><tr><td><code>bitop operration destkey key[key..]</code></td><td>对一个或多个保存二进制位的字符串 key 进行位元操作，并将结果保存到 destkey 上。</td></tr><tr><td><code>BITPOS key bit [start] [end]</code></td><td>返回字符串里面第一个被设置为 1 或者 0 的 bit 位。start 和 end 只能按字节, 不能按位</td></tr></tbody></table>

```
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> DISCARD # 放弃事务
OK
127.0.0.1:6379> EXEC 
(error) ERR EXEC without MULTI # 当前未开启事务
127.0.0.1:6379> get k1 # 被放弃事务中命令并未执行
(nil)
```

**bitmaps 的底层**

![](https://i-blog.csdnimg.cn/blog_migrate/a628fe8b34139c2bae534268b9dc109c.png)

这样设置以后你能 get 到的值是：**\xA2\x80**，所以 bitmaps 是一串从左到右的二进制串

###   
五、事务

Redis 的单条命令是保证原子性的，但是 redis 事务不能保证原子性

> Redis 事务：一组命令的集合。
> 
> 事务中每条命令都会被序列化，执行过程中按顺序执行，不允许其他命令进行干扰。
> 
> *   一次性
> *   顺序性
> *   排他性

所以事务中的命令在加入时都没有被执行，直到提交时才会开始执行 (Exec) 一次性完成。

操作过程：开启事务 (`multi`) >> 命令入队 >> 执行事务 (`exec`)

```
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> error k1 # 这是一条语法错误命令
(error) ERR unknown command `error`, with args beginning with: `k1`, # 会报错但是不影响后续命令入队 
127.0.0.1:6379> get k2
QUEUED
127.0.0.1:6379> EXEC
(error) EXECABORT Transaction discarded because of previous errors. # 执行报错
127.0.0.1:6379> get k1 
(nil) # 其他命令并没有被执行
```

取消事务 (`discurd`)

```
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> INCR k1 # 这条命令逻辑错误（对字符串进行增量）
QUEUED
127.0.0.1:6379> get k2
QUEUED
127.0.0.1:6379> exec
1) OK
2) OK
3) (error) ERR value is not an integer or out of range # 运行时报错
4) "v2" # 其他命令正常执行
```

####   
事务错误

> 代码语法错误（编译时异常）所有的命令都不执行

```
127.0.0.1:6379> set money 100 # 设置余额:100
OK
127.0.0.1:6379> set use 0 # 支出使用:0
OK
127.0.0.1:6379> watch money # 监视money (上锁)
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> DECRBY money 20
QUEUED
127.0.0.1:6379> INCRBY use 20
QUEUED
127.0.0.1:6379> exec # 监视值没有被中途修改，事务正常执行
1) (integer) 80
2) (integer) 20
```

> 代码逻辑错误 (运行时异常) ** 其他命令可以正常执行 **>> 不保证事务原子性

```
127.0.0.1:6379> watch money # money上锁
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> DECRBY money 20
QUEUED
127.0.0.1:6379> INCRBY use 20
QUEUED
127.0.0.1:6379> 	# 此时事务并没有执行
```

####   
监控

**悲观锁**：很悲观，认为什么时候都会出问题，无论做什么都加锁；

**乐观锁**：认为什么时候都不会出问题，所以不会上锁，更新数据的时候会去判断一下，在此期间是否有人修改过这个数据，获取 version，更新时比较 version；

使用`watch key`监控指定数据，相当于乐观锁加锁。

> 正常执行

```
127.0.0.1:6379> INCRBY money 500 # 修改了线程一中监视的money
(integer) 600
```

> 线程插队，修改监视值。

我们启动另外一个客户端模拟插队线程。

线程 1：

```
127.0.0.1:6379> EXEC 
(nil) # 没有结果，说明事务执行失败
 
127.0.0.1:6379> get money # 线程2 修改生效
"600"
127.0.0.1:6379> get use # 线程1事务执行失败，数值没有被修改
"0"
```

模拟线程插队，线程 2:

```
127.0.0.1:6379> UNWATCH # 解锁
OK
127.0.0.1:6379> WATCH money
OK
127.0.0.1:6379> multi # 重新进行事务
OK
127.0.0.1:6379> DECRBY money 200
QUEUED
127.0.0.1:6379> INCRBY use 200
QUEUED
127.0.0.1:6379> exec
1) (integer) 400
2) (integer) 200
```

回到线程 1，执行事务：

```
public class TestPing {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("192.168.52.134", 6379);
        String response = jedis.ping();
        System.out.println(response); // PONG
    }
}
```

> 解锁获取最新值，然后再加锁进行事务。
> 
> `unwatch`进行解锁。

```
public class TestTransaction {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("localhost", 6379);
        JSONObject jsonObject = new JSONObject();
        jsonObject.put("name", "sakura");
        jsonObject.put("msg", "hello world");
        String info = jsonObject.toJSONString();
        // 开启事务
        Transaction transaction = jedis.multi();
        try {
            transaction.set("user1", info);
            transaction.set("user2", info);
            // 执行事务
            transaction.exec();
        } catch (Exception e) {
            e.printStackTrace();
            // 如果出错则放弃事务
            transaction.discard();
        } finally {
            System.out.println(jedis.get("user1"));
            System.out.println(jedis.get("user2"));
            // 关闭连接
            jedis.close();
        }
    }
}
```

注意：每次提交执行 exec 后都会自动释放锁，不管是否成功

###   
六、Jedis

使用 Java 来操作 Redis，Jedis 是 Redis 官方推荐使用的 Java 连接 redis 的客户端。

1.  依赖导入: jedis、fastjson
    
    ```
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis</artifactId>
    </dependency>
    ```
    
2.  连接数据库
    
    > 连接远程：
    > 
    > 开放端口 6379
    > 
    > ```
    > spring.redis.host=127.0.0.1
    > spring.redis.port=6379
    > # 以上俩个属性都有默认值 可以省略
    >  
    > # 默认使用 db0
    > spring.redis.database=0
    > ```
    > 
    > *   1
    > 
    > 需要修改配置文件：
    > 
    > 1.  修改 bind IP，默认是 127.0.0.1 也就是 localhost, 只有本机可用。
    >     
    >     修改为 0.0.0.0 或者 本机的 ip 地址。
    >     
    > 
    > ![](https://i-blog.csdnimg.cn/blog_migrate/d9402c63b1415cb5c077cfbc55ff9f2a.png)
    > 
    > 1.  关闭保护模式
    >     
    >     默认是 yes, 修改为 no
    >     
    > 
    > ![](https://i-blog.csdnimg.cn/blog_migrate/e3c1d3fa22e8f50d0db563d4f1af741c.png)
    > 
    > 然后以此配置文件启动 redis-server
    
    TestPing.java
    
    ```
    @Autowired
    RedisTemplate redisTemplate;
    ```
    
    **Java 中操作 Redis 就需要 一个 Jedis 对象**，创建 Jedis 对象，需要主机号和端口号作为参数。
    
    其中的 API 与使用 Redis 原生的命令是一样的。
    

事务：

```
@Autowired
RedisTemplate redisTemplate;
 
@Test
void contextLoads() {
   // 获取Redis连接对象
   RedisConnection connection = redisTemplate.getConnectionFactory().getConnection();
   // 清空数据库
   connection.flushDb();
   System.out.println(redisTemplate.keys("*"));
 
   ValueOperations operations = redisTemplate.opsForValue();
   // set
   operations.set("name", "sakura");
   // get
   System.out.println(operations.get("name"));
   // 事务支持开启
   redisTemplate.setEnableTransactionSupport(true);
   redisTemplate.multi();
   try {
      operations.set("msg", "hello world");
      System.out.println(operations.get("name"));
      // 事务执行
      redisTemplate.exec();
   } catch (Exception e) {
      e.printStackTrace();
      // 放弃事务
      redisTemplate.discard();
   } finally {
      // 关闭连接
      connection.close();
   }
}
```

> #### 博客（重点）：[redis 缓存在项目中的使用 - 小虾米的 java 梦 - 博客园](https://www.cnblogs.com/fengli9998/p/6755591.html "redis缓存在项目中的使用 - 小虾米的java梦 - 博客园")

1、写逻辑代码的时候，在业务功能处，从缓存中读取 ----- 从 db 中读取 ---- 将数据写入缓存。

2、缓存同步的原理：就是将 redis 中的 key 进行删除，下次访问的时候，redis 中没有改数据，则从 DB 进行查询，再次更新到 redis 中。我们可以写一个缓存同步的服务，只需要在后台进行 CRUD 的地方添加调用该缓存同步的服务即可

###   
七、SpringBoot 整合 Redis

需要的依赖项, 在创建项目时选择，或者手动加入。

```
@Configuration
public class RedisConfig {
 
   @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) throws UnknownHostException {
        // 将template 泛型设置为 <String, Object>
        RedisTemplate<String, Object> template = new RedisTemplate();
        // 连接工厂，不必修改
        template.setConnectionFactory(redisConnectionFactory);
        /*
         * 序列化设置
         */
        // key、hash的key 采用 String序列化方式
        template.setKeySerializer(RedisSerializer.string());
        template.setHashKeySerializer(RedisSerializer.string());
        // value、hash的value 采用 Jackson 序列化方式
        template.setValueSerializer(RedisSerializer.json());
        template.setHashValueSerializer(RedisSerializer.json());
        template.afterPropertiesSet();
        
        return template;
    }
}
```

springboot 2.x 后 ，原来使用的 Jedis 被 lettuce 替换。

> Jedis：采用直连，多线程操作不安全。
> 
> Lettuce：底层采用 Netty

![](https://i-blog.csdnimg.cn/blog_migrate/1d946c48782e0fa7b1d3e034051b0800.png)

![](https://i-blog.csdnimg.cn/blog_migrate/e00f5146f831f5dfb394ed3cc3d02607.png)

我们在学习 SpringBoot 自动配置的原理时，整合一个组件并进行配置一定会有一个自动配置类 xxxAutoConfiguration, 并且在 spring.factories 中也一定能找到这个类的完全限定名。Redis 也不例外。  
![](https://i-blog.csdnimg.cn/blog_migrate/80256eb9c57eada23000bed23010cb8d.png)

那么就一定还存在一个 RedisProperties 类  
![](https://i-blog.csdnimg.cn/blog_migrate/9237e6e4942837df07493c44a4c64f8e.png)

之前我们说 SpringBoot2.x 后默认使用 Lettuce 来替换 Jedis，现在我们就能来验证了。

先看 Jedis:

![](https://i-blog.csdnimg.cn/blog_migrate/f3e346f980f06dbe0069a8036aaa501a.png)

@ConditionalOnClass 注解中有两个类是默认不存在的，所以 Jedis 是无法生效的

然后再看 Lettuce：

![](https://i-blog.csdnimg.cn/blog_migrate/d1cdae0328d65ae3cde06f987d69c2a9.png)

完美生效。

现在我们回到 RedisAutoConfiguration

![](https://i-blog.csdnimg.cn/blog_migrate/4d1e3a9f57fbaa3b19f7b791098d124c.png)

只有两个简单的 Bean

*   RedisTemplate
*   StringRedisTemplate

当看到 xxTemplate 时可以对比 RestTemplat、SqlSessionTemplate, 通过使用这些 Template 来间接操作组件。那么这俩也不会例外。分别用于操作 Redis 和 Redis 中的 String 数据类型。

在 RedisTemplate 上也有一个条件注解，说明我们是可以对其进行定制化的

说完这些，我们需要知道如何编写配置文件然后连接 Redis，就需要阅读 RedisProperties  
![](https://i-blog.csdnimg.cn/blog_migrate/217fd937334dfd6f81d875bf28dcf818.png)

这是一些基本的配置属性。  
![](https://i-blog.csdnimg.cn/blog_migrate/64d7363834ce49dd555d7225c83ac7a3.png)

还有一些连接池相关的配置。注意使用时一定使用 Lettuce 的连接池。  
![](https://i-blog.csdnimg.cn/blog_migrate/d3e502fdb192a233b4cbda73af2f5803.png)

配置文件:(基本不用写)

```
bind node1  # 绑定的ip
protected-mode yes   # 保护模式 默认开启
port 6379 # 端口设置
```

测试使用：

1.  我们说过现在操作 Redis 需要用到 RedisTemplate，所以需要自动注入。
    
    ```
    daemonize yes # 以守护进程的方式运行 默认no  我们需要手动修改yes ！
     
    pidfile /var/run/redis_6379.pid # 如果以后台的方式运行，我们就需要指定一个pid文件
     
    # 日志
    # Specify the server verbosity level.
    # This can be one of:
    # debug (a lot of information, useful for development/testing)
    # verbose (many rarely useful info, but not a mess like the debug level)
    # notice (moderately verbose, what you want in production probably)  # 生产环境用
    # warning (only very important / critical messages are logged)
    loglevel notice
    logfile "" # 日志的文件位置名
    databases 16 # 数据库数量，默认16个
    always-show-logo yes  # 是否总是显示logo  那个魔方一样的
    ```
    
2.  使用 RedisTemplate
    

![](https://i-blog.csdnimg.cn/blog_migrate/83367f2d69c09e3142d0e890438cd728.png)

在操作 Redis 中数据时，需要先通过 opsForxxx() 方法来获取对应的 Operations, 然后才可以操作数据。

1.  以 ValueOperations（操作 String）为例

![](https://i-blog.csdnimg.cn/blog_migrate/7c140056115cc72546def0f323c5b627.png)

```
save 900 1  # 如果900秒15分钟内 至少1个key修改 我们就进行持久化操作
save 300 10 # 如果300秒5分钟内 至少10个key修改 我们就 进行持久化操作
save 60 10000 # 如果60秒内 至少10000个key修改(高迸发) 进行持久化操作
# 我们之后学习持久化，会自己定义这个测试
 
stop-writes-on-bgsave-error yes # 持久化出错了，是否还要继续工作，默认 要！
 
rdbcompression yes # 是否压缩rdb文件，需要消耗cpu资源
 
rdbchecksum yes # 保存reb文件的时候，进行错误校验，错误的话修复
 
dir ./ # rdb文件保存的目录，默认当前目录下
```

此时我们回到 Redis 查看数据时候，惊奇发现全是乱码，可是程序中可以正常输出：  
![](https://i-blog.csdnimg.cn/blog_migrate/2b1e27775d13a5de9671463e73ca83dc.png)

这时候就关系到存储对象的序列化问题，在网络中传输的对象也是一样需要序列化，否者就全是乱码。

我们转到看那个默认的 RedisTemplate 内部什么样子：

![](https://i-blog.csdnimg.cn/blog_migrate/5059b62bd2c3477fe621109e2af94fa1.png)

在最开始就能看到几个关于序列化的参数。

默认的序列化器是采用 JDK 序列化器

![](https://i-blog.csdnimg.cn/blog_migrate/a39974a7181a5d2f471c0d8f9be1f2fd.png)

而默认的 RedisTemplate 中的所有序列化器都是使用这个序列化器：

![](https://i-blog.csdnimg.cn/blog_migrate/db44e5b401a5266fc204000564636fc7.png)

后续我们定制 RedisTemplate 就可以对其进行修改。

**`RedisSerializer`** 提供了很多种序列化方案：

*   我们可以直接调用 RedisSerializer 的方法来返回序列化器，然后 set

![](https://i-blog.csdnimg.cn/blog_migrate/48d201b14c6622d2e887ff8f7a1c47f7.png)

*   也可以自己 new 相应的实现类，然后 set

![](https://i-blog.csdnimg.cn/blog_migrate/0903cd9f1c5bf1f81d4766a54a863639.png)

**定制 RedisTemplate 的模板：**

我们创建一个 Bean 加入容器，就会触发 RedisTemplate 上的条件注解使默认的 RedisTemplate 失效。

```
requirepass foobared  # 默认密码为空 如果需要设置123则:requirepass 123  
# 不过我们更多的用命令去设置密码
 
node1:6379> config set requirepass "123456" # 设置密码123456
OK
node1:6379> config get requirepass  # 发现所有命令没权限了
(error) NOAUTH Authentication required.
node1:6379> ping
(error) NOAUTH Authentication required.
node1:6379> auth 123456  # 使用密码进行登陆
OK
node1:6379> config get requirepass  # 获取redis密码
1) "requirepass"
2) "123456"
```

这样一来，只要实体类进行了序列化，我们存什么都不会有乱码的担忧了。

![](https://i-blog.csdnimg.cn/blog_migrate/621d040bbd03d9f04e034d7cb1dc9569.png)

###   
八、自定义 Redis 工具类

使用 RedisTemplate 需要频繁调用`.opForxxx`然后才能进行对应的操作，这样使用起来代码效率低下，工作中一般不会这样使用，而是将这些常用的公共 API 抽取出来封装成为一个工具类，然后直接使用工具类来间接操作 Redis, 不但效率高并且易用。

工具类参考博客：

[https://www.cnblogs.com/zeng1994/p/03303c805731afc9aa9c60dbbd32a323.html](https://www.cnblogs.com/zeng1994/p/03303c805731afc9aa9c60dbbd32a323.html "https://www.cnblogs.com/zeng1994/p/03303c805731afc9aa9c60dbbd32a323.html")

[https://www.cnblogs.com/zhzhlong/p/11434284.html](https://www.cnblogs.com/zhzhlong/p/11434284.html "https://www.cnblogs.com/zhzhlong/p/11434284.html")

Redis.conf 详解
-------------

redis.conf 文件位置参见上面博客

可以参考博客：[redis.conf 配置详细解析 - 太清 - 博客园 - # redis 配置文件示例 # 当你需要为某个配置项指定内存大小的时候，必须要带上单位， # 通常的格式就是 1k 5gb 4m 等酱紫： # # 1k => 1000 bytes # 1![](https://i-blog.csdnimg.cn/blog_migrate/003a2ce7eb50c2e24a8c624c260c5930.png)https://www.cnblogs.com/kreo/p/4423362.html](https://www.cnblogs.com/kreo/p/4423362.html "redis.conf配置详细解析 - 太清 - 博客园")

**配置文件 unit 单位 对大小写不敏感**![](https://i-blog.csdnimg.cn/blog_migrate/1d5307bd0c321c17955a61e247d1c333.png)

**包含 INCLUDES**

可以把多个配置文件都导入进来，(？？为高可用做准备是吗？？？)。![](https://i-blog.csdnimg.cn/blog_migrate/d5f2ffc546d61edeede36f2f72190ff9.png)

**网络 NETWORK**

```
maxclients 10000 # 设置能连接上redis的最大客户端的数量
maaxmemory <bytes> # redis 配置最大的内存容量
maxmemory-policy noeviction # 内存达到上限之后的处理策略
```

  
**通用 GENERAL**

```
appendonly no  #默认是不开启aof模式的，默认是使用rdb方式持久化的，在大部分所有的情况下，rdb完全够用！
appendfilename "appendonly.aof"  # 持久化的文件的名字
 
# appendfsync always  # 每次修改都会 sync 消耗性能
appendfsync everysec  # 每秒执行一次 sync，可能会丢失这一秒的数据
# appendfsync no      # 不执行 sync 操作系统自己同步数据，速度最快，但一般也不用。
```

  
**快照 SNAPSHOTTING**

持久化，在规定的时间内， 执行了多少次操作，就会持久化到文件. rdb.aof

redis 是内存数据库，如果没有持久化，那么数据断电即失。

```
node1:6379> CONFIG GET dir
1) "dir" 
2) "/export/server/redis-3.2.8-bin/datas" # 如果在这个目录下存在dump.rdb文件，启动就会自动恢复其中的数据
```

  
**复制 REPLICATION**

​ 我们后面讲主从复制的时候讲

**安全：SECURITY**

可以在这里设置 redis 密码，默认是没有密码的

```
auto-aof-rewrite-min-size 64mb
auto-aof-rewrite-percentage 100
```

![](https://i-blog.csdnimg.cn/blog_migrate/512be11c82ca08a9121c4267f02a0398.png)

**限制 CLIENTS**

```
# 开启AOF持久化方式
appendonly yes
 
# AOF持久化文件名
appendfilename "appendonly.aof"
 
# 每秒把缓冲区的数据同步到磁盘
appendfsync everysec
 
# 数据持久化文件存储目录
dir /var/lib/redis
 
# 是否在执行重写时不同步数据到AOF文件 默认为no
# 这里的 yes，就是执行重写时不同步数据到AOF文件
no-appendfsync-on-rewrite yes
 
# 触发AOF文件执行重写的最小尺寸
auto-aof-rewrite-min-size 64mb
 
# 触发AOF文件执行重写的增长率
auto-aof-rewrite-percentage 100
```

**一、最大内存设置**  
redis 默认的最大的内存设置为 0，相当于基于物理机的最大值  
**二、淘汰策略**  
**1. 8 种策略**  
volatile-lru，针对设置了过期时间的 key，使用 lru 算法进行淘汰。  
allkeys-lru，针对所有 key 使用 lru 算法进行淘汰。  
volatile-lfu，针对设置了过期时间的 key，使用 lfu 算法进行淘汰。  
allkeys-lfu，针对所有 key 使用 lfu 算法进行淘汰。  
volatile-random，从所有设置了过期时间的 key 中使用随机淘汰的方式进行淘汰。  
allkeys-random，针对所有的 key 使用随机淘汰机制进行淘汰。  
volatile-ttl，删除生存时间最近的一个键。  
noeviction(默认)，不删除键，值返回错误。

主要是 4 种算法，针对不同的 key, 形成的策略。  
    算法：

     lru 最近很好的使用的 key（根据时间，最不常用的淘汰）  
     lfu 最近很好的使用的 key (根据计数器，用的次数最少的 key 淘汰)  
     random 最难淘汰  
     ttl 快要过期的先淘汰  
key ：

     volatile 有过期的时间的那些 key  
      allkeys 所有的 key  
 

**2. 内存淘汰算法的具体工作原理是：**  
客户端执行一条新命令，导致数据库需要增加数据（比如 set key value）  
Redis 会检查内存使用，如果内存使用超过 maxmemory，就会按照置换策略删除一些 key  
新的命令执行成功

  
**APPEND ONLY 模式 aof 配置**

```
# 订阅端
node1:6379> SUBSCRIBE xiaoyin  # 订阅1个频道，频道名xiaoyin
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "xiaoyin"
3) (integer) 1
# 等待读取推送信息
1) "message"  # 消息
2) "xiaoyin"  # 来自哪个频道
3) "jiayou"	  # 消息内容             
 
# 发布端
node1:6379> PUBLISH xiaoyin "jiayou" # 发布者发布消息"jiayou" 到频道"xiaoyin"
(integer) 1
```

十、持久化——RDB
----------

**RDB：Redis Databases**

Redis 是内存数据库，如果不将内存中的数据库状态保存到磁盘，一旦服务器进程退出，服务器中的数据库状态也会消失，所以 Redis 提供了持久化功能。

RDB（Redis DataBase）  
什么是 RDB

主从复制中，RDB 是备用的。在从机上面，不占用主机内存。而 AOF 基本不用，![](https://i-blog.csdnimg.cn/blog_migrate/27ac8d5a459a683046ce68dfdab879b4.png)

**在指定的时间间隔内将内存中的数据集快照写入磁盘，也就是行话讲的 Snapshot 快照，它恢复时是将快照文件直接读到内存里。**

Redis 会单独创建（fork）一个子进程来进行持久化，会先将数据写入到一个临时文件中，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。整个过程中，主进程是不进行任何 IO 操作的。这就确保了极高的性能。如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那 RDB 方式要比 AOF 方式更加的高效。RDB 的缺点是最后一次持久化后的数据可能丢失。

我们默认的就是 RDB，一般情况下不需要修改这个配置。

有时候在生产环境我们会将 dump.rdb 进行备份

rdb 保存的文件是 dump.rdb 都是在我们的配置文件中快照中配置的![](https://i-blog.csdnimg.cn/blog_migrate/f63e34870d1c2bf8c7f7e49628dee17b.png)

触发机制

1、save 的规则满足的情况下，会自动触发 rdb 规则

2、执行 flushall 命令，也会触发我们的 rdb 规则！  
3、退出 redis，也会产生 rdb 文件！

备份就自动生成一个 dump.rdb 文件

如何恢复 rdb 文件！

1、只需要将 rdb 文件放在我们 redis 启动目录就可以，redis 启动的时候会自动检查 dump.rdb 恢复其中的数据！  
2、查看需要存在的位置

```
node1:6379> info replication  # 查看当前库的信息
# Replication
role:master 		# 角色 master
connected_slaves:0  # 没有从机
master_repl_offset:0
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_o
```

  
**几乎就他自己默认的配置就够用了，但是我们还是需要去学习！**

优点：

1. 大规模的数据恢复

2. 对数据的完整性要不高！

缺点：

1. 需要一定的时间间隔进程操作！(当然你也可以自己去修改配置)。如果 redis 意外宕机了，这个最后一次修改数据就没有的了！

2.fork 进程的时候，会占用一定的内存空间！！

**bgsave 和 save 对比**

![](https://i-blog.csdnimg.cn/blog_migrate/c6e25c5a96851810991a4a8142a76d15.jpeg)

 ![](https://i-blog.csdnimg.cn/blog_migrate/c2226d6a4a1e016893831e1bd3cc220a.jpeg)

<table><thead><tr><th>命令</th><th>save</th><th>bgsave</th></tr></thead><tbody><tr><td>IO 类型</td><td>同步</td><td>异步</td></tr><tr><td>阻塞？</td><td>是</td><td>是（阻塞发生在 fock()，通常非常快）</td></tr><tr><td>复杂度</td><td>O(n)</td><td>O(n)</td></tr><tr><td>优点</td><td>不会消耗额外的内存</td><td>不阻塞客户端命令</td></tr><tr><td>缺点</td><td>阻塞客户端命令</td><td>需要 fock 子进程，消耗内存</td></tr></tbody></table>

####   
十一、持久化——AOF

####   
什么是 AOF

快照功能（RDB）并不是非常耐久（durable）： 如果 Redis 因为某些原因而造成故障停机， 那么服务器将丢失最近写入、以及未保存到快照中的那些数据。 从 1.1 版本开始， Redis 增加了一种完全耐久的持久化方式： AOF 持久化。

如果要使用 AOF，需要修改配置文件：

![](https://i-blog.csdnimg.cn/blog_migrate/5b69c0f82046b7460101a4e934a6a995.png)

`appendonly no yes`则表示启用 AOF

####   
工作原理

每当 Redis 执行一个改变数据集的命令时（比如 SET）， 这个命令就会被追加到 AOF 文件的末尾。这样的话， 当 Redis 重新启时， 程序就可以通过重新执行 AOF 文件中的命令来达到重建数据集的目的。

**创建**

![](https://i-blog.csdnimg.cn/blog_migrate/26bf51352ccdb7350e7b0c6f512dca85.jpeg)

**恢复**

![](https://i-blog.csdnimg.cn/blog_migrate/2785e91b702a1350618992caaaf62986.jpeg)

####   
AOF 持久化的三种策略

![](https://i-blog.csdnimg.cn/blog_migrate/71949d847b313ff162092c5ee3b71ee7.png)

  
always

每次有新命令追加到 AOF 文件时就执行一次同步，安全但是速度慢

![](https://i-blog.csdnimg.cn/blog_migrate/8aa9e5e6070555fe938427eb568d785d.jpeg)

  
everysec(default)

这种 fsync 策略可以兼顾速度和安全性，可能丢失一秒的数据。

![](https://i-blog.csdnimg.cn/blog_migrate/cee0cb194a88e63791b72c104ebb044e.jpeg)

  
no

将数据交给操作系统来处理，由操作系统来决定什么时候同步数据。更快，但是不安全。

![](https://i-blog.csdnimg.cn/blog_migrate/6cde296137657e5523be2af1080edc56.jpeg)

**三者对比**

<table><thead><tr><th>命令</th><th>优点</th><th>缺点</th></tr></thead><tbody><tr><td>always</td><td>不丢失数据</td><td>IO 开销大，一般 SATA 磁盘只有几百 TPS</td></tr><tr><td>everysec</td><td>每秒进行与 fsync，最多丢失 1 秒数据</td><td>可能丢失 1 秒数据</td></tr><tr><td>no</td><td>不用管</td><td>不可控</td></tr></tbody></table>

####   
aof 文件修复

当 aof 被人为破坏，redis 就无法完成启动，可以通过官方提供的 redis-check-aof 工具对 aof 文件进行修复, 当然数据可能发生部分丢失。

```
# 步骤：
# 在一台虚拟机上，克隆3个窗口，复制3份conf文件
 
[root@node1 redis-3.2.8-bin]# cd /export/server/redis-3.2.8-bin
[root@node1 redis-3.2.8-bin]# cp redis.conf redis79.conf 
[root@node1 redis-3.2.8-bin]# cp redis.conf redis80.conf   
[root@node1 redis-3.2.8-bin]# cp redis.conf redis81.conf  
 
命令：vim redis79.conf
port 6379 # 84行  端口这里不用改 本身这个文件就是79端口  
daemonize yes # 128行 后台运行 要yes
pidfile /var/run/redis_6379.pid # 150行 这里pid file 也没问题
logfile "/export/server/redis-3.2.8-bin/logs/redis6379.log" # 163行 logfile加上后缀6379
dbfilename dump6379.rdb # 236行 dump.rdb改成 dump6379.rdb 否则会重名
 
# 至此第一个就保存好了。
# 第二个文件对应的改成6380和6381
```

####   
AOF 重写

因为 AOF 的运作方式是不断地将命令追加到文件的末尾， 所以随着写入命令的不断增加， AOF 文件的体积也会变得越来越大。

举个例子， 如果你对一个计数器调用了 100 次 INCR ， 那么仅仅是为了保存这个计数器的当前值， AOF 文件就需要使用 100 条记录（entry）。然而在实际上， 只使用一条 SET 命令已经足以保存计数器的当前值了， 其余 99 条记录实际上都是多余的。

为了处理这种情况， Redis 支持一种有趣的特性： 可以在不打断服务客户端的情况下， 对 AOF 文件进行重建（rebuild）。执行 bgrewriteaof 命令， Redis 将生成一个新的 AOF 文件， 这个文件包含重建当前数据集所需的最少命令。

Redis 2.2 需要自己手动执行 bgrewriteaof 命令； Redis 2.4+ 则可以**通过配置自动触发 AOF 重写。**

  
AOF 重写的作用

*   减少磁盘占用量
*   加速数据恢复

  
AOF 重写的实现方式

*   `bgrewriteaof` 命令
    
    用于异步执行一个 AOF（AppendOnly File）文件重写操作。重写会创建一个当前 AOF 文件的体积优化版本。  
    即使 bgrewriteaof 执行失败，也不会有任何数据丢失，因为旧的 AOF 文件在 bgrewriteaof 成功之前不会被修改。
    
    AOF 重写由 Redis 自行触发，bgrewriteaof 仅仅用于手动触发重写操作。
    

  
AOF 重写的配置

<table><thead><tr><th>配置名</th><th>含义</th></tr></thead><tbody><tr><td>auto-aof-rewrite-min-size</td><td>触发 AOF 文件执行重写的最小尺寸</td></tr><tr><td>auto-aof-rewrite-percentage</td><td>触发 AOF 文件执行重写的增长率</td></tr></tbody></table>

<table><thead><tr><th>统计名</th><th>含义</th></tr></thead><tbody><tr><td>aof_current_size</td><td>AOF 文件当前尺寸（字节）</td></tr><tr><td>aof_base_size</td><td>AOF 文件上次启动和重写时的尺寸（字节）</td></tr></tbody></table>

触发自动重写有两个条件：

*   当前 AOF 文件大小超过触发重写的最小大小。
    
    即 _aof_current_size > auto-aof-rewrite-min-size_
    
*   当前文件大小增长率超过触发重写的增长率
    
    即 **(a o f _ c u r r e n t _ s i z e − a o f _ b a s e _ s i z e) a o f _ b a s e _ s i z e > a u t o − a o f − r e w r i t e − p e r c e n t a g e {(aof\_current\_size-aof\_base\_size) \over aof\_base\_size}>auto-aof-rewrite-percentageaof_base_size(aof_current_size−aof_base_size)​>auto−aof−rewrite−percentage**
    

例如：

假设 Redis 的配置项为：

```
# 窗口1 
[root@node1 redis-3.2.8-bin]# redis-server redis79.conf 
[root@node1 redis-3.2.8-bin]# redis-cli -h node1 -p 6379
 
# 窗口2 
[root@node1 redis-3.2.8-bin]# redis-server redis80.conf 
[root@node1 redis-3.2.8-bin]# redis-cli -h node1 -p 6380
 
# 窗口3 
[root@node1 redis-3.2.8-bin]# redis-server redis81.conf 
[root@node1 redis-3.2.8-bin]# redis-cli -h node1 -p 6381
```

当

1.  AOF 文件的体积大于 64Mb
2.  并且 AOF 文件的体积比上一次重写之久的体积大了至少一倍（100%）时

Redis 将执行 bgrewriteaof 命令进行重写。

  
相关配置

```
#  在从机中查看信息
node1:6380> SLAVEOF node1 6379 # SLAVEOF host 6379  找谁当自己的老大
OK
node1:6380> info replication 
# Replication
role:slave  # 当前角色的从机
master_host:node1  # 可以看到主机的信息
master_port:6379
master_link_status:up
master_last_io_seconds_ago:0
master_sync_in_progress:0
slave_repl_offset:15
slave_priority:100
slave_read_only:1
connected_slaves:0
master_repl_offset:0
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
 
#  在主机中查看信息
node1:6379> info replication
# Replication
role:master
connected_slaves:2  # 多了从机的配置
slave0:ip=192.168.88.161,port=6380,state=online,offset=253,lag=1  
slave1:ip=192.168.88.161,port=6381,state=online,offset=253,lag=1
master_repl_offset:267
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:2
repl_backlog_histlen:266
```

  
重写过程

![](https://i-blog.csdnimg.cn/blog_migrate/ba07586759558f565828468a0bf85409.jpeg)

1.  执行 bgrewriteaof 命令
2.  父进程 fork 出子进程
3.  将命令写入缓存，然后由缓存写入旧 aof 文件
4.  子进程创建新的 aof 临时文件
5.  1.  子进程通知父进程开始重写 aof 文件
    2.  将命令从缓存写入新 aof 文件
    3.  重写完成使用新 aof 文件替换旧的 aof 文件。

####   
AOF 的缺点

1.  对于相同的数据集来说，AOF 文件的体积通常要大于 RDB 文件的体积。
2.  根据所使用的 fsync 策略，AOF 的速度可能会慢于 RDB 。 在一般情况下， 每秒 fsync 的性能依然非常高， 而关闭 fsync 可以让 AOF 的速度和 RDB 一样快， 即使在高负荷之下也是如此。
3.  数据量较大时，恢复较慢

####   
AOF 优点

1.  使用 AOF 会让你的 Redis 更加耐久: 你可以使用不同的 fsync 策略：无 fsync，每秒 fsync，每次写的时候 fsync。使用默认的每秒 fsync 策略，Redis 的性能依然很好 (fsync 是由后台线程进行处理的，主线程会尽力处理客户端请求)，一旦出现故障，你最多丢失 1 秒的数据。
2.  AOF 文件是一个只进行追加的日志文件，所以不需要写入 seek，即使由于某些原因 (磁盘空间已满，写的过程中宕机等等) 未执行完整的写入命令，你也也可使用 redis-check-aof 工具修复这些问题。
3.  Redis 可以在 AOF 文件体积变得过大时，自动地在后台对 AOF 进行重写： 重写后的新 AOF 文件包含了恢复当前数据集所需的最小命令集合。 整个重写操作是绝对安全的，因为 Redis 在创建新 AOF 文件的过程中，会继续将命令追加到现有的 AOF 文件里面，即使重写过程中发生停机，现有的 AOF 文件也不会丢失。 而一旦新 AOF 文件创建完毕，Redis 就会从旧 AOF 文件切换到新 AOF 文件，并开始对新 AOF 文件进行追加操作。
4.  AOF 文件有序地保存了对数据库执行的所有写入操作， 这些写入操作以 Redis 协议的格式保存， 因此 AOF 文件的内容非常容易被人读懂， 对文件进行分析（parse）也很轻松。 导出（export） AOF 文件也非常简单： 举个例子， 如果你不小心执行了 FLUSHALL 命令， 但只要 AOF 文件未被重写， 那么只要停止服务器， 移除 AOF 文件末尾的 FLUSHALL 命令， 并重启 Redis ， 就可以将数据集恢复到 FLUSHALL 执行之前的状态。

###   
十二、RDB 和 AOF 选择

####   
RDB 和 AOF 对比

<table><thead><tr><th></th><th>RDB</th><th>AOF</th></tr></thead><tbody><tr><td>启动优先级</td><td>低</td><td>高</td></tr><tr><td>体积</td><td>小</td><td>大</td></tr><tr><td>恢复速度</td><td>快</td><td>慢</td></tr><tr><td>数据安全性</td><td>丢数据</td><td>根据策略决定</td></tr></tbody></table>

一、形式不同

1、rdb：rdb 在指定的时间间隔内将内存中的数据集快照写入磁盘，实际操作过程是 fork 一个子进程，先将数据集写入临时文件，写入成功后，再替换之前的文件，用二进制压缩存储。

2、aof：aof 以日志的形式记录服务器所处理的每一个写、删除操作，查询操作不会记录，以文本的方式记录，可以打开文件看到详细的操作记录。

二、启动效率不同

1、rdb：通过 fork 子进程来协助完成数据持久化工作的，因此，如果当数据集较大时，可能会导致整个服务器停止服务几百毫秒，甚至是 1 秒钟。

2、aof：由子进程完成这些持久化的工作，可以极大地避免服务进程执行 IO 操作。如果数据集很大，aof 的启动效率会更高。

三、安全性不同

1、rdb：系统一旦在定时持久化之前出现宕机现象，此前没有来得及写入磁盘的数据都将丢失。

2、aof：由于该机制对日志文件的写入操作采用的是 append 模式，因此在写入过程中即使出现宕机现象，也不会破坏日志文件中已经存在的内容。

####   
如何选择使用哪种持久化方式？     答：RDB 比较常用

一般来说， 如果想达到足以媲美 PostgreSQL 的数据安全性， 你应该同时使用两种持久化功能。

如果你非常关心你的数据， 但仍然可以承受数分钟以内的数据丢失， 那么你可以只使用 RDB 持久化。

有很多用户都只使用 AOF 持久化， 但并不推荐这种方式： 因为定时生成 RDB 快照（snapshot）非常便于进行数据库备份， 并且 RDB 恢复数据集的速度也要比 AOF 恢复的速度要快。

  
Redis 发布订阅
-------------

Redis 发布订阅（pub/sub）是一种消息通信模式：发送者（pub）发送消息，订阅者（sub）接收消息。微信、微博、关注系统！  
Redis 客户端可以订阅任意数量的频道。  
订阅 / 发布消息图：

![](https://i-blog.csdnimg.cn/blog_migrate/31190b5aab6f298b0d8d28976e6789b8.png)

下图展示了频道 channel1，以及订阅这个频道的三个客户端——client2、client5 和 client1 之间的关系：

![](https://i-blog.csdnimg.cn/blog_migrate/abac6b8af378d523b7ca2db93fdb1a0e.png)

当有新消息通过 PUBLISH 命令发送给频道 channel1 时，这个消息就会被发送给订阅它的三个客户端：

![](https://i-blog.csdnimg.cn/blog_migrate/c388493e76791a1bb2c9b9bd2c1f7881.png)

> 命令

这些命令被广泛用于构建即时通信应用，比如网络聊天室（chatroom）和实时广播、实时提醒等。

![](https://i-blog.csdnimg.cn/blog_migrate/591330702ecc91af6a9c06cf7524ef02.png)

> 测试

```
6379窗口：
​		set k6 v6
​	6380和6381窗口：
​		都可以正常get k6
```

> 原理

Redis 是使用 C 实现的，通过分析 Redis 源码里的 pubsub.c 文件，了解发布和订阅机制的底层实现，籍此加深对 Redis 的理解。

微信：

Redis 通过 PUBLISH、SUBSCRIBE 和 PSUBSCRIBE 等命令实现发布和订阅功能。

通过 SUBSCRIBE 命令订阅某频道后，redis-server 里维护了一个字典，字典的键就是一个个 channel(频道)，而字典的值则是一个链表，链表中保存了所有订阅这个 channel 的客户端。SUBSCRIBE 命令的关键，就是将客户端添加到给定 channel 的订阅链表中。

通过 PUB 凵 SH 命令向订阅者发送消息，redis-server 会使用给定的频道作为键，在它所维护的 channel 字典中查找记录了订阅这个频道的所有客户端的链表，遍历这个链表，将消息发布给所有订阅者。

Pub/Sub 从字面上理解就是发布（Publish）与订阅（Subscribe），在 Redis 中你可以设定对某一个 key 值进行消息发布及消息订阅，当一个 key 值上进行了消息发布后，所有订阅它的客户端都会收到相应的消息。这一功能最明显的用法是用作实时消息系统，比如普通的即时聊天，群聊等功能。

![](https://i-blog.csdnimg.cn/blog_migrate/4de6c6ddb367aa6ebb7fa83f387898ed.png)

使用场景：

​ 1. 实时消息系统

​ 2. 实时聊天（频道当做聊天室，将消息回显给所有人即可）

​ 3. 订阅、关注的系统都可以。

稍微复杂的场景我们就会使用 消息中间件 MQ ()

  
  
Redis 主从复制
----------------

1 主人 – 2 仆从

### 概念

主从复制，是指将一台 Redis 服务器的数据，复制到其他的 Redis 服务器。前者称为主节点（master/leader)，后者称为从节点 (slave/follower)；**数据的复制是单向的，只能由主节点到从节点**。Master 以写为主，Slave 以读为主。

**默认情况下，每台 Redis 服务器都是主节点** (因为你还没有配置主从)；

且一个主节点可以有多个从节点（或没有从节点），但一个从节点只能有一个主节点。

**主从复制的作用主要包括**

1、**数据冗余**：主从复制实现了数据的热备份，是持久化之外的一种数据冗余方式。

2、**故障恢复**：当主节点出现问题时，可以由从节点提供服务，实现快速的故障恢复；实际上是一种服务的冗余。

3、**负载均衡**：在主从复制的基础上，配合读写分离，可以由主节点提供写服务，由从节点提供读服务（即写 Redis 数据时应用连接主节点，读 Redis 数据时应用连接从节点），分担服务器负载；尤其是在写少读多的场景下，通过多个从节点分担读负载，可以大大提高 Redis 服务器的并发量。

4、**高可用 (集群) 基石**：除了上述作用用以外，主从复制还是哨兵和集群能够实施的基础，因此说主从复制是 Redis 高可用的基础。

一般来说，要将 Redis 运用于工程项目中，只使用一台 Redis 是万万不能的 (可能宕机，1 主 2 从)，原因如下：

1、从结构上，单个 Redis 服务器会发生单点故障，并且一台服务器需要处理所有的请求负载，压力较大

2、从容量上，单个 Redis 服务器内存容量有限，就算一台 Redis 服务器内存容量为 256G，也不能将所有内存用作 Redis 存储内存

一般来说，**单台 Redis 最大使用内存不应该超过 20G**。

电商网站上的商品，一般都是一次上传，无数次浏览的，说专业点也就是 "多读少写"。

对于这种场景，我们可以使如下这种架构：

![](https://i-blog.csdnimg.cn/blog_migrate/7204a6a4e9ed78d401b84889157c4268.png)

主从复制，读写分离！80% 的情况下都是在进行读操作！减缓服务器压力。架构中经常使用，最低 1 主 2 从。

只要在公司中，主从复制就是必须要用的，因为真实项目中不可能单机使用 Redis！

###   
环境配置（单机多集群）

只配置从库，不用配置主库。

```
# 窗口1 
[root@node1 redis-3.2.8-bin]# redis-server redis79.conf 
[root@node1 redis-3.2.8-bin]# redis-cli -h node1 -p 6379
 
 
# 窗口2 
[root@node1 redis-3.2.8-bin]# redis-server redis80.conf 
[root@node1 redis-3.2.8-bin]# redis-cli -h node1 -p 6380
[root@node1 redis-3.2.8-bin]# SLAVEOF node1 6379
 
# 窗口3 
[root@node1 redis-3.2.8-bin]# redis-server redis81.conf 
[root@node1 redis-3.2.8-bin]# redis-cli -h node1 -p 6381
[root@node1 redis-3.2.8-bin]# SLAVEOF node1 6380
```

注意我们接下来配置的是单机多集群，就是一台虚拟机上开多个窗口，实现多集群的意义，正常我们是应该配置多机多集群的，这样效果会好点。

复制 3 个配置文件，然后修改对应的信息

1、端口

2、pid 名字

3、log 文件名字

4、dump.rdg 名字

```
sentinel monitor myredis node1 6379 1
# 哨兵    监视     被监控名称:自定义   主机名  端口名port  1代表主机挂了，slave投票看谁成为主机，票数最多的就会成为主机
```

修改完毕之后，在三个窗口分别启动我们 3 个 redis 服务器，可以通过进程查看。（注意我们是一台虚拟机的 3 个窗口上）

```
[root@node1 bin]# redis-sentinel ../sentinel.conf    #启动哨兵
 
 
27661:X 26 Nov 18:37:58.600 * Increased maximum number of open files to 10032 (it was originally set to 1024).
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 3.2.8 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                   
 (    '      ,       .-`  | `,    )     Running in sentinel mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 26379
 |    `-._   `._    /     _.-'    |     PID: 27661
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           http://redis.io        
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               
 
27661:X 26 Nov 18:37:58.600 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
27661:X 26 Nov 18:37:58.601 # Sentinel ID is b289eec7e609a7cad25fcb5e9891931fc10afc15
27661:X 26 Nov 18:37:58.601 # +monitor master myredis 192.168.88.161 6379 quorum 1  # 当前监控主机是6379
27661:X 26 Nov 18:37:58.601 * +slave slave 192.168.88.161:6381 192.168.88.161 6381 @ myredis 192.168.88.161 6379  # 从机是6381
27661:X 26 Nov 18:37:58.602 * +slave slave 192.168.88.161:6380 192.168.88.161 6380 @ myredis 192.168.88.161 6379  # 从机是6380
```

![](https://i-blog.csdnimg.cn/blog_migrate/c230850559bbc8aaf0555c0ee08856b0.png)

> ### **一主二从**

默认情况下，每台 redis 服务器都是主节点。(因为你还没有配置主从)。

一般情况下，我们只配置从机就可以了。

认老大，一主（用 79），二从（用 80 和 81）

> info replication

```
1.在6379主机，set k8 v8，然后shutdown exit退出
2.在6380 和 6381从机 分别：info replication ，发现目前依旧是：slave模式，不要急 ，观察下哨兵日志
3.哨兵日志：
 
27661:X 26 Nov 18:42:03.730 # +sdown master myredis 192.168.88.161 6379
27661:X 26 Nov 18:42:03.731 # +odown master myredis 192.168.88.161 6379 #quorum 1/1
27661:X 26 Nov 18:42:03.731 # +new-epoch 1
27661:X 26 Nov 18:42:03.731 # +try-failover master myredis 192.168.88.161 6379
27661:X 26 Nov 18:42:03.732 # +vote-for-leader b289eec7e609a7cad25fcb5e9891931fc10afc15 1
27661:X 26 Nov 18:42:03.732 # +elected-leader master myredis 192.168.88.161 6379
27661:X 26 Nov 18:42:03.732 # +failover-state-select-slave master myredis 192.168.88.161 6379
27661:X 26 Nov 18:42:03.823 # +selected-slave slave 192.168.88.161:6380 192.168.88.161 6380 @ myredis 192.168.88.161 6379
27661:X 26 Nov 18:42:03.823 * +failover-state-send-slaveof-noone slave 192.168.88.161:6380 192.168.88.161 6380 @ myredis 192.168.88.161 6379
27661:X 26 Nov 18:42:03.924 * +failover-state-wait-promotion slave 192.168.88.161:6380 192.168.88.161 6380 @ myredis 192.168.88.161 6379
27661:X 26 Nov 18:42:04.427 # +promoted-slave slave 192.168.88.161:6380 192.168.88.161 6380 @ myredis 192.168.88.161 6379
27661:X 26 Nov 18:42:04.427 # +failover-state-reconf-slaves master myredis 192.168.88.161 6379
27661:X 26 Nov 18:42:04.482 * +slave-reconf-sent slave 192.168.88.161:6381 192.168.88.161 6381 @ myredis 192.168.88.161 6379
27661:X 26 Nov 18:42:05.443 * +slave-reconf-inprog slave 192.168.88.161:6381 192.168.88.161 6381 @ myredis 192.168.88.161 6379
27661:X 26 Nov 18:42:05.443 * +slave-reconf-done slave 192.168.88.161:6381 192.168.88.161 6381 @ myredis 192.168.88.161 6379
27661:X 26 Nov 18:42:05.496 # +failover-end master myredis 192.168.88.161 6379  # 故障转移6379
27661:X 26 Nov 18:42:05.496 # +switch-master myredis 192.168.88.161 6379 192.168.88.161 6380
27661:X 26 Nov 18:42:05.496 * +slave slave 192.168.88.161:6381 192.168.88.161 6381 @ myredis 192.168.88.161 6380
27661:X 26 Nov 18:42:05.496 * +slave slave 192.168.88.161:6379 192.168.88.161 6379 @ myredis 192.168.88.161 6380
27661:X 26 Nov 18:42:35.513 # +sdown slave 192.168.88.161:6379 192.168.88.161 6379 @ myredis 192.168.88.161 6380  # 发现6379已经sdown了，选举6380作为master
 
4.再次观察6380和6381，就会发现6380变成了master
```

![](https://i-blog.csdnimg.cn/blog_migrate/338757eba9c7f354faee15ebd06eb5a9.png)

两个从机都配置完了 就有 2 个从机的配置

真实环境中的主从配置应该在配置文件中配置，这样的话是永久的，我们这里使用的是命令，是暂时的。

![](https://i-blog.csdnimg.cn/blog_migrate/c426c9ae53a37cf762af35095a466bb0.png)

![](https://i-blog.csdnimg.cn/blog_migrate/a743988e8a59c37c42762c3fd4b43482.png)

配置文件配置主机即可，那么当前机器就是从机了，配置：replicaof 主机 ip 主机端口 ，如果主机有密码，配置 masterauth 主机密码

> 细节了解

主机可以写；从机不能写只能读。

**主机中的所有信息和数据，都会自动被从机保存**。

如果在从机中使用命令 set 值，会报错： (error) READONLY You can’t write against a read only slave；  翻译：你不能写，只能读，你仅仅只是一个从机

![](https://i-blog.csdnimg.cn/blog_migrate/181a20adfae0f32c4d8cc0a6162ad3a7.png)

*   主节点崩了，就不能写了。但是你从节点还在，还能读到主节点的内容！
    
    *   不信我们可以对主机进行 shutdown exit，再在从机上进行读取
        *   这还不是真正的高可用
            *   在你没有配置哨兵模式的情况下，主机宕机了，从机还是 salve 状态，不会自动选举。
        *   真正的高可用是：
            *   你主机宕机了，在剩下的 2 台从机中，应该给我选出 1 台主机来
*   命令行模式下（如果修改配置文件就是永久的了）：
    
    *   主机宕机了，再登录回来，主机再次写入的信息，从机依旧可以读取到主机写的信息！
    *   从机宕机了，再重新连接，默认是新的主机，拿不到原主机在断开期间写的数据。
        *   如果此时你再次连接主机变成从机，立马就可以获取主机的所有数据。使用命令让 node1 6379 当主节点：SLAVEOF node1 6379
    
    > 复制原理：全量复制和增量复制
    
    SIave 启动成功连接 master 后会发送一个 sync（同步）命令
    
    Master 接到命令，启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令，在后台进程执行完毕之后，**master 将传送整个数据文件到 slave，并完成一次完全同步**。
    
    *   **全量复制**：而 slave 服务在接收到数据库文件数据后，将其存盘并加载到内存中。
        
        *   主机 set k1 v1，从机断开，主机 set k2 v2，从机重新连接主机，全量复制，从机还可以 get k2。
    *   **增量复制**：Master 继续将新的所有收集到的修改命令依次传给 slave，完成同步
        
        *   主机连接主机后，主机 set k3 v3，从机可以 get k3，这就是增量复制
    
    但是**只要是重新连接 master，一次完全同步（全量复制）将被自动执行**。我们的数据一定可以在从机中看到。
    

----------------------------------------------------------------------------------------------------------------------------

> ### 层层链路模式

![](https://i-blog.csdnimg.cn/blog_migrate/4ca72ded57e7175a8d9335890c1803b9.png)

上面这种模式有个缺点， 当 6379 主机 Master 挂了之后，整个集群就只能读，不能写了。

我们现在来想一个新的模式，让 6381 认 6380 做老大

​ 即：上一个主节点，连接下一个从节点

也可以的完成主从复制：

​ 这个时候在 6380 窗口 2 输入：info replication

```
6379窗口：
​		set k6 v6
​	6380和6381窗口：
​		都可以正常get k6
```

```
# 窗口1 
[root@node1 redis-3.2.8-bin]# redis-server redis79.conf 
[root@node1 redis-3.2.8-bin]# redis-cli -h node1 -p 6379
 
 
# 窗口2 
[root@node1 redis-3.2.8-bin]# redis-server redis80.conf 
[root@node1 redis-3.2.8-bin]# redis-cli -h node1 -p 6380
[root@node1 redis-3.2.8-bin]# SLAVEOF node1 6379
 
# 窗口3 
[root@node1 redis-3.2.8-bin]# redis-server redis81.conf 
[root@node1 redis-3.2.8-bin]# redis-cli -h node1 -p 6381
[root@node1 redis-3.2.8-bin]# SLAVEOF node1 6380
```

![](https://i-blog.csdnimg.cn/blog_migrate/18abd0fc0854ca07e808792c97e75cfc.png)

![](https://i-blog.csdnimg.cn/blog_migrate/9e760427b42a13aa0a5c3702c33cea47.png)

> 问题：如果 6379 老大断开了，这个时候能不能选择一个老大出来呢？

​ 在哨兵模式没有出来之前，我们都是手动设置的。

​ 如果主机断开了连接，我们可以使用 **SLAVEOF no one** ，来让自己变成主机！其他节点就可以手动连接到这个最新的节点（手动）！

​ 如果这个时候老大 6379 恢复了，也只是光杆司令了。除非手动让 6380 再次跟从 6379.

![](https://i-blog.csdnimg.cn/blog_migrate/554a58d7d7cd4fe1b7a8ea590a1a1af7.png)

  
哨兵模式
-------

（自动选举老大的模式）

> 概述

主从切换技术的方方法是 “当主服务器宕机后需要手动把一台从服务器切换为主服务器，这就需要人工干预，费事费力，还会造成一段时间内服务不可用。这不是一种推荐的方式，更多时候，我们优先考虑哨兵模式。Redis 从 2.8 开始正式提提供了 sentinel（哨兵）架构解决这个问题。

谋朝篡位的自动版，能够后台监控主机是否故障，如果故障了根据投票数**自动将从库转换为主库**。

哨兵模式是一种特殊的模式，首先 Redis 提供了哨兵的命令，哨兵是一个独立的进程，作为进程，它会独立运行。其原理是**哨兵通过发送命令，等待 Redis 服务器响应，从而监控运行的多个 Redis 实例**。

![](https://i-blog.csdnimg.cn/blog_migrate/0ce6239cf84f202cb682091095678d0b.png)

哨兵模式文件：redis-sentinel

![](https://i-blog.csdnimg.cn/blog_migrate/5770bae4ff851b9fa152f2b7a3894044.png)

这里的哨兵有两个作用：

*   通过发送命令，让 Redis 服务器返回监控其运行状态，包括主服务器和从服务器。
    
*   当哨兵监测到 master 宕机，会自动将 slave 切换成 master，然后通过发布订阅模式通知其他的从服务器，修改配置文件，让它们切换主机。
    

然而一个哨兵进程对 Redis 服务器进行监控，可能会出现问题，为此，我们可以使用多个哨兵进行监控。各个哨兵之间还会进行监控，这样就形成了**多哨兵模式**。

![](https://i-blog.csdnimg.cn/blog_migrate/679cc632ebd1163a75a538dda6266fee.png)

假设主服务器宕机哨兵 1 先检测到这个结果，系统并不会马上进行 **failover** 过程，仅仅是哨兵 1 主观的认为主服务器

不可用，这个现象成为**主观下线**。当后面的哨兵也检测到主服务器不可用，并且数量达到一定值时，那么哨兵之间

就会进行一次投票，投票的结果由一个哨兵发起，进行 **failover**[故障转移] 操作。切换成功后，就会通过发布订阅

模式，让各个哨兵把自己监控的从服务器实现切换主机，这个过程称为**客观下线**。

> 测试

我们目前的状态是 1 主 2 从

1. 配置哨兵配置文件

哨兵模式有非常多，我们只写一个最简单的。

vim sentinel.conf

```
sentinel monitor myredis node1 6379 1
# 哨兵    监视     被监控名称:自定义   主机名  端口名port  1代表主机挂了，slave投票看谁成为主机，票数最多的就会成为主机
```

2. 启动哨兵

```
[root@node1 bin]# redis-sentinel ../sentinel.conf    #启动哨兵
 
 
27661:X 26 Nov 18:37:58.600 * Increased maximum number of open files to 10032 (it was originally set to 1024).
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 3.2.8 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                   
 (    '      ,       .-`  | `,    )     Running in sentinel mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 26379
 |    `-._   `._    /     _.-'    |     PID: 27661
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           http://redis.io        
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               
 
27661:X 26 Nov 18:37:58.600 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
27661:X 26 Nov 18:37:58.601 # Sentinel ID is b289eec7e609a7cad25fcb5e9891931fc10afc15
27661:X 26 Nov 18:37:58.601 # +monitor master myredis 192.168.88.161 6379 quorum 1  # 当前监控主机是6379
27661:X 26 Nov 18:37:58.601 * +slave slave 192.168.88.161:6381 192.168.88.161 6381 @ myredis 192.168.88.161 6379  # 从机是6381
27661:X 26 Nov 18:37:58.602 * +slave slave 192.168.88.161:6380 192.168.88.161 6380 @ myredis 192.168.88.161 6379  # 从机是6380
```

我们现在测试主机崩了会怎么样

```
1.在6379主机，set k8 v8，然后shutdown exit退出
2.在6380 和 6381从机 分别：info replication ，发现目前依旧是：slave模式，不要急 ，观察下哨兵日志
3.哨兵日志：
 
27661:X 26 Nov 18:42:03.730 # +sdown master myredis 192.168.88.161 6379
27661:X 26 Nov 18:42:03.731 # +odown master myredis 192.168.88.161 6379 #quorum 1/1
27661:X 26 Nov 18:42:03.731 # +new-epoch 1
27661:X 26 Nov 18:42:03.731 # +try-failover master myredis 192.168.88.161 6379
27661:X 26 Nov 18:42:03.732 # +vote-for-leader b289eec7e609a7cad25fcb5e9891931fc10afc15 1
27661:X 26 Nov 18:42:03.732 # +elected-leader master myredis 192.168.88.161 6379
27661:X 26 Nov 18:42:03.732 # +failover-state-select-slave master myredis 192.168.88.161 6379
27661:X 26 Nov 18:42:03.823 # +selected-slave slave 192.168.88.161:6380 192.168.88.161 6380 @ myredis 192.168.88.161 6379
27661:X 26 Nov 18:42:03.823 * +failover-state-send-slaveof-noone slave 192.168.88.161:6380 192.168.88.161 6380 @ myredis 192.168.88.161 6379
27661:X 26 Nov 18:42:03.924 * +failover-state-wait-promotion slave 192.168.88.161:6380 192.168.88.161 6380 @ myredis 192.168.88.161 6379
27661:X 26 Nov 18:42:04.427 # +promoted-slave slave 192.168.88.161:6380 192.168.88.161 6380 @ myredis 192.168.88.161 6379
27661:X 26 Nov 18:42:04.427 # +failover-state-reconf-slaves master myredis 192.168.88.161 6379
27661:X 26 Nov 18:42:04.482 * +slave-reconf-sent slave 192.168.88.161:6381 192.168.88.161 6381 @ myredis 192.168.88.161 6379
27661:X 26 Nov 18:42:05.443 * +slave-reconf-inprog slave 192.168.88.161:6381 192.168.88.161 6381 @ myredis 192.168.88.161 6379
27661:X 26 Nov 18:42:05.443 * +slave-reconf-done slave 192.168.88.161:6381 192.168.88.161 6381 @ myredis 192.168.88.161 6379
27661:X 26 Nov 18:42:05.496 # +failover-end master myredis 192.168.88.161 6379  # 故障转移6379
27661:X 26 Nov 18:42:05.496 # +switch-master myredis 192.168.88.161 6379 192.168.88.161 6380
27661:X 26 Nov 18:42:05.496 * +slave slave 192.168.88.161:6381 192.168.88.161 6381 @ myredis 192.168.88.161 6380
27661:X 26 Nov 18:42:05.496 * +slave slave 192.168.88.161:6379 192.168.88.161 6379 @ myredis 192.168.88.161 6380
27661:X 26 Nov 18:42:35.513 # +sdown slave 192.168.88.161:6379 192.168.88.161 6379 @ myredis 192.168.88.161 6380  # 发现6379已经sdown了，选举6380作为master
 
4.再次观察6380和6381，就会发现6380变成了master
```

如果 Master 节点断开了，这个时候就会从从机中随机选择一个服务器（这里面有一个投票算法）！

![](https://i-blog.csdnimg.cn/blog_migrate/997f9b55a803618a90630449f677acc1.png)

现在选出了新的主机，你就算把 6379 连回来，也没用了，因为已经选出了 6380 作为新的老大，6379 是光杆司令了。

这里有个有趣的点：

*   当你 6379 主机刚连接回来的时候，info replication，这时候 6379 是 master 主机
    
*   观察哨兵日志，发现开始转换
    

```
27661:X 26 Nov 18:48:51.189 * +convert-to-slave slave 192.168.88.161:6379 192.168.88.161 6379 @ myredis 192.168.88.161 6380
```

*   这时候才看 6379，info replication，发现 6379 已经是 slave 了
    
*   再看 6380，info replication，发现 6380 的 connected_slaves: 从 1 变成了 2，也就是说 6379 恢复连接后，被转换成了新的 Master6380 的从机了
    

如果主机此时回来了，只能归并到新的主机下，当做从机，这就是哨兵模式的规则！

> 哨兵模式

优点：

1、哨兵集群，基于主从复制模式，所有的主从配置优点，他全有。

2、主从可以切换，故障可以转换，系统的可用性就会更好

3、哨兵模式就是主从模式的升级，手动到自动，更加健壮！

缺点：

1、Redis 不好在线扩容的，集群容量一旦到达上限，在线扩容就会非常麻烦！

2、实现哨兵模式的配置其实是很麻烦的，里面有很多选择！（我们这是这是开了一个最简单的哨兵模式）

> 哨兵模式的全部配置

​ 这些配置一般是由运维来处理的

![](https://i-blog.csdnimg.cn/blog_migrate/fbfe11daf6c496d99b6c6a7be6d84262.png)

![](https://i-blog.csdnimg.cn/blog_migrate/21798aed398eb70d2b21ab18fdaae1a8.png)

![](https://i-blog.csdnimg.cn/blog_migrate/7dbdb36fd5a26ddf8e8043b3a8138441.png)

![](https://i-blog.csdnimg.cn/blog_migrate/b3bd51563ed66e95cd45e96a1afde0f6.png)

![](https://i-blog.csdnimg.cn/blog_migrate/d311012d1c4a24e6e5ae50f90ab4e9fd.png)

  
Redis 缓存的穿透、击穿、雪崩 (面试高频 工作常用)
--------------------------------

这三个都是服务器高可用需要面临的问题！

这里我们不会详细的去分析解决方案的底层 (未来开专题讲解底层)

Redis 缓存的使用，极大的提升了应用程序的性能和效率，特别是数据查询方面。但同时，它也带来了一些问题。其中，最要害的问题，就是数据的一致性问题，从严格意义上讲，这个问题无解。如果对数据的一致性要求很高，那么就不能使用缓存。

另外的一些典型问题就是，缓存穿透、缓存雪崩和缓存击穿。目前，业界也都有比较流行的解决方案。

场景：秒杀

###   
缓存穿透 (查不到)

> 概念

缓存穿透的概念很简单，用户想要査询一个数据，发现 redis 内存数据库没有，也就是缓存没有命中，于是向持久层数据库査询。发现也没有，于是本次査询失败。当用户很多的时候，缓存都没有命中，于是都去请求了持久层数据库。这会给持久层数据库造成很大的压力，这时候就相当于出现了缓存穿透。

![](https://i-blog.csdnimg.cn/blog_migrate/65e0b210ddc5eb86fee6813f2cea012d.png)

> 解决方案

**1、布隆过滤器：**

**布隆过滤器是一种数据结构**，对所有可能査询的参数以 hash 形式存储，在控制层先进行校验，不符合则丟弃，从而避免了对底层存储系统的查询压力；

一般情况下，先查询 Redis 缓存，如果 Redis 中没有，再查询 MySQL。当数据库中也不存在这条数据时，每次查询都要访问数据库，这就是缓存穿透。

**在 Redis 前面添加一层布隆过滤器，请求先在布隆过滤器中判断，如果布隆过滤器不存在时，直接返回，**不再反问 Redis 和 MySQL。

如果布隆过滤器中存在时，再访问 Redis，再访问数据库。

[Redis 布隆过滤器的原理和应用场景，解决缓存穿透 - 知乎](https://zhuanlan.zhihu.com/p/623313954 "Redis布隆过滤器的原理和应用场景，解决缓存穿透 - 知乎")

![](https://i-blog.csdnimg.cn/blog_migrate/b5f765bcd9c6c097e701748d75309623.png)

布隆过滤器实现原理及源码解析：

**2、缓存空对象**

当存储层不命中后，即使返回的空对象也将其缓存起来，同时会设置一个过期时间，之后再访问这个数据将会从缓存中获取，保护了后端数据源

![](https://i-blog.csdnimg.cn/blog_migrate/05ab3d716cebe72fdc52f2741b061ad5.png)

但是这种方法存在 2 个问题：

1、如果空值能够被缓存起来，这就意味着缓存需要更多的空间存储更多的键，因为这当中可能会有很多的空值的键

2、即使对空值设置了过期时间，还是会存在缓存层和存储层的数据会有一段时间窗口的不一致，这对于需要保持一致性的业务会有影响。

###   
缓存击穿 (量太大，缓存过期！)

微博热搜，服务器宕机 (60 秒过期，60.1 秒回复，这 0.1 秒的间隔 ==> 瞬间全部砸在 mysql 服务器上)

> 概述

这里需要注意和缓存击穿的区别，缓存击穿，是指一个 key 非常热点，在不停的扛着大并发，大并发集中对这一个点进行访问，当这个 key 在失效的瞬间，持续的大并发就穿破缓存，直接请求数据库，就像在一个屏障上凿开了一个洞。

当某个 key 在过期的瞬间，有大量的请求并发访问，这类数据一般是热点数据，由于缓存过期，会同时访问数据库来查询最新数据，并且回写缓存，会导使数据库瞬间压力过大。

> 解决方案

**设置热点数据永不过期**

从缓存层面来看，没有设置过期时间，所以不会出现热点 key 过期后产生的问题。

**加互斥锁**

分布式锁：使用分布式锁，保证对于每个 key 同时只有一个线程去査询后端服务，其他线程没有获得分布式锁的权限，因此只需要等待即可。这种方式将高并发的压力转移到了分布式锁，因此对分布式锁的考验很大。

![](https://i-blog.csdnimg.cn/blog_migrate/c919f35b3d4a243da0466f17ea2cc80e.png)

###   
缓存雪崩

> 概念

缓存雪崩，是指在某一个时间段，缓存集中过期失效。或者 Redis 宕机 (停电了)！

产生雪崩的原因之一，比如在写本文的时候，马上就要到双十二零点，很快就会迎来一波抢购，这波商品时间比较集中的放入了缓存，假设缓存一个小时。那么到了凌晨一点钟的时候，这批商品的缓存就都过期了。而对这批商品的访问查询，都落到了数据库上，对于数据库而言，就会产生周期性的压力波峰。于是所有的请求都会达到存储层，存储层的调用量会暴増，造成存储层也会挂掉的情况。

![](https://i-blog.csdnimg.cn/blog_migrate/a6da8636e178d066dab5fa0413f9ae42.png)

其实集中过期，倒不是非常致命，比较致命的缓存雪崩，是缓存服务器某个节点宕机或断网。因为自然形成的缓存雪崩，一定是在某个时间段集中创建缓存，这个时候，数据库也是可以顶住压力的。无非就是对数据库产生周期性的压力而已。而缓存服务节点的宕机，对数据库服务器造成的压力是不可预知的，很有可能瞬间就把数据库压垮。

双十一：停掉一些服务，来保证主要的服务可用！

> 解决方案

**redis 高可用**

这个思想的含义是，既然 redis 有可能挂掉，那我多增设几台 redis，这样一台挂掉之后其他的还可以继续工作，其实就是搭建的集群。（异地多活！）

**限流降级**

这个解决方案的思想是，在缓存失效后，通过加锁或者队列来控制读数据库写缓存的线程数量。比如对某个 key 只允许一个线程査询数据和写缓存，其他线程等待。

**数据预热**

数据加热的含义就是在正式部署之前，我先把可能的数据先预先访闩 - 遍，这样部分可能大量访问的数据就会加载到缓存中。在即将发生大并发访问前手动触发加载缓存不同的 key，设置不同的过期时间，让缓存失效的时间点尽量均匀

数据预热方案：

缓存预热就是系统启动前, 提前将相关的缓存数据直接加载到缓存系统。避免在用户请求的时候, 先查询数据库, 然后再将数据缓存的问题! 用户直接查询实现被预热的缓存数据。

**加载缓存思路：**

1、数据量不大，可以在项目启动的时候自动进行加载， 利用 @PostConstruct 注解实现启动时先执行  
2、利用定时任务刷新缓存，将数据库的数据刷新到缓存中

遇到的问题：
------

[关于使用 Redisson 客户端无法获取 Redis 数据, 取值为 null 的调查记录_redisson 获取到值为 null-CSDN 博客](https://blog.csdn.net/ankeway/article/details/118520241 "关于使用Redisson客户端无法获取Redis数据,取值为null的调查记录_redisson 获取到值为null-CSDN博客")