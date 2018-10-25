#Bigo面试

##第一面

数据库四大特性ACID （原子性、一致性、隔离性、持久性）

- 原子性：事务是数据库的逻辑工作单位，要么全部执行，要么都不执行
- 一致性：事务前后，数据库的状态满足所有完整性约束
  - 完整性约束指的是
    - 实体完整性：每一个元组是可区分的，唯一的，若属性A是关系R的主属性，则不能为空。
    - 参照完整性：属性与属性的关系，如外键
    - 用户定义完整性：就是针对某一具体关系数据库的约束条件，反应某一具体属性数据满足语义要求，如unique,not null等等
- 隔离性：并发执行的事务是隔离的，一个事务的执行不会被另一个事务影响，也不会对另一个事务产生影响
- 持久性：在事务完成之后，该事务对数据库的影响是永久的，永久的保存在数据库中

数据库隔离级别（读未提交，读已提交，可重复读，可串行化）

|                               | **脏读 Dirty Read** | **不可重复读 Non-Repeatable Read** | **错误读取/幻读/虚读 Phantom Read** |
| ----------------------------- | ------------------- | ---------------------------------- | ----------------------------------- |
| **读未提交 Read Uncommitted** | **√**               | **√**                              | **√**                               |
| **读已提交 Read Committed**   | **×**               | **√**                              | **√**                               |
| **可重复读 Repeatable Read**  | **×**               | **×**                              | **√**                               |
| **可串行化 Serializable**     | **×**               | **×**                              | **×**                               |

- 脏读：(如有事务A和B，A读取了B未提交的数据)   A向B转账100，当B用户增加一百块时候，A的用户还没减少时通知B去确认钱已经到账，而实际上，在A用户账户减少100块的sql语句执行出现了某种错误，导致事务回滚，当B再次查看的时候就会发现钱并没有到账。造成脏读。
- 不可重复读：(如有事务A和B，A负责读取，B负责写入，A连续读的过程中B写入了一次，A前后两次读出来的数据不一样) 
- 幻读：A事务查询多行数据时，A再次查询时，B事务向A事务插入或者删除了某条数据，导致A两次查询出来的行数不一致，产生幻读。
- 串行化：SERIALIZABLE是最高的隔离级别，它通过强制事务串行执行（注意是串行） ），避免了前面的幻读情况，由于他大量加上锁，导致大量的请求超时，因此性能会比较底下，再特别需要数据一致性且并发量不需要那么大的时候才可能考虑这个隔离级别 
- 可串行化调度：多个事务的并发执行是正确的，当且仅当其结果与按某一次序串行执行这些事务的结果是一样的，称这种调度是可串行化调度

 ++i和i++区别，是不是原子操作？

```java
//表面的理解
i=++i//先运算再赋值    先自增,再取值
i=i++//先赋值后运算    先取值再自增;

//底层是这样的
i=++i;
i=i+1;
i=i;

i=i++;
_temp=i;
i=i+1;
i=temp;

效率上了说前者效率更高，后者多了一份I的拷贝
```

- 他们都不是原子操作

什么是悲观锁、乐观锁、synchronized是悲观锁还是乐观锁，synchronized怎么使用？

- 悲观锁(Pessimistic Lock), 顾名思义，就是很悲观，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会block直到它拿到锁。传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在做操作之前先上锁。
- 乐观锁(Optimistic Lock), 顾名思义，就是很乐观，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号等机制。乐观锁适用于多读的应用类型，这样可以提高吞吐量，像数据库如果提供类似于write_condition机制的其实都是提供的乐观锁。
- synchronized是悲观锁
- synchronized可以作用于代码块，对象，方法，静态变量

synchronized对代码块、方法、实例对象、静态变量加锁所持有的是什么样的锁？

- synchronized修饰方法，代码块所持有的锁是对象锁
- synchronized修饰实例对象，所持有的锁是实例对象锁
- synchronized修饰静态变量，该静态变量，实例唯一，锁持有的锁也是该实例对象锁

怎么会造成死锁？死锁的四大条件

互斥条件 ，不可剥夺条件 ，请求和保持条件 ，循环等待条件 。

这四大条件都满足就会造成死锁。

死锁的简单实现（手撕代码）

