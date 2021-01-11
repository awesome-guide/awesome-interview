# 多线程里面对一个整型做加减为啥不能只用volatile

如果一个变量加了volatile关键字，就会告诉编译器和JVM的内存模型：这个变量是对所有线程共享的、可见的，每次jvm都会读取最新写入的值并使其最新值在所有CPU可见。

一个volatile的integer自增（i++），分3步：

1. 读取volatile变量值到local；
2. 增加变量的值
3. 把local的值写回，让其它的线程可见。

这3步的jvm指令为：

```
mov    0xc(%r10),%r8d ; Load
inc    %r8d           ; Increment
mov    %r8d,0xc(%r10) ; Store
lock addl $0x0,(%rsp) ; StoreLoad Barrier
```

最后一步是内存屏障。



**内存屏障和volatile的关系**：

Java内存模型将在写操作后插入一个写屏障指令，在读操作前插入一个读屏障指令。

1、一旦你完成写入，任何访问这个字段的线程将会得到最新的值。

2、在你写入前，会保证所有之前发生的事已经发生，并且任何更新过的数据值也是可见的，因为内存屏障会把之前的写入值都刷新到缓存。

---

上面的JVM指令：从Load到store到内存屏障，一共4步，其中最后一步jvm让这个最新的变量的值在所有线程可见，也就是最后一步让所有的CPU内核都获得了最新的值，但 **中间的几步（从Load到Store）** 是不安全的，中间如果其他的CPU修改了值将会丢失。

下面的测试代码可以实际测试voaltile的自增没有原子性：

```java
private static volatile long _longVal = 0;

private static class LoopVolatile implements Runnable {
  public void run() {
    while (val < 1000_0000) {
      _longVal++;
    }
  }
}

private static class LoopVolatile2 implements Runnable {
  public void run() {
    while (val < 1000_0000) {
      _longVal++;
    }
  }
}

public static void main(String[] args) {
  Thread t1 = new Thread(new LoopVolatile());
  t1.start();

  Thread t2 = new Thread(new LoopVolatile2());
  t2.start();

  while (t1.isAlive() || t2.isAlive()) {}

  System.out.println("final val is: " + _longVal);
}
```
