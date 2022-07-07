### [1、为什么 Redis 需要把所有数据放到内存中？](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%98%EF%BC%8C2021%E5%B9%B4%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%231%E4%B8%BA%E4%BB%80%E4%B9%88-edis-%E9%9C%80%E8%A6%81%E6%8A%8A%E6%89%80%E6%9C%89%E6%95%B0%E6%8D%AE%E6%94%BE%E5%88%B0%E5%86%85%E5%AD%98%E4%B8%AD)

Redis 为了达到最快的读写速度将数据都读到内存中，并通过异步的方式将数据写入磁盘。所以 Redis  具有快速和数据持久化的特征。如果不将数据放在内存中， 磁盘 I/O 速度为严重影响 Redis 的性能。在内存越来越便宜的今天， Redis  将会越来越受欢迎。如果设置了最大使用的内存， 则数据已有记录数达到内存限值后不能继续插入新值。

### [2、MySQL里有2000w数据，Redis中只存20w的数据](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%98%EF%BC%8C2021%E5%B9%B4%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%232mysql%E9%87%8C%E6%9C%892000w%E6%95%B0%E6%8D%AEredis%E4%B8%AD%E5%8F%AA%E5%AD%9820w%E7%9A%84%E6%95%B0%E6%8D%AE)

**如何保证Redis中的数据都是热点数据？**

Redis内存数据集大小上升到一定大小的时候，就会施行数据淘汰策略。

### [3、Reids6种淘汰策略：](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%98%EF%BC%8C2021%E5%B9%B4%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%233reids6%E7%A7%8D%E6%B7%98%E6%B1%B0%E7%AD%96%E7%95%A5%EF%BC%9A)