```java
public class deadlock {
    static String a="lock1";
    static String b="lock2";
    public static void main(String[]args){
        Thread thread1=new Thread(){
            public void run(){
                synchronized (a){
                    System.out.println("lock1 is locked by Lock1");
                    try {
                        Thread.sleep(10000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    synchronized (b){
                        System.out.println("lock2 is locked by Lock1");
                    }
                }
            }
        };
        Thread thread2=new Thread(){
            public void run(){
                synchronized (b){
                    System.out.println("lock2 is locked by Lock2");
                    try {
                        Thread.sleep(1000);
                    }catch(InterruptedException e){
                        e.printStackTrace();
                    }
                    synchronized (a){
                        System.out.println("lock1 is locked by Lock2");
                    }
                }
            }
        };
        thread1.start();
        thread2.start();
    }
}

```



堆排序，快速排序，归并排序复杂度，适用场景，优缺点

平均复杂度都说NlogN,堆排序和快速排序是不稳定的排序方法，归并排序是稳定的排序方法

堆排序与快速排序相比，快速排序所需要的辅助空间较大，而且堆排序不会出现快速排序的最坏情况O（n^2）的复杂度

手写堆排序的建堆过程

```java
import java.io.OutputStream;
import java.io.Writer;

public class Heapsort {
    public static void main(String[]arg){
        int a[]={1,3,4,2,7,45,4,6,9,0,2};
        Heapsort heapsort=new Heapsort();
        heapsort.heapSort(a);
        for(int i:a){
            System.out.print(i+" ");
        }
    }
    public void  heapSort(int[] a){
        buildMaxHeap(a);
        for(int i=a.length-1;i>=1;i--){
            int temp=a[i];
            a[i]=a[0];
            a[0]=temp;
            adjust(a,0,i);
        }
    }

    private int[] buildMaxHeap(int[] a) {
        for(int i=(a.length-2)/2;i>=0;i--){
            adjust(a,i,a.length);
        }
        return a;
    }


    private void adjust(int[] a, int k, int length) {
        int temp=a[k];
        for(int i=2*k+1;i<=length-1;i=2*i+1){
            if(i+1<length&&a[i+1]>a[i]){
                i++;
            }
            if(temp>=a[i]){
                break;
            }else {
                a[k]=a[i];
                k=i;
            }
        }
        a[k]=temp;
    }
}

```



手写两个队列实现一个栈操作

```java
import java.util.LinkedList;
import java.util.Queue;

public class StackByTwoQueue {
    private Queue<String> queue1=new LinkedList<String>();
    private Queue<String> queue2=new LinkedList<String>();
    public void push(String str){
        if(queue1.isEmpty()){
            queue2.add(str);
        }
        if(queue2.isEmpty()){
            queue1.add(str);
        }
    }
    public String pop(){
        if(queue2.isEmpty()&&queue2.isEmpty()){
            try {
                throw new Exception("stack is empty");
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
        if(queue1.isEmpty()){
            while (queue2.size()>1){
                queue1.add(queue2.poll());
            }
            return queue2.poll();
        } else if(queue2.isEmpty()){
            while (queue1.size()>1){
                queue2.add(queue1.poll());
            }
            return queue1.poll();
        }
        return null;
    }
}
```

描述一下，互联网一个url访问全过程？

url经过浏览器的DNS域名解析系统得到对应url 的服务器地址IP，与该服务器通过三次握手建立TCP连接，然后浏览器通过HTTP协议把向服务器发起请求，服务器响应回复请求。

##第二面

内部类（普通内部类，静态内部类，匿名内部类）区别，怎么用，外部变量有什么要求？

- 成员内部类 在成员内部类中要注意两点
  - **第一：**成员内部类中不能存在任何static的变量和方法；（因为成员内部类需要先创建外部类，才能创建它自己的 ）
  - **第二：**成员内部类是依附于外围类的，所以只有先创建了外围类才能够创建内部类 
- 定义在方法中的内部类是局部内部类
  - 没有访问修饰符
  - 在外部类中不能创建内部类实例
  - 创建内部类的实例只能在包含它的方法中
  - 内部类访问包含他是方法中的变量必须有final修饰
  - 外部类不能访问局部内部类，只能在方法体中访问局部内部类，且访问必须在内部类定义之后
- 匿名内部类
  - 匿名内部类是没有访问修饰符的。 
  -  new 匿名内部类，这个类首先是要存在的。 
  - 匿名内部类的形参必须是被final 修饰
  - 匿名内部类没有构造方法（连名字都没有何来构造方法）
- 静态内部类
  - 它的创建是不需要依赖于外围类的。
  -  它不能使用任何外围类的非static成员变量和方法。

