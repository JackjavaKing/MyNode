# 做hadoop集群时的错误：

### 一、在配置虚拟机固定ip时的错误：

![image-20220606190815566](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220606190815566.png)

**网关错误会引起虚拟机无法上网，原因？**

在虚拟机中的

![image-20220606191026541](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220606191026541.png)

上图中，为什么

![image-20220606191103881](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220606191103881.png)

![image-20220606191128536](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220606191128536.png)

```apache
理论上192.168.2.2-255之间都可以
```



Match Group sftp
ChrootDirectory  /usr/sftp
ForceCommand internal-sftp
AllowTcpForwarding   no
XllForwarding  no