**noeviction**: 不删除策略, 达到最大内存限制时, 如果需要更多内存, 直接返回错误信息。大多数写命令都会导致占用更多的内存(有极少数会例外。

**allkeys-lru:**所有key通用; 优先删除最近最少使用(less recently used ,LRU) 的 key。

**volatile-lru:**只限于设置了 expire 的部分; 优先删除最近最少使用(less recently used ,LRU) 的 key。

**allkeys-random:**所有key通用; 随机删除一部分 key。

**volatile-random**: 只限于设置了 **expire** 的部分; 随机删除一部分 key。

**volatile-ttl**: 只限于设置了 **expire** 的部分; 优先删除剩余时间(time to live,TTL) 短的key。

### 4、Redis还提供的高级工具

像慢查询分析、性能测试、Pipeline、事务、Lua自定义命令、Bitmaps、HyperLogLog、/订阅、Geo等个性化功能。

### [5、Pipeline 有什么好处，为什么要用pipeline？](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%98%EF%BC%8C2021%E5%B9%B4%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%235pipeline-%E6%9C%89%E4%BB%80%E4%B9%88%E5%A5%BD%E5%A4%84%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E7%94%A8pipeline)

可以将多次 IO 往返的时间缩减为一次，前提是 pipeline 执行的指令之间没有因果相关性。使用 Redis-benchmark 进行压测的时候可以发现影响 Redis 的 QPS 峰值的一个重要因素是 pipeline 批次指令的数目。

### [6、Redis 集群方案什么情况下会导致整个集群不可用？](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%98%EF%BC%8C2021%E5%B9%B4%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%236redis-%E9%9B%86%E7%BE%A4%E6%96%B9%E6%A1%88%E4%BB%80%E4%B9%88%E6%83%85%E5%86%B5%E4%B8%8B%E4%BC%9A%E5%AF%BC%E8%87%B4%E6%95%B4%E4%B8%AA%E9%9B%86%E7%BE%A4%E4%B8%8D%E5%8F%AF%E7%94%A8)

有 A， B， C 三个节点的集群,在没有复制模型的情况下,如果节点 B 失败了， 那么整个集群就会以为缺少 5501-11000 这个范围的槽而不可用。

### [7、Redis 的内存用完了会发生什么？](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%98%EF%BC%8C2021%E5%B9%B4%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%237redis-%E7%9A%84%E5%86%85%E5%AD%98%E7%94%A8%E5%AE%8C%E4%BA%86%E4%BC%9A%E5%8F%91%E7%94%9F%E4%BB%80%E4%B9%88)

如果达到设置的上限，Redis 的写命令会返回错误信息（ 但是读命令还可以正常返回。） 或者你可以将 Redis 当缓存来使用配置淘汰机制， 当 Redis 达到内存上限时会冲刷掉旧的内容。

### [8、删除key](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%98%EF%BC%8C2021%E5%B9%B4%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%238%E5%88%A0%E9%99%A4key)

del key1 key2 ...

### [9、Redis集群最大节点个数是多少？](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%98%EF%BC%8C2021%E5%B9%B4%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%239redis%E9%9B%86%E7%BE%A4%E6%9C%80%E5%A4%A7%E8%8A%82%E7%82%B9%E4%B8%AA%E6%95%B0%E6%98%AF%E5%A4%9A%E5%B0%91)

16384个。

### [10、Redis 到底是怎么实现“附近的人”](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%98%EF%BC%8C2021%E5%B9%B4%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%2310redis-%E5%88%B0%E5%BA%95%E6%98%AF%E6%80%8E%E4%B9%88%E5%AE%9E%E7%8E%B0%E2%80%9C%E9%99%84%E8%BF%91%E7%9A%84%E4%BA%BA)

GEOADD key longitude latitude member [longitude latitude member ...]

将给定的位置对象（纬度、经度、名字）添加到指定的key。其中，key为集合名称，member为该经纬度所对应的对象。在实际运用中，当所需存储的对象数量过多时，可通过设置多key(如一个省一个key)的方式对对象集合变相做sharding，避免单集合数量过多。

**成功插入后的返回值：**

(integer) N

其中N为成功插入的个数。

### 1[1、MySQL里有2000w数据，Redis中只存20w的数据，如何保证Redis中的数据都是热点数据？](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E9%99%84%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%231mysql%E9%87%8C%E6%9C%892000w%E6%95%B0%E6%8D%AEredis%E4%B8%AD%E5%8F%AA%E5%AD%9820w%E7%9A%84%E6%95%B0%E6%8D%AE%E5%A6%82%E4%BD%95%E4%BF%9D%E8%AF%81redis%E4%B8%AD%E7%9A%84%E6%95%B0%E6%8D%AE%E9%83%BD%E6%98%AF%E7%83%AD%E7%82%B9%E6%95%B0%E6%8D%AE)

Redis内存数据集大小上升到一定大小的时候，就会施行数据淘汰策略。

### 1[2、Redis 过期键的删除策略？](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E9%99%84%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%232redis-%E8%BF%87%E6%9C%9F%E9%94%AE%E7%9A%84%E5%88%A0%E9%99%A4%E7%AD%96%E7%95%A5)

**1、** 定时删除:在设置键的过期时间的同时，创建一个定时器 timer). 让定时器在键的过期时间来临时， 立即执行对键的删除操作。

**2、** 惰性删除:放任键过期不管，但是每次从键空间中获取键时，都检查取得的键是 否过期， 如果过期的话， 就删除该键;如果没有过期， 就返回该键。

**3、** 定期删除:每隔一段时间程序就对数据库进行一次检查，删除里面的过期键。至 于要删除多少过期键， 以及要检查多少个数据库， 则由算法决定。

