# Redis 学习

Redis 是Key-value数据库，支持主从同步，数据存在内存，性能非常卓越

## 配置文件

- 只允许本机访问，绑定bind 120.0.0.1
- 运行所有机器访问，绑定bing 0.0.0.0
- 默认端口：6379

## redis 数据库持久化

数据保存在dump.rdb下

defilename dump.rdp

```redis
save 900 1
save 300 10
save 60 10000
```

关于保存策略：

- 默认900秒/15分钟，如果至少有一个key被改变，同步一次
- 300秒/5分钟，如果至少有10个key 被改变，同步一次
- 60秒/一分钟，如果至少有1000个Key被改变，同步一次

保存方式：RDB保存和AOF保存

###RDB:

- （保存最终操作的结果，最终态）如有一千条数据，数据改变了，但是还是一千条，没有改变。就直接把现有的一千条再保存一遍，覆盖

AOF：

- （保存执行过程）数据改变是有一系列语句执行导致的，保存执行的语句，当需要持久化时，把保存的语句执行一遍就可以了

## Redis 常用命令

```
操作字符串
set key value
get key 

操作list
lpush list a
lpush list b
lrange list 0 2
lremove list a
> lpush mylist 1 2 3
(integer) 3
> exists mylist
(integer) 1
> lpop mylist
"3"
> lpop mylist
"2"
> lpop mylist
"1"
> exists mylist
(integer) 0

操作set
> sadd myset 1 2 3
(integer) 3
> smembers myset
1. 3
2. 1
3. 2
> sismember myset 3
(integer) 1
> sismember myset 30
(integer) 0

操作hash
> hmset user:1000 username antirez birthyear 1977 verified 1
OK
> hget user:1000 username
"antirez"
> hget user:1000 birthyear
"1977"
> hgetall user:1000
1) "username"
2) "antirez"
3) "birthyear"
4) "1977"
5) "verified"
6) "1"

交集
> sinter tag:1:news tag:2:news tag:10:news tag:27:news
... results here ...
并集
> sunionstore game:1:deck deck
(integer) 52
```

## List上的阻塞操作

可以使用Redis来实现生产者和消费者模型，如使用LPUSH和RPOP来实现该功能。但会遇到这种情景：list是空，这时候消费者就需要轮询来获取数据，这样就会增加redis的访问压力、增加消费端的cpu时间，而很多访问都是无用的。为此redis提供了阻塞式访问 [BRPOP](http://www.redis.cn/commands/brpop.html) 和 [BLPOP](http://www.redis.cn/commands/blpop.html) 命令。 消费者可以在获取数据时指定如果数据不存在阻塞的时间，如果在时限内获得数据则立即返回，如果超时还没有数据则返回null, 0表示一直阻塞。

同时redis还会为所有阻塞的消费者以先后顺序排队。

##Redis数据结构：

1. 字符串

   ```c
   struct sdhdr{
       //已经保存的字符串的长度
       int len;
       //记录buf数组中未使用字节的数量
       int free;
       //字节数组，用于保存字符串
       char buf[];
   }
   ```

   字符串指令：

   get //获取存储在指定键中的值

   set //设置存储在指定键中的值

   del //删除存储在指定键中的值

2. List 链表

   链表上的内个节点都包含一个字符串，从串的两端可以推入或者弹出元素。根据偏移量对链表进行修剪，读取单个或者对个元素。

   每个链表的节点结构：

   ```c
   typedef struct listNode{
       //前置节点
       struct listNode *prev;
       //后置节点
       struct listNode *next;
       //节点的值
       void *value;
   }listNode;
   ```

   持有链表的结构

   ```c
   typedef struct list{
       //表头节点
       listNode *head;
       //表未节点
       listNode *tail;
       //节点所包含的节点数目
       unsigned long len;
       //节点复制函数
       void （*dup）(void *ptr);
       //节点值释放函数
       void （*free）(void * ptr);
       //节点值对比函数
       int (*match)（void *ptr,void *key）;
   }list;
   
   ```

   链表的命令：

   1. rpush //将给定的值推入列表的右端
   2. lrange //获取列表指定范围的所有的值
   3. lindex //获取列表在指定范围上的单个元素
   4. lpop //从列表的左端弹出一个值，并返回被弹出的值

3. set字典

   Redis 的字典使用哈希表作为你底层实现，一个哈希表可以有多个哈希表节点，而每个哈希表节点就保存了字典中的一个键值对

   Redis字典锁使用的哈希表结构

   ```c
   typedef struct dictht{
       //哈希表数组
       dictEntry **table;
       //哈希表大小
       unsigned long size;
       //哈希表大小掩码，用于计算索引值
       //总是等与size-1;
       unsigned long sizemask;
       //该哈希表已有节点的数量
       unsigned long used;
   }dictht;
   ```

   哈希表节点结构

   ```c
   typedef struct dictEntry{
       //键
       void *key;
       //值
       union{
        	void *val;
           uint64_tu64;
           int64_ts64;
       ｝v;
       //指向下一个哈希节点，形成链表
       struct dictEntrty *next;
   }dictEntry;
   ```

   持有哈希表的结构

   ```c
   typedef struct dict{
       //类型特定函数
       dictType *type;
       //稀有数据
       void *privdata;
       //哈希表
       dictht ht[2];
       //rehash索引
       //rehash不进行时使用值为-1
       in trehashidx;
   }dict;
   ```

   集合的指令：

   sadd //给指定的元素添加到集合中、

   smembers //返回几个包含的所有元素

   sismember //检查指定元素是否存在于集合中

   srem //检查指定元素是否存在于集合中，那么移除这个元素

4. 过期键删除策略

   1. 定时删除:在设置键的过期时间的同时，创建一个定时器，让定时器在键的过期时间来临时，立即执行对键的删除操作
   2. 惰性删除：放任键过期不管，但是每次从键空间获取键时，都检查出的键的过期时间，如果过期的话就删除给键，如果没有过期就返回该键
   3. 定期删除：每隔一段时间，程序就对数据库进行一次检查，删除里面过期的键。至于一次删除多少键，扫描多少数据库由算法决定

   在这三次策略中：第一种和第三种是主动删除，第二种是被动删除。Redis 服务器实际上使用的是惰性删除和定期删除两种策略，通过配合两种策略，服务器可以很好的合理使用CPU时间和避免内存浪费之间取得平衡。