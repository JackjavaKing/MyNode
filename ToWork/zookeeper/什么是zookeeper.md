### 什么是zookeeper

ZooKeeper由雅虎研究院开发，是Google Chubby的开源实现，后来托管到Apache，于2010年11月正式成为Apache的顶级项目。
ZooKeeper是一个经典的分布式数据一致性解决方案，致力于为分布式应用提供一个高性能、高可用，且具有严格顺序访问控制能力的分布式协调服务。
分布式应用程序可以基于ZooKeeper实现数据发布与订阅、负载均衡、命名服务、分布式协调与通知、集群管理、Leader选举、分布式锁、分布式队列等功能。



fast fail  无单点故障    leader 选举采用**zab**协议   过半数节点**以上**据统一成功就成功,其余后台线程网络同步  保障追终一致.

**Zab**

ZAB 协议是为分布式协调服务 ZooKeeper 专门设计的一种支持崩溃恢复的原子广播协议。

ZAB 协议包括两种基本的模式：崩溃恢复 和 消息广播。

当整个 ZooKeeper 集群 刚刚启动或者 Leader 服务器宕机、重启或者网络故障导致不存在过半的服务器与 Leader 服务器保持正常通信时，所有进程（服务器）进入崩溃恢复模式，首先选举产生新的 Leader 服务器，然后集群中 Follower 服务器开始与新的 Leader 服务器进行数据同步，当集群中超过半数服务器与该 Leader 服务器完成数据同步之后，退出恢复模式进入消息广播模式，Leader 服务器开始接受客户端的事务请求生成事务提案来进行事务请求处理。

![image-20220507092106075](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220507092106075.png)

### 为什么zookeeper 的集群数是单数？

**zookeeper集群选举leader的原则：可用节点数量>集群总节点数量/2**

**1.容错**（更加节省资源）

2台服务器，至少2台正常运行才行（2的半数为1，半数以上最少为2），正常运行1台服务器都不允许挂掉

3台服务器，至少2台正常运行才行（3的半数为1.5，半数以上最少为2），正常运行可以允许1台服务器挂掉

4台服务器，至少3台正常运行才行（4的半数为2，半数以上最少为3），正常运行可以允许1台服务器挂掉

5台服务器，至少3台正常运行才行（5的半数为2.5，半数以上最少为3），正常运行可以允许2台服务器挂掉

6台服务器，至少3台正常运行才行（6的半数为3，半数以上最少为4），正常运行可以允许2台服务器挂掉
**2.防脑裂：**

集群互不通讯情况：

一个集群3台服务器，全部运行正常，但是其中1台裂开了，和另外2台无法通讯。3台机器里面2台正常运行过半票可以选出一个leader。

一个集群4台服务器，全部运行正常，但是其中2台裂开了，和另外2台无法通讯。4台机器里面2台正常工作没有过半票以上达到3，无法选出leader正常运行。

一个集群5台服务器，全部运行正常，但是其中2台裂开了，和另外3台无法通讯。5台机器里面3台正常运行过半票可以选出一个leader。

一个集群6台服务器，全部运行正常，但是其中3台裂开了，和另外3台无法通讯。6台机器里面3台正常工作没有过半票以上达到4，无法选出leader正常运行。





### **监听客户端的命令**

**addWatch**：客户端在节点上添加监听，有两种模式PERSISTENT和PERSISTENT_RECURSIVE，PERSISTENT模式只监听指定的节点事件，而PERSISTENT_RECURSIVE模式会监听指定节点与它所有子节点的事件。
**removewatches**：删除客户端对节点的监听
![image-20220507114040106](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220507114040106.png)
**printwatches**：是否打印监听事件（on或者off)



### node的特点类型等等

1：**PERSISTENT**                // 持久化节点 ， 节点创建后会被持久化，只有主动调用delete方法的时候才可以删除节点。

**create /nodename**

2：**PERSISTENT_SEQUENTIAL**    // 持久化排序节点， 排序节点：创建的节点名称后自动添加序号，如节点名称为"node-"，自动添加为"node-1"，顺序添加为"node-2"

**create -s /nodename**

3：**EPHEMERAL**                 // 临时节点， 临时节点：节点创建后在创建者超时连接或失去连接的时候，节点会被删除。临时节点下不能存在字节点。

**create -e /nodename**

4：**EPHEMERAL_SEQUENTIAL**    // 临时排序节点， 排序节点：创建的节点名称后自动添加序号，如节点名称为"node-"，自动添加为"node-1"，顺序添加为"node-2"

**create -s -w /nodename** 



**节点信息的解释**



![image-20220507112818544](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220507112818544.png)

```java
属性	含义
cZxid	创建节点时的事务ID
ctime	创建节点时的时间
mZxid	最后修改节点时的事务ID
mZxid	最后修改节点时的事务ID
pZxid	表示该节点的子节点列表最后一次修改的事务ID，添加子节点或删除子节点就会影响子节点列表，但是修改子节点的数据内容则不影响ID（注意，只有子节点列表变更了才会变更pzxid，子节点内容变更不会影响pzxid）
cversion	子节点版本号，子节点每次修改版本号加1
dataVersion	数据版本号，数据每次修改该版本号加1
aclVersion	权限版本号，权限每次修改该版本号加1
ephemeralOwner	创建该临时节点的会话的sessionID。（如果该节点是持久节点，那么这个属性值为0）
dataLength	该节点的数据长度
numChildren	直接子节点的数量
```



### ACL（访问控制列表）

ZooKeeper采用 ACL（Access Control Lists）策略来进行权限控制，类似于Linux文件系统的权限控制。
ACL权限控制，主要包括3个方面： 授权策略（Scheme）、授权对象（ID）以及授权权限（Permission）。ZooKeeper的权限控制是基于每个Znode的，需要对每个节点设置权限，每个Znode支持设置多种权限控制方案和多个权限，子节点不会继承父节点的权限，客户端无权访问某节点，但可能可以访问它的子节点。
授权策略（Scheme）：

```java
world：开放模式，world表示任意客户端都可以访问（默认设置）。
ip：限定客户端IP防问。
auth：只有在会话中通过了认证才可以访问（通过addauth命令）。
digest：与auth类似，区别在于auth用明文密码，而digest用SHA1+base64加密后的密码（通过addauth命令，实际场景中digest更常见）。
    
    
    
    
权限位	权限	   描述
 c	  CREATE  可以创建子节点
 d	  DELET   可以删除子节点（仅直接子节点）
 r	  READ	  可以读取节点数据及显示子节点列表
 w	  WRITE	  可以设置节点数据
 a	  ADMIN	  可以设置节点访问控制列表权限
```
### 客户端命令：addauth、getAcl、setAcl

    addauth：添加认证用户。
    getAcl：获取节点的ACL，加上-s参数还可以获取Znode的状态信息。
    setAcl：设置节点的ACL，加上-s参数还可以获取Znode的状态信息，并且可以基于version设置Znode的ACL，加上-R参数(递归)可以将该节点和其所有子节点都设置成指定的ACL。
```java
addauth degist:lj:aaa
setAcl degist:
```




### sync命令

zookeeper保证   顺序一致    结果一致   sync强制同步  结果会比其他慢些

  