### 1[3、mySQL里有2000w数据，Redis中只存20w的数据，如何保证Redis中的数据都是热点数据](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E9%99%84%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%233mysql%E9%87%8C%E6%9C%892000w%E6%95%B0%E6%8D%AEredis%E4%B8%AD%E5%8F%AA%E5%AD%9820w%E7%9A%84%E6%95%B0%E6%8D%AE%E5%A6%82%E4%BD%95%E4%BF%9D%E8%AF%81redis%E4%B8%AD%E7%9A%84%E6%95%B0%E6%8D%AE%E9%83%BD%E6%98%AF%E7%83%AD%E7%82%B9%E6%95%B0%E6%8D%AE)

*相关知识：Redis 内存数据集大小上升到一定大小的时候，就会施行数据淘汰策略（回收策略）。*

### 1[4、Redis key的过期时间和永久有效分别怎么设置？](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E9%99%84%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%234redis-key%E7%9A%84%E8%BF%87%E6%9C%9F%E6%97%B6%E9%97%B4%E5%92%8C%E6%B0%B8%E4%B9%85%E6%9C%89%E6%95%88%E5%88%86%E5%88%AB%E6%80%8E%E4%B9%88%E8%AE%BE%E7%BD%AE)

EXPIRE和PERSIST命令。

### 1[5、请用Redis和任意语言实现一段恶意登录保护的代码，](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E9%99%84%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%235%E8%AF%B7%E7%94%A8redis%E5%92%8C%E4%BB%BB%E6%84%8F%E8%AF%AD%E8%A8%80%E5%AE%9E%E7%8E%B0%E4%B8%80%E6%AE%B5%E6%81%B6%E6%84%8F%E7%99%BB%E5%BD%95%E4%BF%9D%E6%8A%A4%E7%9A%84%E4%BB%A3%E7%A0%81)

限制1小时内每用户Id最多只能登录5次。具体登录函数或功能用空函数即可，不用详细写出。

用列表实现:列表中每个元素代表登陆时间,只要最后的第5次登陆时间和现在时间差不超过1小时就禁止登陆.用Python写的代码如下：

\#!/usr/bin/env python3 import Redis   import sys   import time     r =  Redis.StrictRedis(host=’127.0.0.1′, port=6379, db=0)   try:             id = sys.argv[1] except:           print(‘input argument error’)         sys.exit(0)   if r.llen(id) >= 5 and time.time() –  float(r.lindex(id, 4)) <= 3600:           print(“you are forbidden  logining”) else:            print(‘you are allowed to login’)          r.lpush(id, time.time())         # login_func()

### 1[6、，或是关注](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E9%99%84%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%236%E6%88%96%E6%98%AF%E5%85%B3%E6%B3%A8)

### 1[7、怎么理解Redis事务？](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E9%99%84%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%237%E6%80%8E%E4%B9%88%E7%90%86%E8%A7%A3redis%E4%BA%8B%E5%8A%A1)

事务是一个单独的隔离操作：事务中的所有命令都会序列化、按顺序地执行。事务在执行的过程中，不会被其他客户端发送来的命令请求所打断。

事务是一个原子操作：事务中的命令要么全部被执行，要么全部都不执行。

### 1[8、Redis key 的过期时间和永久有效分别怎么设置？](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E9%99%84%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%238redis-key-%E7%9A%84%E8%BF%87%E6%9C%9F%E6%97%B6%E9%97%B4%E5%92%8C%E6%B0%B8%E4%B9%85%E6%9C%89%E6%95%88%E5%88%86%E5%88%AB%E6%80%8E%E4%B9%88%E8%AE%BE%E7%BD%AE)

EXPIRE 和 PERSIST 命令。

### 1[9、Redis中海量数据的正确操作方式](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E9%99%84%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%239redis%E4%B8%AD%E6%B5%B7%E9%87%8F%E6%95%B0%E6%8D%AE%E7%9A%84%E6%AD%A3%E7%A1%AE%E6%93%8D%E4%BD%9C%E6%96%B9%E5%BC%8F)

利用SCAN系列命令（SCAN、SSCAN、HSCAN、ZSCAN）完成数据迭代。

