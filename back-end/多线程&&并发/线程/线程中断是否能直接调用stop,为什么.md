# 线程中断是否能直接调用stop,为什么



## 1、为什么废弃Thread的stop函数？

- **stop是通过立即抛出ThreadDeath异常，来达到停止线程的目的，此异常抛出有可能发生在任何一时间点，包括在catch、finally等语句块中，但是此异常并不会引起程序退出(笔者只测试了Java8)。**
- **由于有异常抛出，导致线程会释放全部所持有的锁，极可能引起线程安全问题。**



下面是stop的源码(Java8)：



```java
    @Deprecated
    public final void stop() {
        SecurityManager security = System.getSecurityManager();
        if (security != null) {
            checkAccess();
            if (this != Thread.currentThread()) {
                security.checkPermission(SecurityConstants.STOP_THREAD_PERMISSION);
            }
        }
        // A zero status value corresponds to "NEW", it can't change to
        // not-NEW because we hold the lock.
        if (threadStatus != 0) {
            resume(); // Wake up thread if it was suspended; no-op otherwise
        }

        // The VM can handle all thread states
        stop0(new ThreadDeath());
    }
    
    private native void stop0(Object o);
```

上述源码中关键代码就是stop0(new ThreadDeath())函数，这是Native函数，传递的参数是ThreadDeath，ThreadDeath是一个异常对象，该对象从Native层抛到了Java层，从而导致线程停止，不过此异常并不会引起程序退出。





**再查一下**