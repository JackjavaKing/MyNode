# SQL查询优化

## 简介

虽然sql查询优化的技术有很多，但是大方向上完全可以分为**物理查询优化**和**逻辑查询优化**两块。

①物理查询优化是通过**索引**和**表连接**方式等技术来进行优化，这里重点需要掌握索引的使用。

②逻辑查询优化是通过**sql等价变换**提升查询效率，直白说就是换一种**查询写法**执行效率可能更高

## 测试过程

![image-20220815160658365](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220815160658365.png)

创建一个测试用的数据库

SELECT sqlyou.sysobjects.name as Table_name, sqlyou.syscolumns.name AS Column_name

FROM sqlyou.syscolumns INNER JOIN

sqlyou.sysobjects ON sqlyou.syscolumns.id = sqlyou.sysobjects.id

WHERE sqlyou.sysobjects.name='TM_User'and (sqlyou.sysobjects.xtype = 'u') AND (NOT (sqlyou.sysobjects.name LIKE 'dtproperties'))