### 2[0、什么是Redis？简述它的优缺点？](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E9%99%84%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%2310%E4%BB%80%E4%B9%88%E6%98%AFredis%E7%AE%80%E8%BF%B0%E5%AE%83%E7%9A%84%E4%BC%98%E7%BC%BA%E7%82%B9)

Redis的全称是：Remote Dictionary.Server，本质上是一个Key-Value类型的内存数据库，很像Memcached，整个数据库统统加载在内存当中进行操作，定期通过异步操作把数据库数据flush到硬盘上进行保存。

因为是纯内存操作，Redis的性能非常出色，每秒可以处理超过 10万次读写操作，是已知性能最快的Key-Value DB。

Redis的出色之处不仅仅是性能，Redis最大的魅力是支持保存多种数据结构，此外单个value的最大限制是1GB，不像 Memcached只能保存1MB的数据，因此Redis可以用来实现很多有用的功能。

比方说用他的List来做FIFO双向链表，实现一个轻量级的高性 能消息队列服务，用他的Set可以做高性能的tag系统等等。

另外Redis也可以对存入的Key-Value设置expire时间，因此也可以被当作一 个功能加强版的Memcached来用。  Redis的主要缺点是数据库容量受到物理内存的限制，不能用作海量数据的高性能读写，因此Redis适合的场景主要局限在较小数据量的高性能操作和运算上。

### 2[1、Redis如何做内存优化？](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%982021%E5%B9%B4%EF%BC%8C%E5%B8%B8%E8%A7%81%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%231redis%E5%A6%82%E4%BD%95%E5%81%9A%E5%86%85%E5%AD%98%E4%BC%98%E5%8C%96)

尽可能使用散列表（hashes），散列表（是说散列表里面存储的数少）使用的内存非常小，所以你应该尽可能的将你的数据模型抽象到一个散列表里面。比如你的web系统中有一个用户对象，不要为这个用户的名称，姓氏，邮箱，密码设置单独的key,而是应该把这个用户的所有信息存储到一张散列表里面.

### 2[2、Pipeline有什么好处，为什么要用pipeline？](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%982021%E5%B9%B4%EF%BC%8C%E5%B8%B8%E8%A7%81%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%232pipeline%E6%9C%89%E4%BB%80%E4%B9%88%E5%A5%BD%E5%A4%84%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E7%94%A8pipeline)

可以将多次IO往返的时间缩减为一次，前提是pipeline执行的指令之间没有因果相关性。使用Redis-benchmark进行压测的时候可以发现影响Redis的QPS峰值的一个重要因素是pipeline批次指令的数目。

### 2[3、Redis常用管理命令](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%982021%E5%B9%B4%EF%BC%8C%E5%B8%B8%E8%A7%81%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%233redis%E5%B8%B8%E7%94%A8%E7%AE%A1%E7%90%86%E5%91%BD%E4%BB%A4)

\# dbsize 返回当前数据库 key 的数量。 # info 返回当前 Redis 服务器状态和一些统计信息。 # monitor  实时监听并返回Redis服务器接收到的所有请求信息。 # shutdown 把数据同步保存到磁盘上，并关闭Redis服务。 # config  get parameter 获取一个 Redis 配置参数信息。（个别参数可能无法获取） # config set parameter  value 设置一个 Redis 配置参数信息。（个别参数可能无法获取） # config resetstat 重置 info  命令的统计信息。（重置包括：keyspace 命中数、 # keyspace 错误数、 处理命令数，接收连接数、过期 key 数） #  debug object key 获取一个 key 的调试信息。 # debug segfault 制造一次服务器当机。 # flushdb  删除当前数据库中所有 key,此方法不会失败。小心慎用 # flushall 删除全部数据库中所有 key，此方法不会失败。小心慎用

