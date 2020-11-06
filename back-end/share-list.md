## 后端技术栈分享内容参考列表



[TOC]



### 基础知识

#### Java基础

1. HashMap 与 ConcurrentHashMap 的实现原理是怎样的？ConcurrentHashMap 是如何保证线程安全的？
2. Java 中垃圾回收机制中如何判断对象需要回收？常见的 GC 回收算法有哪些？
3.  synchronized 关键字底层是如何实现的？它与 Lock 相比优缺点分别是什么？
4.  hashmap 和 hashtable 的区别是什么？
5. HashMap 实现原理，为什么使用红黑树？
6. 简述 Java的反射机制
7. Java 线程间有多少通信方式？
8. 简述 Synchronized，volatile，可重入锁的不同使用场景及优缺点
9. Java 类的加载流程是怎样的？什么是双亲委派机制？
10. 简述常见的工厂模式以及单例模式的使用场景
11. JVM 中内存模型是怎样的，简述新生代与老年代的区别？
12. Java 常见锁有哪些？ReetrantLock 是怎么实现的？
13. ThreadLocal 实现原理是什么？
14. 简述 Spring 的初始化流程
15. 简述生产者消费者模型
16. Java 如何高效进行数组拷贝
17. CAS 实现原理是什么？
18. 成员变量和方法的区别？
19. JVM 是怎么去调优的？简述过程和调优的结果
20. Java 缓冲流 buffer 的用途和原理是什么？
21. 简述 BIO, NIO, AIO 的区别
22.  简述 Java 的 happen before 原则



#### 算法

- ▲ 29 10亿个数中如何高效地找到最大的一个数以及最大的第 K 个数
- ▲ 21 最大子序和 (Leetcode)
- ▲ 20 爬楼梯 (Leetcode)
- ▲ 18 用 Rand7() 实现 Rand10() (Leetcode)
- ▲ 15 AVL 树和红黑树有什么区别？
- ▲ 15 给定一个包含 40亿 个无符号整数的大型文件，使用最多 1G 内存，对此文件进行排序
- ▲ 14 合并两个有序链表 (Leetcode)
- ▲ 14 路径总和 (Leetcode)
- ▲ 14 如果通过一个不均匀的硬币得到公平的结果？
- ▲ 13 实现快速排序
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



#### 网络协议

- 简述 TCP 三次握手以及四次挥手的流程。为什么需要三次握手以及四次挥手？
-  RestFul 与 RPC 的区别是什么？RestFul 的优点在哪里？
-  HTTP 与 HTTPS 有哪些区别？
-  RestFul 是什么？RestFul 请求的 URL 有什么特点？
-  一次 HTTP 的请求过程中发生了什么？
-  TCP 与 UDP 在网络协议中的哪一层，他们之间有什么区别？
-  TCP 中常见的拥塞控制算法有哪些？
-  TCP 怎么保证可靠传输？
-  从系统层面上，UDP如何保证尽量可靠？
- TCP 的 keepalive 了解吗？说一说它和 http 的 keepalive 的区别？
-  简述 TCP 滑动窗口以及重传机制
-  简述 HTTP 1.0，1.1，2.0 的主要区别
-  简述 TCP 的 TIME_WAIT
- HTTP 的方法有哪些？
-  简述 TCP 协议的延迟 ACK 和累计应答
-  简述 TCP 的报文头部结构
-  简述 TCP 半连接发生场景
-  什么是 SYN flood，如何防止这类攻击？ 



### 高并发相关

#### MQ

1. [为什么使用消息队列？消息队列有什么优点和缺点？Kafka、ActiveMQ、RabbitMQ、RocketMQ 都有什么优点和缺点？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/why-mq.md)
2. [如何保证消息队列的高可用？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/how-to-ensure-high-availability-of-message-queues.md)
3. [如何保证消息不被重复消费？（如何保证消息消费的幂等性）](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/how-to-ensure-that-messages-are-not-repeatedly-consumed.md)
4. [如何保证消息的可靠性传输？（如何处理消息丢失的问题）](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/how-to-ensure-the-reliable-transmission-of-messages.md)
5. [如何保证消息的顺序性？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/how-to-ensure-the-order-of-messages.md)
6. [如何解决消息队列的延时以及过期失效问题？消息队列满了以后该怎么处理？有几百万消息持续积压几小时，说说怎么解决？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/mq-time-delay-and-expired-failure.md)
7. [如果让你写一个消息队列，该如何进行架构设计啊？说一下你的思路。](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/mq-design.md)

#### 搜索引擎

1. [ES 的分布式架构原理能说一下么（ES 是如何实现分布式的啊）？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/es-architecture.md)
2. [ES 写入数据的工作原理是什么啊？ES 查询数据的工作原理是什么啊？底层的 Lucene 介绍一下呗？倒排索引了解吗？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/es-write-query-search.md)
3. [ES 在数据量很大的情况下（数十亿级别）如何提高查询效率啊？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/es-optimizing-query-performance.md)
4. [ES 生产集群的部署架构是什么？每个索引的数据量大概有多少？每个索引大概有多少个分片？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/es-production-cluster.md)

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

#### 分库分表

- [为什么要分库分表（设计高并发系统的时候，数据库层面该如何设计）？用过哪些分库分表中间件？不同的分库分表中间件都有什么优点和缺点？你们具体是如何对数据库如何进行垂直拆分或水平拆分的？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/database-shard.md)
- [现在有一个未分库分表的系统，未来要分库分表，如何设计才可以让系统从未分库分表动态切换到分库分表上？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/database-shard-method.md)
- [如何设计可以动态扩容缩容的分库分表方案？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/database-shard-dynamic-expand.md)
- [分库分表之后，id 主键如何处理？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/database-shard-global-id-generate.md)

#### 读写分离

- [如何实现 MySQL 的读写分离？MySQL 主从复制原理是啥？如何解决 MySQL 主从同步的延时问题？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/mysql-read-write-separation.md)

#### 高并发系统

- [如何设计一个高并发系统？](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/high-concurrency-design.md)

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

#### 数据处理

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



