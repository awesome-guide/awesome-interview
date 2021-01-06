

## 为什么wait和notify方法要在同步块中调用



## Lost Wake-Up Problem

事情得从一个多线程编程里面臭名昭著的问题"Lost wake-up problem"说起。

这个问题并不是说只在Java语言中会出现，而是会在所有的多线程环境下出现。

假如有两个线程，一个消费者线程，一个生产者线程。生产者线程的任务可以简化成将count加一，而后唤醒消费者；消费者则是将count减一，而后在减到0的时候陷入睡眠：

生产者伪代码：

```
count+1;notify();
```

消费者伪代码：

```
while(count<=0)   wait()
count--
```

熟悉多线程的朋友一眼就能够看出来，这里面有问题。什么问题呢？

生产者是两个步骤：

1. count+1;
2. notify();

消费者也是两个步骤：

1. 检查count值；
2. 睡眠或者减一；

万一这些步骤混杂在一起呢？比如说，初始的时候count等于0，这个时候消费者检查count的值，发现count小于等于0的条件成立；就在这个时候，发生了上下文切换，生产者进来了，噼噼啪啪一顿操作，把两个步骤都执行完了，也就是发出了通知，准备唤醒一个线程。这个时候消费者刚决定睡觉，还没睡呢，所以这个通知就会被丢掉。紧接着，消费者就睡过去了……

![阿里面试题，为什么wait()方法要放在同步块中？](https://www.javazhiyin.com/wp-content/uploads/2019/04/java1-1555167532.jpg)

这就是所谓的lost wake up问题。

## 那么怎么解决这个问题呢？

现在我们应该就能够看到，问题的根源在于，消费者在检查count到调用wait()之间，count就可能被改掉了。

这就是一种很常见的竞态条件。

很自然的想法是，让消费者和生产者竞争一把锁，竞争到了的，才能够修改count的值。

于是生产者的代码是:

```
tryLock()
count+1
notify()
releaseLock()
```

消费者的代码是:

```
tryLock()
while(count <= 0)  wait()
count-1
releaseLock
```

注意的是，我这里将两者的两个操作都放进去了同步块中。

现在来思考一个问题，生产者代码这样修改行不行？

```
tryLock()
count+1
notify()
releaseLock()
```

答案是，这样改毫无卵用，依旧会出现lost wake up问题，而且和无锁的表现是一样的。

## 终极答案

所以，我们可以总结到，为了避免出现这种lost wake up问题，在这种模型之下，总应该将我们的代码放进去的同步块中。

Java强制我们的wait()/notify()调用必须要在一个同步块中，就是不想让我们在不经意间出现这种lost wake up问题。

不仅仅是这两个方法，包括java.util.concurrent.locks.Condition的await()/signal()也必须要在同步块中：

```
    private ReentrantLock lock = new ReentrantLock();
    private Condition condition = lock.newCondition();
    @Test    public void test() {        try {            condition.signal();        } catch (Exception e) {            e.printStackTrace();        }    }
```

![阿里面试题，为什么wait()方法要放在同步块中？](https://www.javazhiyin.com/wp-content/uploads/2019/04/java1-1555167533.jpg)

准确的来说，即便是我们自己在实现自己的锁机制的时候，也应该要确保类似于wait()和notify()这种调用，要在同步块内，防止使用者出现lost wake up问题。

Java的这种检测是很严格的。它要求的是，一定要处于锁对象的同步块中。举例来说：

```
    private Object obj = new Object();
    private Object anotherObj = new Object();
    @Test    public void produce() {        synchronized (obj) {            try {                anotherObj.notify();            } catch (Exception e) {                e.printStackTrace();            }        }    }
```

这样是没有什么卵用的。一样出现IllegalMonitorStateException。