### 2[4、Redis持久化数据和缓存怎么做扩容？](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%982021%E5%B9%B4%EF%BC%8C%E5%B8%B8%E8%A7%81%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%234redis%E6%8C%81%E4%B9%85%E5%8C%96%E6%95%B0%E6%8D%AE%E5%92%8C%E7%BC%93%E5%AD%98%E6%80%8E%E4%B9%88%E5%81%9A%E6%89%A9%E5%AE%B9)

1. 如果Redis被当做缓存使用，使用一致性哈希实现动态扩容缩容。
2. 如果Redis被当做一个持久化存储使用，必须使用固定的keys-to-nodes映射关系，节点的数量一旦确定不能变化。否则的话(即Redis节点需要动态变化的情况），必须使用可以在运行时进行数据再平衡的一套系统，而当前只有Redis集群可以做到这样。

### 2[5、Twemproxy是什么？](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%982021%E5%B9%B4%EF%BC%8C%E5%B8%B8%E8%A7%81%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%235twemproxy%E6%98%AF%E4%BB%80%E4%B9%88)

Twemproxy是Twitter维护的（缓存）代理系统，代理Memcached的ASCII协议和Redis协议。它是单线程程序，使用c语言编写，运行起来非常快。它是采用Apache 2.0 license的开源软件。  Twemproxy支持自动分区，如果其代理的其中一个Redis节点不可用时，会自动将该节点排除（这将改变原来的keys-instances的映射关系，所以你应该仅在把Redis当缓存时使用Twemproxy)。 Twemproxy本身不存在单点问题，因为你可以启动多个Twemproxy实例，然后让你的客户端去连接任意一个Twemproxy实例。  Twemproxy是Redis客户端和服务器端的一个中间层，由它来处理分区功能应该不算复杂，并且应该算比较可靠的。

### 2[6、Redis没有直接使用C字符串](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%982021%E5%B9%B4%EF%BC%8C%E5%B8%B8%E8%A7%81%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%236redis%E6%B2%A1%E6%9C%89%E7%9B%B4%E6%8E%A5%E4%BD%BF%E7%94%A8c%E5%AD%97%E7%AC%A6%E4%B8%B2)

(即以空字符’\0’结尾的字符数组)作为默认的字符串表示，而是使用了SDS。SDS是简单动态字符串(Simple Dynamic String)的缩写。

### 2[7、使用过 Redis 分布式锁么，它是什么回事？](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%982021%E5%B9%B4%EF%BC%8C%E5%B8%B8%E8%A7%81%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%237%E4%BD%BF%E7%94%A8%E8%BF%87-redis-%E5%88%86%E5%B8%83%E5%BC%8F%E9%94%81%E4%B9%88%E5%AE%83%E6%98%AF%E4%BB%80%E4%B9%88%E5%9B%9E%E4%BA%8B)

先拿 setnx 来争抢锁， 抢到之后， 再用 expire 给锁加一个过期时间防止锁忘记了释放。

这时候对方会告诉你说你回答得不错， 然后接着问如果在 setnx 之后执行 expire 之前进程意外 crash 或者要重启维护了， 那会怎么样？

这时候你要给予惊讶的反馈： 唉， 是喔， 这个锁就永远得不到释放了。紧接着你需要抓一抓自己得脑袋， 故作思考片刻， 好像接下来的结果是你主动思考出来的， 然后回我记得  set 指令有非常复杂的参数， 这个应该是可以同时把 setnx 和expire 合成一条指令来用的！ 对方这时会显露笑容， 心里开始默念：  摁， 这小子还不错。

### 2[8、Redis如何设置密码及验证密码？](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%982021%E5%B9%B4%EF%BC%8C%E5%B8%B8%E8%A7%81%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%238redis%E5%A6%82%E4%BD%95%E8%AE%BE%E7%BD%AE%E5%AF%86%E7%A0%81%E5%8F%8A%E9%AA%8C%E8%AF%81%E5%AF%86%E7%A0%81)

设置密码：config set requirepass 123456

授权密码：auth 123456

