

#  为什么wait()和notify()需要搭配synchonized关键字使用



请预测一下下面一段程序在 `10 秒之内` 的输出结果是什么，如果判断正确的话， 那么后续的内容介绍应该不会令你感到困惑

```java
package com;

public class SyncThread implements Runnable {
    private static int count;
    private static String lock = "lock";
    public SyncThread() {
        count = 0;
    }

    public void run() {
        synchronized (lock) {
            System.out.printf( Thread.currentThread().getName() + "enter ");
            try {
                lock.wait(10*1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
    public static void main(String[] args) {
        SyncThread syncThread = new SyncThread();
        Thread thread1 = new Thread(new SyncThread(), "SyncThread1");
        Thread thread2 = new Thread(new SyncThread(), "SyncThread2");
        thread1.start();
        thread2.start();
    }
}
123456789101112131415161718192021222324252627
```

答案是：

```
SyncThread1 enter 
SyncThread2 enter 
12
```

## 先修知识：

- `synchronized` 的含义：
  - Java中每一个对象都可以成为一个监视器（`Monitor`）, 该Monitor由一个锁（lock）, 一个等待队列（waiting queue ）, 一个入口队列( entry queue).
  - 对于一个对象的方法， 如果没有`synchronized`关键字， 该方法可以被任意数量的线程，在任意时刻调用。
  - 对于添加了`synchronized`关键字的方法，任意时刻只能被**唯一的一个**获得了**对象实例锁**的线程调用。
  - `synchronized`用于实现多线程的同步操作
- `wait()`功用
  - `wait()`, `notify()`, `notifyAll()` 和 `synchonized` 需要搭配使用， 用于线程同步
  - wait()总是在一个循环中被调用，挂起当前线程来等待一个条件的成立。 Wait调用会一直等到其他线程调用`notifyAll()`时才返回。
  - 当一个线程在执行`synchronized` 的方法内部，调用了`wait()`后， 该线程会释放该对象的锁， 然后该线程会被添加到该对象的等待队列中（waiting queue）, 只要该线程在等待队列中， 就会一直处于闲置状态， 不会被调度执行。 要注意`wait()`方法会强迫线程先进行释放锁操作，所以在调用`wait()`时， 该线程必须已经获得锁，否则会抛出异常。由于`wait()`在synchonized的方法内部被执行， 锁一定已经获得， 就不会抛出异常了。
    ![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcwOTIzMTUzNzI5MDkz?x-oss-process=image/format,png)
- `notify()`的功用
  - `wait()`, `notify()`, `notifyAll()` 和 `synchonized` 需要搭配使用， 用于线程同步
  - 当一个线程调用一个对象的`notify()`方法时， 调度器会从所有处于该对象等待队列（waiting queue）的线程中取出**任意一个**线程， 将其添加到入口队列( entry queue) 中. 然后在入口队列中的多个线程就会竞争对象的锁， 得到锁的线程就可以继续执行。 如果等待队列中（waiting queue）没有线程， `notify()`方法不会产生任何作用
  - `notifyAll()` 和`notify()`工作机制一样， 区别在于`notifyAll()`会将等待队列（waiting queue）中所有的线程都添加到入口队列中（entry queue）
  - 注意, `notifyAll()`比`notify()`更加常用， 因为`notify()`方法只会唤起一个线程， 且无法指定唤醒哪一个线程，所以只有在多个执行相同任务的线程在并发运行时， 我们不关心哪一个线程被唤醒时，才会使用`notify()`

## 为什么`wait()`和`notify()`或`notifyAll()`需要搭配`synchronized`关键字使用

- 从语义角度来讲， 一个线程调用了`wait()`之后， 必然需要由另外一个线程调用`notify()`来唤醒该线程， 所以本质上， `wait()`与`notify()`的成对使用， 是一种线程间的通信手段。
- 进一步分析， `wait()` 操作的调用必然是在等待某种条件的成立， 而条件的成立必然是由其他的线程来完成的。 所以实际上， 我们调用 wait() 的时候， 实际上希望达到如下的效果

```Java
// 线程A 的代码
while(!condition){ // 不能使用 if , 因为存在一些特殊情况， 使得线程没有收到 notify 时也能退出等待状态
    wait();
}
// do something
12345
// 线程 B 的代码
if(!condition){ 
	// do something ...
    condition = true;
    notify();
}
123456
```

- 现在考虑， 如果`wait()` 和 `notify()` 的操作没有相应的同步机制， 则会发生如下情况

1. 【线程A】 进入了 while 循环后（通过了 `!condition` 判断条件， 但尚未执行 `wait` 方法）, CPU 时间片耗尽， CPU 开始执行线程B的代码
2. 【线程B】 执行完毕了 `condition = true; notify();` 的操作， 此时【线程A】的 `wait()` 操作尚未被执行， `notify()` 操作没有产生任何效果
3. 【线程A】执行`wait()` 操作， 进入等待状态，如果没有额外的 notify() 操作， 该线程将持续在 `condition = true` 的情形下， 持续处于等待状态得不到执行。

由此看出， 在使用 wait() 和 notify() 这种会挂起线程的操作时， 我们需要一种同步机制保证， `condition` 的检查与 `wait()` 操作， 以及 `condition` 的更新与 `notify()` 是互斥的。

- 那是否简单的将之前的代码包裹在一个 synchronized 代码块中就可以满足需求呢？ 像下面这样。

```Java
// 线程A 的代码
synchronized(obj_A)
{
	while(!condition){ 
	    wait();
	}
	// do something 
}
12345678
// 线程 B 的代码
synchronized(obj_A)
{
	if(!condition){ 
		// do something ...
	    condition = true;
	    notify();
	}
}
123456789
```

- 乍一看， 上述的代码可以解决问题， 但是仔细分析一下， 由于`wait()` 操作会挂起当前线程， 那么必然需要在挂起前释放掉 `obj_A` 的锁， 但如果 `obj_A` 允许是任意对象， `wait()` 函数作为一个没有参数输入的方法，无从得知应该释放哪个对象的锁 。于是很自然的， 语法就会被设计成 java 现在的样子。即基于对象的 `wait()` 与 `notify()` 的调用， 必须先获得该对象的锁。
- 正确的用法示例如下

```
// 线程 A 的代码
synchronized(obj_A)
{
	while(!condition){ 
	    obj_A.wait();
	}
	// do something 
}
12345678
// 线程 B 的代码
synchronized(obj_A)
{
	if(!condition){ 
		// do something ...
	    condition = true;
	    obj_A.notify();
	}
}
```