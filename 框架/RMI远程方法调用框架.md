#RMI远程方法调用框架

RMI（remote method invocation） 可以认为是RPC的java版本

RMI使用的是JRMP(Java Remote Messageing Protocol),JRMP是专门为java定制的通信协议，所以是纯java 的分布式解决方案

##如何实现一个RMI程序

1. 创建远程接口，并实现java.rmi.Remote
2. 实现远程接口，并继承UnicastRemoteObject
3. 创建服务器程序：createRegister方法注册远程对象，让客户端能够通过stubs获得,远程对象
4. 创建客户端程序，客户端通过Naming.lookup得到相应的远程对象

![1533893131882](E:\mydairy\框架\RMI.png)

##RMI的局限性

1. 不支持跨语言，只能实现Java系统之间的调用，而WebService可以实现跨语言实现系统之间的调用
2. RMI使用JAVA默认的序列化方式，对应性能要求比较高的系统，可能需要其他的序列化方案来解决
3. RMI服务在运行时难免会存在故障，没有重连等安全防范机制
4. 底层使用阻塞IO，相比非阻塞io效率低下。