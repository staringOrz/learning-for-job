##Zookeeper

1. Zookeeper是什么？

   Zookeeper是一个分布式的，开放源码的分布式应用程序协调服务。是Googel的chubby一个开源项目。它是集群的管理者，监视集群中各个节点的状态根据节点的提交的反馈进行下一步合理的操作。最终，将简单易用的接口和性能高效、功能稳定的系统提供给用户

2. Zookeeper提供了什么？

   文件系统

   消息通知机制

3. zookeeper文件系统

   每个目录项的nameService都被称作为znode，和文件系统一样能够自由的增加、删除znode。唯一不同的是znode是可以存储数据的

   有四种znode

   1. presistent 持久化目录节点（客户端和zookeeper断开后节点依旧存在）
   2. 有序的持久化节点
   3. EPHEMERAL-临时节点目录节点（客户端和zookeeper断开后节点消失）
   4. 临时有序节点

4. zookeeper消息通知机制：

   客户端向zookeeper注册自己感兴趣的目录节点，当节点发送变化后，zookeeper会通知相应的客户端。

5. zookeeper还可以做配置中心

   程序总是需要配置的，如果程序分散在多台机器上，要逐个改变配置就变得困难。而把这些配置全部放在zookeeper上，保存在zookeeper上的节点上，然后让各个相应的应用程序对这些目录进行监听，一旦配置信息发生改变，每个应用程序就会收到zookeeper的通知,然后应用程序从zookeeper的节点中获得最新的配置应用到系统中。

6. zookeeper还可以进行集群管理

7. zookeeeper还可以实现分布式锁

8. zookeeper中的角色：

   1. leader(决策)
   2. follower（参与投票）
   3. observer（不参与投票，接收follwer的消息进行转发给leader）