### 2[9、一个 Redis 实例最多能存放多少的 keys？List、Set、Sorted Set 他们最多能存放多少元素?](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%982021%E5%B9%B4%EF%BC%8C%E5%B8%B8%E8%A7%81%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%239%E4%B8%80%E4%B8%AA-redis-%E5%AE%9E%E4%BE%8B%E6%9C%80%E5%A4%9A%E8%83%BD%E5%AD%98%E6%94%BE%E5%A4%9A%E5%B0%91%E7%9A%84-keyslistsetsorted-set-%E4%BB%96%E4%BB%AC%E6%9C%80%E5%A4%9A%E8%83%BD%E5%AD%98%E6%94%BE%E5%A4%9A%E5%B0%91%E5%85%83%E7%B4%A0)

理论上 Redis 可以处理多达 232 的 keys，并且在实际中进行了测试，每个实例至少存放了 2 亿 5 千万的  keys。我们正在测试一些较大的值。任何 list、set、和 sorted set 都可以放 232 个元素。换句话说， Redis  的存储极限是系统中的可用内存值。

### [30、Redis有哪些适合的场景？](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B0%E9%9D%A2%E8%AF%95%E9%A2%982021%E5%B9%B4%EF%BC%8C%E5%B8%B8%E8%A7%81%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E7%AD%94%E6%A1%88%E6%B1%87%E6%80%BB.md%2310redis%E6%9C%89%E5%93%AA%E4%BA%9B%E9%80%82%E5%90%88%E7%9A%84%E5%9C%BA%E6%99%AF)

**会话缓存（Session Cache）**

最常用的一种使用Redis的情景是会话缓存（sessioncache），用Redis缓存会话比其他存储（如Memcached）的优势在于：Redis提供持久化。当维护一个不是严格要求一致性的缓存时，如果用户的购物车信息全部丢失，大部分人都会不高兴的，现在，他们还会这样吗？

幸运的是，随着 Redis 这些年的改进，很容易找到怎么恰当的使用Redis来缓存会话的文档。甚至广为人知的商业平台Magento也提供Redis的插件。

**全页缓存（FPC）**

除基本的会话token之外，Redis还提供很简便的FPC平台。回到一致性问题，即使重启了Redis实例，因为有磁盘的持久化，用户也不会看到页面加载速度的下降，这是一个极大改进，类似PHP本地FPC。

再次以Magento为例，Magento提供一个插件来使用Redis作为全页缓存后端。

此外，对WordPress的用户来说，Pantheon有一个非常好的插件wp-Redis，这个插件能帮助你以最快速度加载你曾浏览过的页面。

**队列**

Reids在内存存储引擎领域的一大优点是提供list和set操作，这使得Redis能作为一个很好的消息队列平台来使用。Redis作为队列使用的操作，就类似于本地程序语言（如Python）对 list 的 push/pop 操作。

如果你快速的在Google中搜索“Redis queues”，你马上就能找到大量的开源项目，这些项目的目的就是利用Redis创建非常好的后端工具，以满足各种队列需求。例如，Celery有一个后台就是使用Redis作为broker，你可以从这里去查看。

**排行榜/计数器**

Redis在内存中对数字进行递增或递减的操作实现的非常好。集合（Set）和有序集合（SortedSet）也使得我们在执行这些操作的时候变的非常简单，Redis只是正好提供了这两种数据结构。

所以，我们要从排序集合中获取到排名最靠前的10个用户–我们称之为“user_scores”，我们只需要像下面一样执行即可：

当然，这是假定你是根据你用户的分数做递增的排序。如果你想返回用户及用户的分数，你需要这样执行：

ZRANGE user_scores 0 10 WITHSCORES

Agora Games就是一个很好的例子，用Ruby实现的，它的排行榜就是使用Redis来存储数据的，你可以在这里看到。

**发布/订阅**

