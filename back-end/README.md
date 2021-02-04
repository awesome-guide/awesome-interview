## 后端技术栈分享内容参考列表

[TOC]

### 基础知识

#### Java基础

##### jdk

1. [HashMap 与 ConcurrentHashMap 的实现原理是怎样的？](集合/HashMap.md)[ConcurrentHashMap 是如何保证线程安全的？](集合/CurrentHashMap.md)![](../images/草莓.png)
2. [HashMap底层实现，为什么扩容是2的幂次](集合/HashMap为什么扩容是2的幂次.md)；[为什么是0.75f](集合/HashMap负载因子为什么是0.75f)，[1.7与1.8的区别](集合/HashMap1.7与1.8的区别.md) `@A`![](../images/草莓.png)
3. [hashmap 和 hashtable 的区别是什么？](集合/hashmap和hashtable的区别是什么.md)`@A`![](../images/草莓.png)
4. [HashMap 实现原理，为什么使用红黑树？](集合/HashMap实现原理，为什么使用红黑树.md)`@A`![](../images/草莓.png)
5. [ArrayList集合加入1万条数据，应该怎么提高效率](集合/ArrayList集合加入1万条数据，应该怎么提高效率.md)`@A`![](../images/草莓.png)
6. [可以用for循环直接删除ArrayList的特定元素吗？可能会出现什么问题？怎样解决](集合/可以用for循环直接删除ArrayList的特定元素吗？可能会出现什么问题？怎样解决.md) `@A`![](../images/草莓.png)
7. [简述 ArrayList 与 LinkedList 的底层实现以及常见操作的时间复杂度](集合/简述ArrayList与LinkedList的底层实现以及常见操作的时间复杂度.md)@A![](../images/草莓.png)
8. [为什么 char 数组比 Java 中的 String 更适合存储密码？](JVM/为什么char数组比Java中的String更适合存储密码.md) `@B`![](../images/柠檬.png)
9. [为什么 String 在 Java 中是不可变的？](JVM/String为什么不可变.md) `@B` ![](../images/柠檬.png)
10. [你对String对象的intern()熟悉么?](JVM/你对String对象的intern熟悉么.md) ![](../images/柠檬.png)
11. [为什么Java不支持运算符重载？](JVM/为什么Java不支持运算符重载.md) `@B`![](../images/柠檬.png)
12. [为什么Java中不支持多重继承？](JVM/Java为什么不支持多继承.md) `@B`![](../images/柠檬.png)
13. [Java类初始化顺序](JVM/类的初始化流程.md) `@B`![](../images/柠檬.png)
14. [Java 类的加载流程是怎样的？什么是双亲委派机制？](JVM/类的加载过程双亲委派.md) `@B` ![](../images/柠檬.png)
15. [如果你的Serializable类包含一个不可序列化的成员，会发生什么？你是如何解决的？](JVM/Serializable类包含一个不可序列化的成员会发生什么.md) `@B` ![](../images/柠檬.png)
16. [char 型变量中能不能存贮一个中文汉字，为什么？](JVM/char型变量中能不能存贮一个中文汉字.md)![](../images/柠檬.png)
17. [java 创建对象的几种方式](JVM/java创建对象的几种方式.md)![](../images/柠檬.png)
18. [ThreadLocal实现原理是什么？](JVM/ThreadLocal实现原理是什么.md)@C![](../images/西瓜.png)
19. [Java 如何高效进行数组拷贝](JVM/Java如何高效进行数组拷贝.md) @C![](../images/西瓜.png)
20. [CAS 实现原理是什么？](JVM/CAS实现的原理是什么.md) @C![](../images/西瓜.png)
22. [Java 缓冲流 buffer 的用途和原理是什么？](JVM/Java缓冲流buffer的用途和原理是什么.md) @C![](../images/西瓜.png)
23. [简述 Java 的 happen before 原则](JVM/简述Java的happenBefore原则.md) @C![](../images/西瓜.png)
24. final 内存语义？final有哪些用法，什么时候用，使用时需要考虑的问题@D![](../images/牛油果.png)
25. 原子操作类底层实现机制？自增操作是怎么保证原子性的？@D![](../images/牛油果.png)
26. [Collections.sort和Arrays.sort的实现原理](集合/Collections.sort和Arrays.sort的实现原理.md)@D![](../images/牛油果.png)
27. [poll()方法和 remove()方法的区别？](集合/poll()方法和remove()方法的区别.md)@D![](../images/牛油果.png)
28. [private修饰的方法可以通过反射访问，那么private的意义是什么](JVM/private修饰的方法可以通过反射访问，那么private的意义是什么.md)@D![](../images/牛油果.png)
29. [String a = "ab"; String b = "a" + "b"; a == b 是否相等，为什么!](JVM/String字符串问题.md)@D![](../images/牛油果.png)
30. [short s1=1; s1 = s1 + 1;该段代码是否有错,有的话怎么改？short s1=1; s1 += 1; 该段代码是否有错,有的话怎么改？](JVM/short类型相加.md)@D![](../images/牛油果.png)
32. [a=a+b与a+=b有什么区别吗?](JVM/a=a+b与a+=b有什么区别吗.md)@D ![](../images/牛油果.png)
33. [a.hashCode()有什么用?与a.equals(b)有什么关系,equals()和hashcode的区别!](集合/hashCode和equals.md)@D![](../images/牛油果.png)
34. 局部变量使用前需要显式地赋值，否则编译通过不了，为什么这么设计@E![](../images/葡萄.png)
36. [int.a=1是原子性操作吗](基础知识/int a=1;是原子性操作吗.md)@E![](../images/葡萄.png)
37. [简述Java的反射机制](简述Java的反射机制.md)@E![](../images/葡萄.png)
38. [简述 SortedSet 实现原理](基础知识/简述SortedSet实现原理.md)@E![](../images/葡萄.png)
39. [简述使用协程的优点](基础知识/简述使用协程的优点.md)@E![](../images/葡萄.png)
40. 如何判断一个 Hash 函数好不好？![](../images/葡萄.png)
41. 3*0.1==0.3返回值是什么![](../images/葡萄.png)
42. Object中有哪些公共方法?![](../images/葡萄.png)
43. Error vs Exception的区别![](../images/葡萄.png)
44. try{} 里有一个 return 语句，那么紧跟在这个 try 后的 finally{} 里的 code 会不会被执行，什么时候被执行，在 return 前还是后?![](../images/葡萄.png)
45. Java 语言如何进行异常处理，关键字：throws、throw、try、catch、finally 分别如何使用？![](../images/葡萄.png)
46. 

