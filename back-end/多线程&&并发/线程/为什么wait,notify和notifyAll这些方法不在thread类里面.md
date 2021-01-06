## 为什么wait, notify 和 notifyAll这些方法不在thread类里面？



JAVA 提供的锁是对象级的而不是线程级的，每个对象都有

锁，通过线程获得。由于 wait，notify 和 notifyAll 都是锁级别的操作，所以把他

们定义在 Object 类中因为锁属于对象。