# rabbitMQ

**概念**：是一个由 Erlang 语言开发的 AMQP 的开源实现。 AMQP:Advanced Message Queue，高级消息队列协议(Advanced Message Queuing Protoco)

**作用**：接受、存储和转发二进制数据块 -*消息*。

​		RabbitMQ 用于分布式系统中存储转发消息，在易用性、扩展性、高可用性等方面表现不俗

**组成**：  RabbitMQ 内部几个重要结构：  交换机组件对象、  路由组件对象、队列组件对象
Exchange交换机组件对象： 用来和消息发送端(生产者)建立连接并接收消息 
Routes路由组件对象:  用来指定以什么策略将消息传递到队列中(消息传递策略自己找资料了解)
Queue队列组件对象： 用来给消息的接收端(消费者)提供消息 (接收端用监听器从队列取数据)





