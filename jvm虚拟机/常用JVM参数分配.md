# 常用JVM参数分配

-XX:+printGC

- 打印GC

-XX:+printGCDetails

- 打印GC详细输出

## Trace 跟踪参数

- -Xloggc:log/go.log
  1. 指定GC log的位置，以文件输出
  2. 帮助开发人员分析问题

-XX :printHeapAtGC

- 每次发生GC打印堆GC前后信息

-XX:+TraceClassLoading

- 监控类的加载

-Xmx -Xms

- 指定最大堆和最小堆

-Xmn

- 设置新生代大小

-XX:NewRatio

- 新生代（Eden+2*s）和老年区（不包含永久区）的比值
- 4表示新生代:老年代=1:4即年轻代占堆的1/5

-XX:SurvivorRatio

- 设置两个Survivor区的比
- 8表示两个Survivor:eden=2:8,即一个Survivor占年轻代的1/10

## 根据实际情况调整新生代和幸存代的大小

- 官方推荐新生代占堆的3/8
- 幸存代占新生代的1/10

-XX:permSize  -XX:MaxpermSize

- 设置永久区的初始空间和最大空间

栈大小分配（-Xss）

- 通常只有几百K
- 决定了函数调用的深度
- 每个线程都有独立的栈空间
- 局部变量、参数分配在栈上