线程池有多少种，有什么区别，具体参数怎么用，参数代表什么意思？

4种

- **newCachedThreadPool**（ 创建一个可缓存线程池，如果线程池长度超过处理需要，可灵活回收空闲线程，若无可回收，则新建线程。 ）

  - 工作线程的创建数量几乎没有限制(其实也有限制的,数目为Interger. MAX_VALUE), 这样可灵活的往线程池中添加线程。
  - 如果长时间没有往线程池中提交任务，即如果工作线程空闲了指定的时间(默认为1分钟)，则该工作线程将自动终止。终止后，如果你又提交了新的任务，则线程池重新创建一个工作线程。
  - 在使用CachedThreadPool时，一定要注意控制任务的数量，否则，由于大量线程同时运行，很有会造成系统瘫痪。

- ## **newFixedThreadPool** （创建一个指定工作线程数量的线程池 ）

  - 每当提交一个任务就创建一个工作线程，如果工作线程数量达到线程池初始的最大数，则将提交的任务存入到池队列中。

    FixedThreadPool是一个典型且优秀的线程池，它具有线程池提高程序效率和节省创建线程时所耗的开销的优点。但是，在线程池空闲时，即线程池中没有可运行任务时，它不会释放工作线程，还会占用一定的系统资源。

- ## **newSingleThreadExecutor**（创建一个单线程化的Executor，即只创建唯一的工作者线程来执行任务，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序(FIFO, LIFO, 优先级)执行）

  - 如果这个线程异常结束，会有另一个取代它，保证顺序执行。单工作线程最大的特点是可保证顺序地执行各个任务，并且在任意给定的时间不会有多个线程是活动的。 

- ## **newScheduleThreadPool**  （创建一个定长的线程池，而且支持定时的以及周期性的任务执行，支持定时及周期性任务执行 ）

参数有七大参数：

- corePollSize：核心线程数。在创建了线程池后，线程中没有任何线程，等到有任务到来时才创建线程去执行任务。默认情况下，在创建了线程池后，线程池中的线程数为0，当有任务来之后，就会创建一个线程去执行任务，当线程池中的线程数目达到corePoolSize后，就会把到达的任务放到缓存队列当中。
- maximumPoolSize：最大线程数。表明线程中最多能够创建的线程数量。
- keepAliveTime：空闲的线程保留的时间。
- TimeUnit：空闲线程的保留时间单位。
- BlockingQueue<Runnable>：阻塞队列，存储等待执行的任务。参数有ArrayBlockingQueue、LinkedBlockingQueue、SynchronousQueue可选。
- ThreadFactory：线程工厂，用来创建线程
- RejectedExecutionHandler：队列已满，而且任务量大于最大线程的异常处理策略。有以下取值

进程通信和线程通信有什么区别，有什么方式？

- 进程通信
  - 管道pipe
  - 有名管道哦
  - 共享内存
  - 信号量
  - 信号
  - 消息队列
- 线程通信
  - 使用wait/notify实现线程间的通信
  - 管道pipe

int i=12;System.out.println(i*=++i);我做错了

i=i*(i=i+1);

长连接和短连接是什么、区别？

手写反转链表（多种方法实现，栈）

volatile有什么用，特性，使用这个关键词是为了解决什么问题？

##第三面

进程和线程的区别 

进程是资源分配的基本单位，怎么理解，什么是资源？

线程它也有变量，资源来于那里？

线程什么时候会被分配资源，怎么理解？PCB

TCP，UDP的区别，TCP怎么保证可靠性？

接收方接到的包无序，怎么确保有序性？

TCP有没有包长度，是多少？

TCP存在粘包，什么场景下出现，UDP有没有这样的问题？

存在粘包的时候怎么解决？

如何设计一个操作系统？操作系统有什么，你是怎么想的，为什么会有这种东西？

中断是什么？

自己写过的大于200行的代码程序是什么？

看过什么书？jvm,他说jvm的东西都是固定，没什么问的价值。

计算机网络，问那些设计比较精妙，说说看？ip,测试联通性，ping,出错的时候，icmp协议，回传错误信息。

给我多个子网，怎么从一个子网的主机到另一个子网的主机，访问过程是怎么的？

虚拟内存怎么理解？什么是虚拟内存，概念，作用。如果内存足够大，还需不需要虚拟内存技术？虚拟内存技术主要解决什么问题？

你的职业规划是什么？

有什么问题？





