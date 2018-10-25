# Join&Threadlocal

## join()与sleep()的区别

- 方法join()的作用是等待线程对象销毁，该方法内部调用wait()方法等待线程对象销毁，会释放锁资源给其它线程使用。
- 方法sleep()线程阻塞设定的时间后再执行，不释放锁资源。

## 方法Threadlocal（）

- Threadlocal类可以比喻成全局存放数据的盒子，盒子中可以存储每个线程的私有数据
- Threadlocal类解决了变量在不同线程间的隔离性，也就是说不同线程拥有自己的值，不同线程的值可以放入Threadlocal类中进行保存