最后（但肯定不是最不重要的）是Redis的发布/订阅功能。发布/订阅的使用场景确实非常多。我已看见人们在社交网络连接中使用，还可作为基于发布/订阅的脚本触发器，甚至用Redis的发布/订阅功能来建立聊天系统！

### 3[1、Redis集群方案应该怎么做？都有哪些方案？](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B02021%E5%B9%B4%E9%9D%A2%E8%AF%95%E9%A2%98%EF%BC%8C%E9%AB%98%E7%BA%A7%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E9%99%84%E7%AD%94%E6%A1%88%E8%A7%A3%E6%9E%90.md%231redis%E9%9B%86%E7%BE%A4%E6%96%B9%E6%A1%88%E5%BA%94%E8%AF%A5%E6%80%8E%E4%B9%88%E5%81%9A%E9%83%BD%E6%9C%89%E5%93%AA%E4%BA%9B%E6%96%B9%E6%A1%88)

**1、** codis。

目前用的最多的集群方案，基本和twemproxy一致的效果，但它支持在 节点数量改变情况下，旧节点数据可恢复到新hash节点。

**2、** Redis cluster3.0自带的集群，特点在于他的分布式算法不是一致性hash，而是hash槽的概念，以及自身支持节点设置从节点。具体看官方文档介绍。

**3、** 在业务代码层实现，起几个毫无关联的Redis实例，在代码层，对key 进行hash计算，然后去对应的Redis实例操作数据。 这种方式对hash层代码要求比较高，考虑部分包

### 3[2、Reids支持的语言：](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B02021%E5%B9%B4%E9%9D%A2%E8%AF%95%E9%A2%98%EF%BC%8C%E9%AB%98%E7%BA%A7%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E9%99%84%E7%AD%94%E6%A1%88%E8%A7%A3%E6%9E%90.md%232reids%E6%94%AF%E6%8C%81%E7%9A%84%E8%AF%AD%E8%A8%80%EF%BC%9A)

java、C、C#、C++、php、Node.js、Go等。

### 3[3、怎么测试Redis的连通性？](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B02021%E5%B9%B4%E9%9D%A2%E8%AF%95%E9%A2%98%EF%BC%8C%E9%AB%98%E7%BA%A7%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E9%99%84%E7%AD%94%E6%A1%88%E8%A7%A3%E6%9E%90.md%233%E6%80%8E%E4%B9%88%E6%B5%8B%E8%AF%95redis%E7%9A%84%E8%BF%9E%E9%80%9A%E6%80%A7)

ping

### 3[4、Redis 集群会有写操作丢失吗？为什么？](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B02021%E5%B9%B4%E9%9D%A2%E8%AF%95%E9%A2%98%EF%BC%8C%E9%AB%98%E7%BA%A7%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E9%99%84%E7%AD%94%E6%A1%88%E8%A7%A3%E6%9E%90.md%234redis-%E9%9B%86%E7%BE%A4%E4%BC%9A%E6%9C%89%E5%86%99%E6%93%8D%E4%BD%9C%E4%B8%A2%E5%A4%B1%E5%90%97%E4%B8%BA%E4%BB%80%E4%B9%88)

Redis 并不能保证数据的强一致性，这意味这在实际中集群在特定的条件下可能会丢失写操作。

### 3[5、Redis回收使用的是什么算法？](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B02021%E5%B9%B4%E9%9D%A2%E8%AF%95%E9%A2%98%EF%BC%8C%E9%AB%98%E7%BA%A7%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E9%99%84%E7%AD%94%E6%A1%88%E8%A7%A3%E6%9E%90.md%235redis%E5%9B%9E%E6%94%B6%E4%BD%BF%E7%94%A8%E7%9A%84%E6%98%AF%E4%BB%80%E4%B9%88%E7%AE%97%E6%B3%95)

LRU算法

