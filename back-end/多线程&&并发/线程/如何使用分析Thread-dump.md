# 获取thread dump
1. ps –ef  | grep java
2. jstack -l <pid> > 111.txt 导出线程信息
3. jstack pid | tee -a jstack.log 导出堆栈信息

# 线程原始状态
###### NEW：还没有调用start()方法开始执行，在thread dump中不会出现
###### RUNNABLE：正常在执行的线程
###### BLOCKED：阻塞状态，等待获取锁进入同步方法或者同步代码块
###### WAITING：调用wait()，join()，LockSupport.park()方法
###### TIMED_WAITING：调用wait(long)，join(long)，LockSupport.parkNanos，LockSupport.parkUntil方法，在指定时间内等待并且没有超时
###### TERMINATED：正常执行完的线程，在thread dump中不会出现
###### 在thread dump中可能出现的thread线程的原始状态有：RUNNABLE，BLOCKED，WAITING，TIMED_WAITING

# thread dump中的线程状态
- Runnable
- sleeping
- Waiting for monitor entry （重点关注）
- Wait on condition（重点关注）
- in Object.wait() （重点关注）
- Deadlock（重点关注）

###### thread的RUNNABLE对应---> Runnable
###### thread的BLOCKED对应---> Waiting for monitor entry 
###### thread的WAITING和TIMED_WAITING对应---> in Object.wait()和Wait on condition



thread dump线程状态详细信息 | 解释 | 对应的方法调用
---|---|---
java.lang.Thread.State: RUNNABLE | 线程运行中或I/O等待| 无
java.lang.Thread.State: BLOCKED (on object monitor)  |	等待进入一个临界区 |	synchronized
java.lang.Thread.State: TIMED_WAITING (parking) |	线程等待唤醒，并且设置等待时长 |	LockSupport.parkNanos(等待时长)、LockSupport.parkUntil(等待时长)
java.lang.Thread.State: TIMED_WAITING (sleeping) |	线程等待唤醒，并且设置等待时长 |	Thread.sleep(等待时长)，Thread.join(等待时长)
java.lang.Thread.State: TIMED_WAITING (on object monitor) | 线程在等待唤醒，并且设置等待时长 | Object.wait(等待时长)
java.lang.Thread.State: WAITING (parking) |	线程等待唤醒，没有设置等待时长 |	LockSupport.park()
java.lang.Thread.State: WAITING (on object monitor)  |	线程等待唤醒，并且没有设置等待时长 |	Object.wait()
ava.lang.Thread.State: WAITING (on object monitor)  |	线程等待唤醒，没有设置等待时长 |	Thread.join()

# dump文件含义
结合实际文件信息说明

