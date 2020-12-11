# ThreadLocal实现原理是什么

**ThreadLocal有什么作用：**

值 和 线程绑定，在其他线程使用无法访问和修改。

## 使用：

```java
public class InheritableThreadLocalDemo {
    private static ThreadLocal<String> threadLocal = new ThreadLocal<>();
    // InheritableThreadLocal可以被子线程获取到
    // private static InheritableThreadLocal<String> threadLocal = new InheritableThreadLocal<>();
    
    public static void main(String[] args) {
        threadLocal.set("parentThread");
        System.out.println("value:"+threadLocal.get());
        Thread thread = new Thread(new Runnable() {
            @Override
            public void run() {
                // 子线程无法获取
                String value = threadLocal.get();
                System.out.println("value:"+value);
            }
        });
        thread.start();
    }
}

// 结果
value:parentThread
value:null
```

## 实现

通过Thread.currentThread() 获取当前线程，然后从当前线程中的ThreadLocalMap中取值

```java
public T get() {
    Thread t = Thread.currentThread();
    ThreadLocalMap map = getMap(t);
    if (map != null) {
        ThreadLocalMap.Entry e = map.getEntry(this);
        if (e != null) {
            T result = (T)e.value;
            return result;
        }
    }
    return setInitialValue();
}
    
ThreadLocalMap getMap(Thread t) {  return t.threadLocals;  }

private T setInitialValue() {
    T value = initialValue();
    Thread t = Thread.currentThread();
    ThreadLocalMap map = getMap(t);
    if (map != null)
        map.set(this, value);
    else
        createMap(t, value);
    return value;
}
```

## Thread中怎么存储的

两个变量：

- `threadLocals` 当前线程可用

- `inheritableThreadLocals` 用于需要传递给子线程的情况

  在线程初始化时，会将父线程inheritableThreadLocals中的变量保存到自己的threadLocals中。

  如果希望变量能被子线程使用，需使用 `InheritableThreadLocal` 类代替 `ThreadLocal`

```java
public class Thread implements Runnable {
    ThreadLocal.ThreadLocalMap threadLocals = null;
    ThreadLocal.ThreadLocalMap inheritableThreadLocals = null;

    private void init(...) {
        Thread parent = currentThread();
        if (inheritThreadLocals && parent.inheritableThreadLocals != null) {
            this.inheritableThreadLocals =
                ThreadLocal.createInheritedMap(parent.inheritableThreadLocals);
        }
    }
}
```

```java
public class ThreadLocal<T> {
    public ThreadLocal() {}
	// 使用SuppliedThreadLocal可提供默认值
	protected T initialValue() {  return null;  }
	// 为线程创建Map
    void createMap(Thread t, T firstValue) {
        t.threadLocals = new ThreadLocalMap(this, firstValue);
    }
	// 获取到父线程的inheritableThreadLocals，创建thradLocalMap
    static ThreadLocalMap createInheritedMap(ThreadLocalMap parentMap) {
        return new ThreadLocalMap(parentMap);
    }
	// InheritableThreadLocal 中使用
    T childValue(T parentValue) {  throw new UnsupportedOperationException();  }
}
```

## InheritableThreadLocal

重写了`childValue()`方法。

在子线程创建ThreadLocalMap的时候，调用父线程的 inheritableThreadLocals的childValue()方法，提供变量的值。

```java
public class InheritableThreadLocal<T> extends ThreadLocal<T> {
    protected T childValue(T parentValue) {
        return parentValue;
    }

    ThreadLocalMap getMap(Thread t) {
       return t.inheritableThreadLocals;
    }
	// 为线程的 inheritableThreadLocals初始化。ThreadLocal类中是threadLocals变量
    void createMap(Thread t, T firstValue) {
        t.inheritableThreadLocals = new ThreadLocalMap(this, firstValue);
    }
}

// ThreadLocal的构造方法，
// 在子线程创建时，如果发现父线程inheritableThreadLocals有值，会调用该方法创建ThreadlLocalMap
private ThreadLocalMap(ThreadLocalMap parentMap) {
    // 	省略...
    for (int j = 0; j < len; j++) {
        Entry e = parentTable[j];
        if (e != null) {
            ThreadLocal<Object> key = (ThreadLocal<Object>) e.get();
            if (key != null) {
                // 获取 parentValue
                Object value = key.childValue(e.value);
                Entry c = new Entry(key, value);
                int h = key.threadLocalHashCode & (len - 1);
                while (table[h] != null)
                    h = nextIndex(h, len);
                table[h] = c;
                size++;
            }
        }
    }
}
```

## hashCode计算

```java
private final int threadLocalHashCode = nextHashCode();
private static AtomicInteger nextHashCode = new AtomicInteger();
// 神奇的数字
private static final int HASH_INCREMENT = 0x61c88647;
private static int nextHashCode() {
    return nextHashCode.getAndAdd(HASH_INCREMENT);
}

// 在map中的使用
int index = key.threadLocalHashCode & (table.length - 1);
```

