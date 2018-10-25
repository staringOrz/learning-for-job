# Object类源码解读

```java
 /**
     * 一个本地方法,具体是用C(C++)在DLL中实现的,然后通过JNI调用
     */
    private static native void registerNatives();

    /**
     * 对象初始化时自动调用此方法
     */
    static {
        registerNatives();
    }
```

- getClass()方法:返回对象Object的运行时类
- int hashcode()方法:返回一串hashcode数字
- boolean equals(Object o)方法:比较类对象是否相等，返回boolean 型
- Object clone()方法：克隆/复制当前对象，放回对象的类
- String toString() 方法：返回类名路径加16进制的hashcode
- notify()方法：不能被重写，用于唤醒一个在因等待该对象（调用了wait方法）被处于等待状态（waiting 或 time_wait）的线程，该方法只能同步方法或同步块中调用
- notifyAll()方法：不能被重写，用于唤醒所有在因等待该对象（调用wait方法）被处于等待状态（waiting或time_waiting）的线程，该方法只能同步方法或同步块中调用
- wait()方法：不能被重写，用于在线程调用中，导致当前线程进入等待状态（time_waiting)，timeout单位为毫秒,该方法只能同步方法或同步块中调用,超过设置时间后线程重新进入可运行状态
- finalize()方法：这个方法用于当对象被回收时调用，这个由JVM支持，Object的finalize方法默认是什么都没有做，如果子类需要在对象被回收时执行一些逻辑处理，则可以重写finalize方法。