### 3[6、Redis的并发竞争问题如何解决?](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B02021%E5%B9%B4%E9%9D%A2%E8%AF%95%E9%A2%98%EF%BC%8C%E9%AB%98%E7%BA%A7%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E9%99%84%E7%AD%94%E6%A1%88%E8%A7%A3%E6%9E%90.md%236redis%E7%9A%84%E5%B9%B6%E5%8F%91%E7%AB%9E%E4%BA%89%E9%97%AE%E9%A2%98%E5%A6%82%E4%BD%95%E8%A7%A3%E5%86%B3)

单进程单线程模式，采用队列模式将并发访问变为串行访问。Redis本身没有锁的概念，Redis对于多个客户端连接并不存在竞争，利用setnx实现锁。

### 3[8、AOF常用配置总结](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B02021%E5%B9%B4%E9%9D%A2%E8%AF%95%E9%A2%98%EF%BC%8C%E9%AB%98%E7%BA%A7%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E9%99%84%E7%AD%94%E6%A1%88%E8%A7%A3%E6%9E%90.md%237aof%E5%B8%B8%E7%94%A8%E9%85%8D%E7%BD%AE%E6%80%BB%E7%BB%93)

下面是AOF常用的配置项，以及默认值；前面介绍过的这里不再详细介绍。

**1、** appendonly no：是否开启AOF

**2、** appendfilename "appendonly.aof"：AOF文件名

**3、** dir ./：RDB文件和AOF文件所在目录

**4、** appendfsync everysec：fsync持久化策略

**5、** no-appendfsync-on-rewrite no：AOF重写期间是否禁止fsync；如果开启该选项，可以减轻文件重写时CPU和硬盘的负载（尤其是硬盘），但是可能会丢失AOF重写期间的数据；需要在负载和安全性之间进行平衡

**6、** auto-aof-rewrite-percentage 100：文件重写触发条件之一

**7、** auto-aof-rewrite-min-size 64mb：文件重写触发提交之一

**8、** aof-load-truncated yes：如果AOF文件结尾损坏，Redis启动时是否仍载入AOF文件

### 3[9、Redis 管道 Pipeline](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B02021%E5%B9%B4%E9%9D%A2%E8%AF%95%E9%A2%98%EF%BC%8C%E9%AB%98%E7%BA%A7%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E9%99%84%E7%AD%94%E6%A1%88%E8%A7%A3%E6%9E%90.md%238redis-%E7%AE%A1%E9%81%93-pipeline)

在某些场景下我们在**一次操作中可能需要执行多个命令**，而如果我们只是一个命令一个命令去执行则会浪费很多网络消耗时间，如果将命令一次性传输到 `Redis`中去再执行，则会减少很多开销时间。但是需要注意的是 `pipeline`中的命令并不是原子性执行的，也就是说管道中的命令到达 `Redis`服务器的时候可能会被其他的命令穿插

### [40、Redis集群方案什么情况下会导致整个集群不可用？](https://link.zhihu.com/?target=https%3A//gitee.com/souyunku/DevBooks/blob/master/docs/Redis/Redis%E6%9C%80%E6%96%B02021%E5%B9%B4%E9%9D%A2%E8%AF%95%E9%A2%98%EF%BC%8C%E9%AB%98%E7%BA%A7%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%8A%E9%99%84%E7%AD%94%E6%A1%88%E8%A7%A3%E6%9E%90.md%2310redis%E9%9B%86%E7%BE%A4%E6%96%B9%E6%A1%88%E4%BB%80%E4%B9%88%E6%83%85%E5%86%B5%E4%B8%8B%E4%BC%9A%E5%AF%BC%E8%87%B4%E6%95%B4%E4%B8%AA%E9%9B%86%E7%BE%A4%E4%B8%8D%E5%8F%AF%E7%94%A8)

有A，B，C三个节点的集群,在没有复制模型的情况下,如果节点B失败了，那么整个集群就会以为缺少5501-11000这个范围的槽而不可用。