##### Thread

1. [Java创建线程的方式](多线程&&并发/线程/1.java创建线程的方式.md) @F![](../images/猕猴桃.png)
2. [Java 线程间有多少通信方式？](多线程&&并发/线程/3.线程的通信方式)@F![](../images/猕猴桃.png)
3. 编写两个线程，一个线程打印1~25，另一个线程打印字母A~Z，打印顺序为12A34B56C……5152Z，要求使用线程间的通信。![](../images/猕猴桃.png)
4. [Java 线程的状态及转换](多线程&&并发/线程/2.java线程的状态及转换.md) ![](../images/猕猴桃.png)
5. [在线程中你怎么处理异常？](多线程&&并发/线程/在线程中你怎么处理异常.md)![](../images/猕猴桃.png)`
7. [线程中的wait()和sleep()方法有什么区别](多线程&&并发/线程/线程中的wait()和sleep()方法有什么区别.md)![](../images/猕猴桃.png)
8. [为什么wait, notify和notifyAll这些方法不在thread类里面?](多线程&&并发/线程/为什么wait,notify和notifyAll这些方法不在thread类里面.md)![](../images/猕猴桃.png)
8. [为什么wait和notify方法要在同步块中调用？](多线程&&并发/线程/为什么wait和notify方法要在同步块中调用.md)![](../images/猕猴桃.png)
9. [为什么wait()和notify()需要搭配synchonized关键字使用](多线程&&并发/线程/为什么wait()和notify()需要搭配synchonized关键字使用.md)
10. [讲下join,yield方法的作用,以及什么场合用它们？](多线程&&并发/线程/讲下join,yield方法的作用,以及什么场合用它们.md)![](../images/猕猴桃.png)
11. [为什么Thread类的sleep()和yield()方法是静态的](多线程&&并发/线程/为什么Thread类的sleep()和yield()方法是静态的.md)？![](../images/猕猴桃.png)
12. [Thread.sleep(0) 到底有什么用？](多线程&&并发/线程/Thread.sleep(0)有什么用.md)![](../images/猕猴桃.png)
13. [sleep、yield、wait、join、park的区别](多线程&&并发/线程/sleep、yield、wait、join的区别.md)🌰
14. [为什么Thread里面的大部分方法都是final的？](多线程&&并发/线程/为什么Thread里面的大部分方法都是final的.md)🌰
16. [多线程同步有哪几种方法？](多线程&&并发/线程/多线程同步有哪几种方法.md)🌰
17. [什么是原子性、可见性、有序性？](多线程&&并发/线程/什么是原子性、可见性、有序性.md)🌰
19. [一个线程运行时发生异常会怎样？](多线程&&并发/线程/一个线程运行时发生异常会怎样.md)🌰
20. [在线程中你怎么处理不可控制异常](多线程&&并发/线程/在线程中你怎么处理不可控制异常.md)🌰
21. [多线程中的忙循环是什么?](多线程&&并发/线程/多线程中的忙循环是什么.md)🐡
23. [线程中断是否能直接调用stop,为什么?](多线程&&并发/线程/线程中断是否能直接调用stop,为什么.md)🐡
25. [不可变对象对多线程有什么帮助？](多线程&&并发/线程/不可变对象对多线程有什么帮助.md)🐡
26. 什么是 FutureTask？@B![](../images/樱桃.png)
27. 如何在两个线程间共享数据？@B![](../images/樱桃.png)
28. [多线程里面对一个整型做加减为啥不能用volatile](JVM/多线程里面对一个整型做加减为啥不能用volatile.md) @B![](../images/樱桃.png)
29. 简述 BIO, NIO, AIO 的区别@B![](../images/樱桃.png)
30. Java 中 interrupted 和 isInterrupted 方法的区别？@B![](../images/樱桃.png)
31. CyclicBarrier 和 CountDownLatch 的区别？![](../images/樱桃.png)
32. Fork/Join 框架是干什么的？![](../images/樱桃.png)
33. 常用的线程池模式以及不同线程池的使用场景？![](../images/樱桃.png)
34. BlockingQueue的原理是什么![](../images/樱桃.png)
35. 如何确保N个线程可以访问N个资源同时又不导致死锁？![](../images/樱桃.png)
36. [有三个线程 T1，T2，T3，怎么确保它们按顺序执行？](多线程&&并发/线程/有三个线程T1,T2,T3,怎么确保它们按顺序执行.md)![img](file:///Users/zhangbo/Documents/workspace/awesome-interview/images/%E8%8A%92%E6%9E%9C.png?lastModify=1605700951)
37. [多线程交替打印ABC10次的多种实现方法(请给出至少四种实现思路)（应该有5种）](多线程&&并发/线程/多线程交替打印ABC10次的多种实现方法(请给出至少四种实现思路).md)![](../images/芒果.png)
38. 如何写代码来解决生产者消费者问题？![](../images/芒果.png)
39. 你如何确保main()方法所在的线程是Java程序最后结束的线程？![](../images/芒果.png)
40. 同步方法和同步块，哪个是更好的选择？![](../images/芒果.png)
41. 数组A内容为 1,2,3,4...52 ,数组B内容为26个英文字母，使用两个线程分别输入两个数组，打印内容为：12a34b56c78e....... 这样的规律![](../images/芒果.png)
42. 列举出一般情况下线程中断的几种方式,并说明他们之间的优缺点,并且说明那种中断方式最好![](../images/芒果.png)
43. 在Java中Lock接口比synchronized块的优势是什么？你需要实现一个高效的缓存，它允许多个用户读，但只允许一个用户写，以此来保持它的完整性，你会怎样去实现它？🍈
44. 如何用Java实现阻塞队列🍈
45. 用Java编程一个会导致死锁的程序，你将怎么解决？🍈
46. 你将如何使用thread dump？你将如何分析Thread dump？🍈
47. 你在多线程环境中遇到的常见的问题是什么？你是怎么解决它的？🍈
48. 现有的程序代码模拟产生了16个日志对象，并且需要运行16秒才能打印完这些日志，请在程序中增加4个线程去调用parseLog()方法来分头打印这16个日志对象，程序只需要运行4秒即可打印完这些日志对象。🍈

##### Lock

1. 编写 Java 程序时, 如何在 Java 中创建死锁并修复它？@C![](../images/红心柚.png)
2. 为什么Java中 wait 方法需要在 synchronized 的方法中调用？@C![](../images/红心柚.png)
3. 为什么 wait，notify 和 notifyAll 是在 Object 类中定义的而不是在 Thread 类中定义@C![](../images/红心柚.png)
4. 简述 Synchronized，volatile，可重入锁的不同使用场景及优缺点@C![](../images/红心柚.png)
5. synchronized 关键字底层是如何实现的？原理？它与 Lock 相比优缺点分别是什么？![](../images/红心柚.png)
6. JUC包下对哪些类了解，synchronized和JDK提供的锁区别![](../images/红心柚.png)
7. ReentranLock与sychronized的区别![](../images/红心柚.png)
8. volitile的内存语义，底层如何实现，为什么不能保证原子性@C![](../images/红心柚.png)
9. Java 常见锁有哪些？ReetrantLock 是怎么实现的？@D🍐
10. JAVA8的ConcurrentHashMap为什么放弃了分段锁，有什么问题吗，如果你来设计，你如何设计。@D🍐
11. 阻塞队列的实现，ArrayBlockingQueue的底层实现？@D🍐
12. 介绍一下AQS 🍐
13. AQS 中独占锁和共享锁的操作流程大体描述一下@D🍐
14. 重入锁有什么好处，什么时候考虑用   @D🍐
15. 读写锁有什么好处，什么时候考虑用？读锁是什么类型的锁，写锁呢？🍐
16. 说下读写锁里的锁降级流程，什么时候可以考虑用这个机制🍌
17. park 方法是怎么实现的🍌
18. 锁的等待通知机制 Condition 是怎么实现的，有了线程的等待通知机制为什么还要设计 Condition？🍌
19. 死锁怎么产生的，如何避免，自己写一个死锁🍌
20. 说说 Java 中有哪些锁🍌
21. 阻塞和非阻塞有什么区别，他们可以用什么方式实现🎈
22. 队列（Queue）提供哪些操作🎈
23. 阻塞队列提供了哪些获取元素的方法，有什么区别？🎈
24. 阻塞队列有哪些实现？为什么要分有界无界？🎈
25. CountDownLatch 怎么实现的，什么时候考虑用？🎈
26. CyclicBarrier 怎么实现的，什么时候考虑用？🎈
27. Semaphore 怎么实现的，什么时候考虑用？🎈
28. Semaphore拿到执行权的线程之间是否互斥🎈
29. fork/join 框架是什么？🍑
30. ReadWriteLock读写之间互斥吗🍑
31. CAS原理，ABA问题；🍑
32. 什么是公平锁？什么是非公平锁？🍑
33. Java中活锁和死锁有什么区别？🍑
34. 什么是乐观锁和悲观锁🍑



##### 模式

1. 简述常见的工厂模式以及单例模式的使用场景🌞
2. 简述生产者消费者模型，写一个生产者消费者模式🌞
3. 写一个你认为最好的单例模式🌞

##### JVM

1. JVM 中内存模型是怎样的，简述新生代与老年代的区别？新生代有哪些区，作用是什么？🌛
2. 如何判断对象是否可以被回收（1引用计数2可达性分析）🌛
3. JVM常用垃圾回收算法，讲一下CMS原理🌛
4. Java 中垃圾回收机制中如何判断对象需要回收？常见的 GC 回收算法有哪些？🌛
5. JVM 是怎么去调优的？简述过程和调优的结果🌛
6. 如何监控 GC✨
7. 常见 OutOfMemoryError 有哪些✨
8. 常见的 JDK 诊断命令有哪些，应用场景？✨
9. CPU 较高，如何定位问题✨
10. 内存占用较高，如何定位大对象✨
11. 内存泄漏时，如何实时跟踪内存变化情况✨
12. 内存泄漏时，如何定位问题代码✨
13. 大型项目如何进行性能瓶颈调优？🌲
14. 字节码是如何在 JVM 中进行流转的（栈帧）🌲
15. 方法调用的底层实现🌲
16. 方法重写和重载的实现过程🌲
17. invokedynamic 指令实现🌲
18. 如何修改字节码📚
19. JIT 参数配置如何影响程序运行？📚
20. 虚拟机有哪些性能优化策略📚
21. JVM收集器G1的内存模型和CMS的内存模型有什么不同？📚
22. 对方法区和永久区的理解以及它们之间的关系📚📚
24. CMS和G1的异同
25. G1 什么时候引发FullGC
26. 说一个最熟悉的垃圾回收算法
27. 吞吐量优先和响应优先的回收器有哪些
28. 怎么判定内存泄露
29. 讲一下CMS的流程
30. 为什么压缩指针超过32G失效
31. 什么是内存泄露？GC调优有经验吗？一般GC问题怎么解决？
32. ThreadLocal有没有内存泄露问题？
33. G1两个Region 不是连续的，而且之间有可达的引用。我现在想回收一个，另一个怎么处理？
34. 讲一下JVM堆内存管理（对象分配过程）
35. 听说过CMS并发预处理和并发可中段预处理吗
36. 到底多大的对象会被直接扔进老年代



#### 问题解决

- [Java故障排查方式有哪些如何排故查的](ops/Java故障排查.md)
- [Arthas是什么，如何使用Arthas排查线上问题，常用命令有哪些,举例说明一下](ops/Arthas.md)

#### 算法

- ▲  [10亿个数中如何高效地找到最大的一个数以及最大的第 K 个数](算法/10亿个数中如何高效地找到最大的一个数以及最大的第K个数.md)
- ▲ 最大子序和 (Leetcode)
- ▲ 爬楼梯 (Leetcode)
- ▲ 用 Rand7() 实现 Rand10() (Leetcode)
- ▲ AVL 树和红黑树有什么区别？
- ▲ 给定一个包含 40亿 个无符号整数的大型文件，使用最多 1G 内存，对此文件进行排序
- ▲ 合并两个有序链表 (Leetcode)
- ▲ 路径总和 (Leetcode)
- ▲ 如果通过一个不均匀的硬币得到公平的结果？
- ▲ 实现快速排序
- ▲ 13 数组中的第 K 个最大元素 (Leetcode)
- ▲ 13 10亿条数据包括 id，上线时间，下线时间，请绘制每一秒在线人数的曲线图
- ▲ 13 删除排序链表中的重复元素 (Leetcode)
- ▲ 11 有序链表插入的时间复杂度是多少？
- ▲ 10 Hash 表常见操作的时间复杂度是多少？遇到 Hash 冲突是如何解决的？
- ▲ 9 环形链表 (Leetcode)
- ▲ 6 常用的限流算法有哪些？简述令牌桶算法原理
- ▲ 4 简述常见的负载均衡算法
- ▲ 1 反转链表 (Leetcode)
- ▲ 1 第一个只出现一次的字符 (Leetcode)
-  顺时针打印矩阵
-  用栈实现队列 (Leetcode)
-  环形链表 (Leetcode)

#### 计算机基础

- 操作系统，进程、线程和协程的区别

- 进程间通信方式有哪些



#### 网络协议

- 简述 TCP 三次握手以及四次挥手的流程。为什么需要三次握手以及四次挥手？TCP四次挥手说一下，为什么要等待2MSL，第二次和第三次挥手是否可以合并（可以）。
- TCP3次握手过程，第三次是否可以携带数据，如何避免SYN攻击（syncookies）
- RestFul 与 RPC 的区别是什么？RestFul 的优点在哪里？
- HTTP 与 HTTPS 有哪些区别？
- RestFul 是什么？RestFul 请求的 URL 有什么特点？
- 一次 HTTP 的请求过程中发生了什么？
- TCP 与 UDP 在网络协议中的哪一层，他们之间有什么区别？
- TCP 中常见的拥塞控制算法有哪些？
- TCP 怎么保证可靠传输？
- 从系统层面上，UDP如何保证尽量可靠？
- TCP 的 keepalive 了解吗？说一说它和 http 的 keepalive 的区别？
- 简述 TCP 滑动窗口以及重传机制
- 简述 HTTP 1.0，1.1，2.0 的主要区别
- 简述 TCP 的 TIME_WAIT
- HTTP 的方法有哪些？
- 简述 TCP 协议的延迟 ACK 和累计应答
- 简述 TCP 的报文头部结构
- 简述 TCP 半连接发生场景
- 什么是 SYN flood，如何防止这类攻击？
- RPC和http的区别 
- 粘包和拆包？



### 中间件相关

#### MQ

1. [为什么使用消息队列？消息队列有什么优点和缺点？Kafka、ActiveMQ、RabbitMQ、RocketMQ 都有什么优点和缺点？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/why-mq.md)
2. [如何保证消息队列的高可用？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/how-to-ensure-high-availability-of-message-queues.md)
3. [如何保证消息不被重复消费？（如何保证消息消费的幂等性）](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/how-to-ensure-that-messages-are-not-repeatedly-consumed.md)
4. [如何保证消息的可靠性传输？（如何处理消息丢失的问题）](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/how-to-ensure-the-reliable-transmission-of-messages.md)
5. [如何保证消息的顺序性？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/how-to-ensure-the-order-of-messages.md)
6. [如何解决消息队列的延时以及过期失效问题？消息队列满了以后该怎么处理？有几百万消息持续积压几小时，说说怎么解决？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/mq-time-delay-and-expired-failure.md)
7. [如果让你写一个消息队列，该如何进行架构设计啊？说一下你的思路。](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/mq-design.md)
8. [如何从0到1设计一个MQ消息队列](http://youzhixueyuan.com/design-the-message-queue.html)

#### 搜索引擎

1. [ES 的分布式架构原理能说一下么（ES 是如何实现分布式的啊）？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/es-architecture.md)
2. [ES 写入数据的工作原理是什么啊？ES 查询数据的工作原理是什么啊？底层的 Lucene 介绍一下呗？倒排索引了解吗？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/es-write-query-search.md)
3. [ES 在数据量很大的情况下（数十亿级别）如何提高查询效率啊？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/es-optimizing-query-performance.md)
4. [ES 生产集群的部署架构是什么？每个索引的数据量大概有多少？每个索引大概有多少个分片？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/es-production-cluster.md)
5. 倒排索引与正排索引的区别

#### 缓存

1. [在项目中缓存是如何使用的？缓存如果使用不当会造成什么后果？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/why-cache.md)
2. [Redis 和 Memcached 有什么区别？Redis 的线程模型是什么？为什么单线程的 Redis 比多线程的 Memcached 效率要高得多？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/redis-single-thread-model.md)
3. [Redis 都有哪些数据类型？分别在哪些场景下使用比较合适？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/redis-data-types.md)
4. [Redis 的过期策略都有哪些？手写一下 LRU 代码实现？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/redis-expiration-policies-and-lru.md)
5. [如何保证 Redis 高并发、高可用？Redis 的主从复制原理能介绍一下么？Redis 的哨兵原理能介绍一下么？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/how-to-ensure-high-concurrency-and-high-availability-of-redis.md)
6. [Redis 主从架构是怎样的？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/redis-master-slave.md)
7. [Redis 的持久化有哪几种方式？不同的持久化机制都有什么优缺点？持久化机制具体底层是如何实现的？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/redis-persistence.md)
8. [Redis 集群模式的工作原理能说一下么？在集群模式下，Redis 的 key 是如何寻址的？分布式寻址都有哪些算法？了解一致性 hash 算法吗？如何动态增加和删除一个节点？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/redis-cluster.md)
9. [了解什么是 Redis 的雪崩、穿透和击穿？Redis 崩溃之后会怎么样？系统该如何应对这种情况？如何处理 Redis 的穿透？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/redis-caching-avalanche-and-caching-penetration.md)
10. [如何保证缓存与数据库的双写一致性？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/redis-consistence.md)
11. [Redis 的并发竞争问题是什么？如何解决这个问题？了解 Redis 事务的 CAS 方案吗？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/redis-cas.md)
12. [生产环境中的 Redis 是怎么部署的？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/redis-production-environment.md)
13. Redis常用数据结构
14. 如何用Redis实现分布式锁，可能遇到的问题和解决办法

### 架构

#### 基础

1.  Spring boot和spring的差别，tomcat如何嵌入spring boot的/spring boot中的tomcat是如何启动的；
2.  Spring如何解决循环依赖问题；
3. 简述 Spring 的初始化流程
4. dispatchServlet怎样分发任务的
5. 高并发情况下，我们系统是如何支撑大量的请求的
6. 集群如何同步会话状态
7. 什么情况会出现雪崩，以及如何应对？
8. AOP和IOC原理
9.  [OAuth2.0 是什么？有几种方式？](OAuth2.0 是什么？有几种方式？.md)
10.  SSO是什么？与OAuth2.0的关系

#### 高并发系统

- [如何设计一个高并发系统？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/high-concurrency-design.md)
- 1000个多并发线程，10台机器，每台机器4核的，设计线程池大小。
- 如何设计一个高并发的日志系统

#### 分布式框架

- [为什么要进行系统拆分？如何进行系统拆分？拆分后不用 Dubbo 可以吗？](https://github.com/doocs/advanced-java/blob/master/docs/distributed-system/why-dubbo.md)
- [说一下 Dubbo 的工作原理？注册中心挂了可以继续通信吗？](https://github.com/doocs/advanced-java/blob/master/docs/distributed-system/dubbo-operating-principle.md)
- [Dubbo 支持哪些序列化协议？说一下 Hessian 的数据结构？PB 知道吗？为什么 PB 的效率是最高的？](https://github.com/doocs/advanced-java/blob/master/docs/distributed-system/dubbo-serialization-protocol.md)
- [Dubbo 负载均衡策略和集群容错策略都有哪些？动态代理策略呢？](https://github.com/doocs/advanced-java/blob/master/docs/distributed-system/dubbo-load-balancing.md)
- [Dubbo 的 spi 思想是什么？](https://github.com/doocs/advanced-java/blob/master/docs/distributed-system/dubbo-spi.md)
- [如何基于 Dubbo 进行服务治理、服务降级、失败重试以及超时重试？](https://github.com/doocs/advanced-java/blob/master/docs/distributed-system/dubbo-service-management.md)
- [分布式服务接口的幂等性如何设计（比如不能重复扣款）？](https://github.com/doocs/advanced-java/blob/master/docs/distributed-system/distributed-system-idempotency.md)
- [分布式服务接口请求的顺序性如何保证？](https://github.com/doocs/advanced-java/blob/master/docs/distributed-system/distributed-system-request-sequence.md)
- [如何自己设计一个类似 Dubbo 的 RPC 框架？](https://github.com/doocs/advanced-java/blob/master/docs/distributed-system/dubbo-rpc-design.md)
- [CAP 定理的 P 是什么？](https://github.com/doocs/advanced-java/blob/master/docs/distributed-system/distributed-system-cap.md)

#### 分布式锁

- [Zookeeper 都有哪些应用场景？](https://github.com/doocs/advanced-java/blob/master/docs/distributed-system/zookeeper-application-scenarios.md)
- [使用 Redis 如何设计分布式锁？使用 Zookeeper 来设计分布式锁可以吗？以上两种分布式锁的实现方式哪种效率比较高？](https://github.com/doocs/advanced-java/blob/master/docs/distributed-system/distributed-lock-redis-vs-zookeeper.md)

#### 分布式事务

- [分布式事务了解吗？你们如何解决分布式事务问题的？TCC 如果出现网络连不通怎么办？XA 的一致性如何保证？](https://github.com/doocs/advanced-java/blob/master/docs/distributed-system/distributed-transaction.md)

#### 分布式会话

- [集群部署时的分布式 Session 如何实现？](https://github.com/doocs/advanced-java/blob/master/docs/distributed-system/distributed-session.md)

#### 高可用架构

- [Hystrix 介绍](https://github.com/doocs/advanced-java/blob/master/docs/high-availability/hystrix-introduction.md)
- [电商网站详情页系统架构](https://github.com/doocs/advanced-java/blob/master/docs/high-availability/e-commerce-website-detail-page-architecture.md)
- [Hystrix 线程池技术实现资源隔离](https://github.com/doocs/advanced-java/blob/master/docs/high-availability/hystrix-thread-pool-isolation.md)
- [Hystrix 信号量机制实现资源隔离](https://github.com/doocs/advanced-java/blob/master/docs/high-availability/hystrix-semphore-isolation.md)
- [Hystrix 隔离策略细粒度控制](https://github.com/doocs/advanced-java/blob/master/docs/high-availability/hystrix-execution-isolation.md)
- [深入 Hystrix 执行时内部原理](https://github.com/doocs/advanced-java/blob/master/docs/high-availability/hystrix-process.md)
- [基于 request cache 请求缓存技术优化批量商品数据查询接口](https://github.com/doocs/advanced-java/blob/master/docs/high-availability/hystrix-request-cache.md)
- [基于本地缓存的 fallback 降级机制](https://github.com/doocs/advanced-java/blob/master/docs/high-availability/hystrix-fallback.md)
- [深入 Hystrix 断路器执行原理](https://github.com/doocs/advanced-java/blob/master/docs/high-availability/hystrix-circuit-breaker.md)
- [深入 Hystrix 线程池隔离与接口限流](https://github.com/doocs/advanced-java/blob/master/docs/high-availability/hystrix-thread-pool-current-limiting.md)
- [基于 timeout 机制为服务接口调用超时提供安全保护](https://github.com/doocs/advanced-java/blob/master/docs/high-availability/hystrix-timeout.md)

#### 高可用系统

- 如何设计一个高可用系统？

#### 限流

- [如何限流？在工作中是怎么做的？说一下具体的实现？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/huifer-how-to-limit-current.md)

#### 熔断

- 如何进行熔断？
- 熔断框架都有哪些？具体实现原理知道吗？
- [熔断框架如何做技术选型？选用 Sentinel 还是 Hystrix？](https://github.com/doocs/advanced-java/blob/master/docs/high-availability/sentinel-vs-hystrix.md)

#### 降级

- 如何进行降级？

#### 微服务架构

- [微服务架构整个章节内容属额外新增，后续抽空更新，也欢迎读者们参与补充完善](https://github.com/doocs/advanced-java)
- [关于微服务架构的描述](https://github.com/doocs/advanced-java/blob/master/docs/micro-services/microservices-introduction.md)
- [从单体式架构迁移到微服务架构](https://github.com/doocs/advanced-java/blob/master/docs/micro-services/migrating-from-a-monolithic-architecture-to-a-microservices-architecture.md)
- [微服务的事件驱动数据管理](https://github.com/doocs/advanced-java/blob/master/docs/micro-services/event-driven-data-management-for-microservices.md)
- [选择微服务部署策略](https://github.com/doocs/advanced-java/blob/master/docs/micro-services/choose-microservice-deployment-strategy.md)
- [微服务架构的优势与不足](https://github.com/doocs/advanced-java/blob/master/docs/micro-services/advantages-and-disadvantages-of-microservice.md)

#### Spring Cloud 微服务架构

- [什么是微服务？微服务之间是如何独立通讯的？](https://github.com/doocs/advanced-java/blob/master/docs/micro-services/huifer-what's-microservice-how-to-communicate.md)
- Spring Cloud 和 Dubbo 有哪些区别？
- Spring Boot 和 Spring Cloud，谈谈你对它们的理解？
- 什么是服务熔断？什么是服务降级？
- 微服务的优缺点分别是什么？说一下你在项目开发中碰到的坑？
- [你所知道的微服务技术栈都有哪些？](https://github.com/doocs/advanced-java/blob/master/docs/micro-services/huifer-micro-services-technology-stack.md)
- [微服务治理策略](https://github.com/doocs/advanced-java/blob/master/docs/micro-services/huifer-micro-service-governance.md)
- Eureka 和 Zookeeper 都可以提供服务注册与发现的功能，它们有什么区别？
- [谈谈服务发现组件 Eureka 的主要调用过程？](https://github.com/doocs/advanced-java/blob/master/docs/micro-services/how-eureka-enable-service-discovery-and-service-registration.md)

### 大数据

#### 大数据处理

- [如何从大量的 URL 中找出相同的 URL？](https://github.com/doocs/advanced-java/blob/master/docs/big-data/find-common-urls.md)
- [如何从大量数据中找出高频词？](https://github.com/doocs/advanced-java/blob/master/docs/big-data/find-top-100-words.md)
- [如何找出某一天访问百度网站最多的 IP？](https://github.com/doocs/advanced-java/blob/master/docs/big-data/find-top-1-ip.md)
- [如何在大量的数据中找出不重复的整数？](https://github.com/doocs/advanced-java/blob/master/docs/big-data/find-no-repeat-number.md)
- [如何在大量的数据中判断一个数是否存在？](https://github.com/doocs/advanced-java/blob/master/docs/big-data/find-a-number-if-exists.md)
- [如何查询最热门的查询串？](https://github.com/doocs/advanced-java/blob/master/docs/big-data/find-hotest-query-string.md)
- [如何统计不同电话号码的个数？](https://github.com/doocs/advanced-java/blob/master/docs/big-data/count-different-phone-numbers.md)
- [如何从 5 亿个数中找出中位数？](https://github.com/doocs/advanced-java/blob/master/docs/big-data/find-mid-value-in-500-millions.md)
- [如何按照 query 的频度排序？](https://github.com/doocs/advanced-java/blob/master/docs/big-data/sort-the-query-strings-by-counts.md)
- [如何找出排名前 500 的数？](https://github.com/doocs/advanced-java/blob/master/docs/big-data/find-rank-top-500-numbers.md)

### 数据库

#### 数据库基础

- MySQL是如何优化的，数据量有多少
- MySQL索引如何实现，为什么用B+树不用B树二叉树；
- B树和B+树是解决什么样的问题的，怎样演化过来，之间区别
- 聚簇索引和非聚簇索引的区别；
- [MySQL行锁、表锁、悲观锁、乐观锁的特点与应用](http://youzhixueyuan.com/mysql-row-locks-table-locks-pessimistic-locks.html)
- mysql给离散度低的字段建立索引会出现什么问题，具体说下原因
- 什么情况下会发生死锁，如何解决死锁？

#### 分库分表

- [为什么要分库分表（设计高并发系统的时候，数据库层面该如何设计）？用过哪些分库分表中间件？不同的分库分表中间件都有什么优点和缺点？你们具体是如何对数据库如何进行垂直拆分或水平拆分的？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/database-shard.md)
- [现在有一个未分库分表的系统，未来要分库分表，如何设计才可以让系统从未分库分表动态切换到分库分表上？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/database-shard-method.md)
- [如何设计可以动态扩容缩容的分库分表方案？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/database-shard-dynamic-expand.md)
- [分库分表之后，id 主键如何处理？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/database-shard-global-id-generate.md)

#### 读写分离

- [如何实现 MySQL 的读写分离？MySQL 主从复制原理是啥？如何解决 MySQL 主从同步的延时问题？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/mysql-read-write-separation.md)



