## 为什么String在Java中是不可变的

![下载](https://gitee.com/liuw5367/resource/raw/master/img/下载.jpg)

```java
// Java 1.8
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence{
    /** The value is used for character storage. */
    private final char value[];

    /** Cache the hash code for the string */
    private int hash; // Default to 0
    
    public String(String original) {
        this.value = original.value;
        this.hash = original.hash;
    }
    // 第一次调用的时候，会将hash缓存起来
    public int hashCode() {
        int h = hash;
        if (h == 0 && value.length > 0) {
            char val[] = value;

            for (int i = 0; i < value.length; i++) {
                h = 31 * h + val[i];
            }
            hash = h;
        }
        return h;
    }
}
```

如果String是可变的会产生的一些问题：

1. 字符串已**被多个变量同时引用**，如果其中任何一个更改了值，**其他变量将受到影响**

   ```java
   String A = "Test";
   String B = "Test";
   
   现在字符串 B 调用 "Test".toUpperCase(), 将同一个对象改为“TEST”，
   所以 A 也是 “TEST”，这不是期望的结果。
   ```

2. **字符串已被广泛用作许多 Java 类的参数**，如果String修改为可变，这些Java类可能会受到影响。

   例如，为了打开网络连接，你可以将主机名和端口号作为字符串传递，你可以将数据库 URL 作为字符串传递, 以打开数据库连接，你可以通过将文件名作为参数传递给 File I/O 类来打开 Java 中的任何文件.

   如果 String 不是不可变的，这将导致严重的安全威胁，我的意思是有人可以访问他有权授权的任何文件，然后可以故意或意外地更改文件名并获得对该文件的访问权限。

   由于不变性，你无需担心这种威胁。这个原因也说明了，为什么 String 在 Java 中是最终的，通过使 `java.lang.String` final，Java设计者确保没有人覆盖 String 类的任何行为。

3. String 不可变的绝对最重要的原因是它被类加载机制使用，因此具有深刻和基本的安全考虑。如果 String 是可变的，加载“java.io.Writer” 的请求可能已被更改为加载 “mil.vogoon.DiskErasingWriter”. 安全性和字符串池是使字符串不可变的主要原因。

4. 由于 String 是不可变的，它可以**安全地在多线程中共享**，这对于多线程编程非常重要. 并且避免了 Java 中的同步问题，不变性也使得String 实例在 Java 中是线程安全的，这意味着你不需要从外部同步 String 操作。

5. **String 会缓存其哈希码**，不会在每次调用 String 的 hashcode 方法时重新计算，这使得它在 Java 中的 HashMap 中使用的 HashMap 键非常快。

    ```java
    // HashMap.java
    public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }
    
    public V get(Object key) {
        Node<K,V> e;
        return (e = getNode(hash(key), key)) == null ? null : e.value;
    }
    
    static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
    ```