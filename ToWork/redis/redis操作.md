1.redis默认有16个数据库
   默认是使用第0个
2.切换数据库 select 下标
3.keys *查看所有的key  然后get 
4.dbsize 数据库的大小
5.flushdb 清空当前数据库
6.flushall 清空全部数据库
7.redis是单线程的 ，官方表示，redis是基于内存操作的，cpu不是redis的性能瓶颈，redis
的性能瓶颈是根据机器的内存和网络的带宽，既然可以使用单线程就单线程
8.CPU>内存>硬盘redis是将所有的数据放在内存的，所以说使用单线程效率就是最高的
多线程会进行cpu的上下文切换，这是一个耗时的操作，对于内存系统来说，没有上下文切换
效率就是最佳的，多次读写都是在一个cpu上的
9.redis的作用：数据库，消息中间件，缓存

基本类型：Redis-key
1.exists key存在返回1 不存在返回0
2.move key 1（表示当前数据库） 移除数据
3.expire key time （s） 设置过期时间
4.ttl key  查看过期的剩余时间
5.type key查看key的类型

String (字符串的类型) 
1.append key ”字符“ 可以动态的增加key的长度 如果当前key不存在，就相当于set
2.strlen key 截取字符串类似于追加
3.incr key 自增一
4.decr key 减一
5.incrby|decr不by 数字 设置步长指定增量
6. getrange key start end  截取对应的值的内容  end是-1 就是全部的
7.strrange key star xxx   替换，指定位置开始的字符串
8.setex  设置过期时间
9.setnx  如果不存在创建  存在就不成功（0） （分布式锁中常常使用）
10.mset 设置多个值
11.mget 得到多个值
12.msetnx  原子性的操作，一起成功一起失败
13.对象 set user:1:name 张三
14.getset key ”00“ 先获取再set 若空则报空再创建，存在返回这个值然后再设置值

List
1.在redis里面可以把list玩成，栈队列
*所有的命令都是l开头
2.lpush 将一个值插入列表的头部
3.Rpush 从右边插入（尾部）
4.lrang list start end 查看list中的数据 若end是-1 则是查看全部
5.左右移除 lpop|rpop list 
6.lindex list key 获取下标
7.llen list 返回列表的长度
8.lrem key count（移除的个数，从左到右，从上到下， list可以有重复的值）key  移除list集合中的指定个数的value
9.ltrim list start end 留下中间的元素包括边界点
10.rpoplpush list olist 移除列表的最后一个元素并添加到新的列表中
11.lset list index ”xxx“    list必须存在，存在就更新下标的值，不存在报错
12.linsert list key before|after 键名 ”xxx“  往前或者往后插入

set
1.sadd set "xxx" 添加值
2.smembers  set 查看所有的元素
3.sismember 判断某一个元素是不是在集合中
4.scard set 获取set元素的个数
5.srem set name 移除set中的指定元素
6.srandmember set （count） 随机抽选元素 随机抽选元素的个数  有放回采样
7.spop 随机删除set中的元素 无放回采样
8. smove set1 set2 "set1中的值" 将一个指定的值移动到另一个set集合  从源移动到目标
9.sdiff|sinter|sunion set1 set2  差集交集 并集   （共同好友）
10 sinterstore dest key1 key2 key3 命令将给定集合之间的交集存储在指定的集合中。如果指定的集合已经存在，则将其覆盖

hash
1.hset hash key value  设置一个
2.hget hash key   获取一个
3.hmset  hash key value key2 value2 设置多个  会覆盖
4.hmget hash key key2  得到多个
5.hgetall   得到全部
6.hdel hash key 删除删除指定key的hash中的值
7.hlen hash  获取hash的长度
8.hexisits hash key 指定的key是否存在
9.hkeys hash 获取所有的键
10. hval hash  获取所有的值
11.hincrby|hdecrby hash key count 指定增量
12.hsetnx hash key value如果不存在可以设置，存在就不可以设置

   sorted set
zset 有序集合  多了一个排序
1.在set上加了一个值
2. zadd zset count value 增加一个值
3.zrangebyscore zset -inf +inf 显示全部的用户，从小到大
zrange zset  0 -1 [withscores] 带分数的