[从 ThreadLocal 的实现看散列算法](https://zhuanlan.zhihu.com/p/40515974)

## ThreadLocalMap的实现

Entry使用WeakReference，防止内存泄漏

```java
static class ThreadLocalMap {
	// WeakReference。key为ThreadLocal
    static class Entry extends WeakReference<ThreadLocal<?>> {
        /** The value associated with this ThreadLocal. */
        Object value;

        Entry(ThreadLocal<?> k, Object v) {
            super(k);
            value = v;
        }
    }
    private void set(ThreadLocal<?> key, Object value) {
        Entry[] tab = table;
        int len = tab.length;
        int i = key.threadLocalHashCode & (len-1);
        tab[i] = new Entry(key, value);
    }
}
```

> [关于内存泄露，虽然 key 是弱引用，但是 value 不一定能被回收吧？因为它和 Current Thread 之间还存在一个强引用](https://droidyue.com/blog/2016/03/13/learning-threadlocal-in-java/)

其实这个思考是会存在的，但是这并不会导致服务器内存大面积泄露，只是仅仅来说有一个跟随ThreadLocalMap的一个值而已，其内存并不会一直增加，你考虑到了key的弱引用被回收了，那么就会出现更多的key被回收，而他们的value会覆盖，并不会存在多个key为null的情况，也就不会导致value占用的内存进一步增加，换句话说，当下一个key为null的情况出现，就是value被回收的时间，这种情况并不会导致传统意义上的(广义上的)内存泄漏，换句话说这个内存在可控范围



## 对象存放在哪里

在Java中，栈内存归属于单个线程，每个线程都会有一个栈内存，其存储的变量只能在其所属线程中可见，即栈内存可以理解成线程的私有内存。而堆内存中的对象对所有线程可见。堆内存中的对象可以被所有线程访问。

### 问：那么是不是说ThreadLocal的实例以及其值存放在栈上呢？

其实不是，因为ThreadLocal实例实际上也是被其创建的类持有（更顶端应该是被线程持有）。而ThreadLocal的值其实也是被线程实例持有。

**它们都是位于堆上，只是通过一些技巧将可见性修改成了线程可见。**

## 在Android中的应用

### Handler机制

用于**切换线程**。

Looper在**A**线程中创建，并且死循环从队列中取值，当取到值时就执行对应的回调。

我们可以在**B**线程通过往队列中插入值，来触发Looper调用。

因为Looper是在**A**线程中的，所以回调也是运行在和Looper同一个线程中。

#### Handler使用

```kotlin
var handler = object :Handler(Looper.myLooper()) {
    override fun handleMessage(msg: Message?) {
        if(msg.what == 1) {  do.....  }
    }
}
// 从缓存池中获取一个消息对象
var message = handler.obtainMessage()
message.what = 1
message.obj = xx;
handler.sendMessage(message)
```

![image-20200818101407775.png](https://gitee.com/liuw5367/resource/raw/master/img/image-20200818101407775.png)

#### 主线程创建**mainLooper**

```java
// ActivityThread.java，在Android程序启动时调用
public static void main(String[] args) {
	......
    // 主线程Looper创建
    Looper.prepareMainLooper();
	......
    Looper.loop();
}
```

#### Looper创建

```java
static final ThreadLocal<Looper> sThreadLocal = new ThreadLocal<Looper>();

public static void prepareMainLooper() {
  prepare(false);
  synchronized (Looper.class) {
     if (sMainLooper != null) {
        throw new IllegalStateException("The main Looper has already been prepared.");
     }
     sMainLooper = myLooper();
   }
}

private static void prepare(boolean quitAllowed) {
  // ThreadLocal 线程唯一
  if (sThreadLocal.get() != null) {
    throw new RuntimeException("Only one Looper may be created per thread");
  }
  // 创建
  sThreadLocal.set(new Looper(quitAllowed));
}

public static @Nullable Looper myLooper() {
  return sThreadLocal.get();
}
```

#### Looper.loop

```java
/**
 * Run the message queue in this thread. Be sure to call
 * {@link #quit()} to end the loop.
 */
public static void loop() {
    final Looper me = myLooper();
    if (me == null) {
        throw new RuntimeException("No Looper; Looper.prepare() wasn't called on this thread.");
    }
    final MessageQueue queue = me.mQueue;

    for (;;) {
        Message msg = queue.next(); // might block
        if (msg == null) {
            return;
        }
        try {
            msg.target.dispatchMessage(msg);
        } finally {
        }
        msg.recycleUnchecked();
    }
}

Message next() {
    int nextPollTimeoutMillis = 0;
    for (;;) {
		// native方法，等待直到下一条消息可用。 Linux I/O多路复用机制
        nativePollOnce(ptr, nextPollTimeoutMillis);
    }
}
```