4.zrevrang zset 0 -1 从大到小排序
5. zrangebyscore zset -inf +inf  withscores 附带成绩
6.zrangebyscore zset -inf  2500 withscores 显示小于2500的员工升序
7.zrem zset value 移除某一个值
zremrangebyrank set 范围
zrembyscore set 范围
8.zcard zset 获取有序集合中的个数
9.zcount zset 1 2 获取指定区间的成员数量
10. zrank set 输出排名的位置从一开始
11.zinterstore mubiao number1 number2 

三种特殊的数据类型
geospatial
可以查询一些城市的经纬读，查询周围多少公里的人
1.getadd key value1（维度） value2（经度）城市名字 可以添加两个 添加地理位置经纬度
2.一般直接导入
3.geopos key 城市名 获取指定的地理位置的经纬度
4.geodist key 城市1 城市2 直线距离
5.georadius key 经 纬度 count 单位 withdist（距离）|withcoord（具体信息）  count（限定）  以经纬为中心，显示在key中的半径为count的值
6.georadiusbymember key 城市 count 单位 找出指定元素周围的其他元素
7.geohash key 城市 讲二维的变为一维的字符，越像越近
8.zrange key 0 -1  查看所有的元素


hyperloglog（是否可以容错）
基数统计人数之类的
占用的内存是固定的
0.81的错误率
1.pfadd key value[] 
2.pfcount key 统计数量
3.pfmerge key（结果） key1 key2 合并两组 （并集）

bitmaps（0  1 ）
位存储
1.setbit sign 0|1
2.bitcount sign 统计1总数
(bitop and )与对位运算实现逻辑与运算


事务
redis的事务：一组命令的集合，一个事务的所有命令都会被序列化，在事务执行的过程中，会按照顺序执行
一次性顺序性排他性
redis的单条命令保证原子性，事务不保证原子性
redis没有隔离级别的概念

1.开启事务：multi 
2.命令入队.....
3.执行事务 exec

4.取消事务：discard
5.事务出错 编译出现异常 全部不会执行
6.与运行时异常 出错的抛出异常




redis 的乐观锁
1.Watch 监控
2.watch key
3.exec
若其他线程来操作监控的数据  则就操作失败


jedis ： 采用的是直连的，多个线程操作的话是不安全的，如果想要避免不安全使用jedispool连接池，像bio，连接池的问题
lettuce ： 采用netty，实力可以在多个线程中进行共享，不存在线程不安全的情况，可以减少线程  更像nio




rdb：每几分钟向磁盘写入数据库的存储信息	
aof：对时间间隔存储写操作  文件正常重启就可以直接恢复文件
aof默认是文件的无限追加 超过默认的大小就会重写

在宿主机上的docker 的日志数据以及配置文件  挂接到本机的 数据文件上   便于管理    mounted

docker run -itd --name redis -p 6379:6379 --restart=always  -v f:\redis.conf:/etc/redis/redis.conf  -v f:\data:/data  redis  redis-server /etc/redis/redis.conf

docker :
redis-server  配置文件
redis-cli  
docker exec -it redis /bin/bash 启动服务器
	  启动客户端
--------------------------------------------------------------------------

incr 值 每次数加一 
decr 
incrby 值  数字	
incrbyfloat 值 数字   
strlen  name




-----------------------------------------questions1-----------------------------
统计100w用户一周内登录的次数  
用位运算 
//初始化 
setbit mon 100w 0;
setbit tue 100w 0;
...
登录了一天就把对应的值改为1

bitop (逻辑运算) bit1 bit2;
-----------------------------------------questions2-----------------------------
排行榜
zadd 对排行榜内的人进行成绩的覆盖
zrevrange 从大到小的逆序排序  获取前多少人的数据
zrevrank  leaderboard lisi  获取lisi这个人在全部中的排名
-----------------------------------------questions3-----------------------------
标签求值的问题  
若是直接使用mysql建立数据库    那就会有查询标签的sql语句复杂
可以使用redis的set来解决     存值之后可以 使用  并交差来算取结果

docker exec -it redis bin/bash 
redis-cli