# String：安全性，不可变，内存

## 为什么 char 数组比 Java 中的 String 更适合存储密码

**安全问题**

因为字符串是不可变对象，如果作为普通文本存储密码，那么它会一直存在内存中（堆）直至被垃圾收集器回收。任何可以访问内存的人都能以明码的方式把密码dump出来。

应该始终使用加密密码而不是纯文本.

由于字符串是不可变的，任何更改都会产生新的字符串，而如果使用`char[]`，可以将所有元素设置为空白或零。

```java
String strPassword =“Unknown”;
char [] charPassword = new char [] {'U'，'n'，'k'，'w'，'o'，'n'};
System.out.println(“字符串密码：”+ strPassword);
System.out.println(“字符密码：”+ charPassword);

==========

字符串密码：Unknown
字符密码：[C @110b053
```

[为什么存储密码字符数组比字符串更合适](https://blog.csdn.net/x_iya/article/details/80086995)



## 为什么 String 在 Java 中是不可变的

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

1. 已被多个变量引用，因此如果其中任何一个更改了值，其他参数将自动受到影响

   ```java
   String A = "Test";
   String B = "Test";
   
   现在字符串 B 调用 "Test".toUpperCase(), 将同一个对象改为“TEST”，
   所以 A 也是 “TEST”，这不是期望的结果。
   ```

2. 字符串已被广泛用作许多 Java 类的参数，例如，为了打开网络连接，你可以将主机名和端口号作为字符串传递，你可以将数据库 URL 作为字符串传递, 以打开数据库连接，你可以通过将文件名作为参数传递给 File I/O 类来打开 Java 中的任何文件.

   如果 String 不是不可变的，这将导致严重的安全威胁，我的意思是有人可以访问他有权授权的任何文件，然后可以故意或意外地更改文件名并获得对该文件的访问权限。

3. String 不可变的绝对最重要的原因是它被类加载机制使用，因此具有深刻和基本的安全考虑。如果 String 是可变的，加载“java.io.Writer” 的请求可能已被更改为加载 “mil.vogoon.DiskErasingWriter”. 安全性和字符串池是使字符串不可变的主要原因。

4. 由于 String 是不可变的，它可以安全地共享许多线程，这对于多线程编程非常重要. 并且避免了 Java 中的同步问题，不变性也使得String 实例在 Java 中是线程安全的，这意味着你不需要从外部同步 String 操作。

5. 为什么 String 在 Java 中是不可变的另一个原因是允许 String 缓存其哈希码，Java 中的不可变 String 缓存其哈希码，并且不会在每次调用 String 的 hashcode 方法时重新计算，这使得它在 Java 中的 HashMap 中使用的 HashMap 键非常快。

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

[为什么 String 是不可变的？](https://mp.weixin.qq.com/s?__biz=MzI3ODcxMzQzMw==&mid=2247493546&idx=2&sn=b53d3f2c8edeb81e8e79f4123e051b55&chksm=eb50629cdc27eb8ae52edd72e0b6bda617d8670b601e154957c0dff0044381fe2ea40ff971c9&scene=21#wechat_redirect)

## String.intern()

### 字符串对象在JVM中是如何存放的?

- 字符串对象在JVM中可能有两个存放的位置：(1) 字符串常量池 (2) 堆内存。
- 使用常量字符串初始化的字符串对象，它的值存放在 **字符串常量池** 中。
- 使用字符串构造方法创建的字符串对象，它的值存放在**堆内存**中。new String()
- **`intern()`** API 可以手动将一个字符串对象的值转移到字符串常量池中。
- 历史
  - JDK1.7之前，字符串常量池是在 **PermGen（方法区）** 区域，这个区域的大小是固定的——不能在运行时根据需要扩大，也不能被垃圾收集器回收。因此如果程序中有太多的字符串调用了intern方法的话，就可能造成OOM。
  - JDK1.7以后，**字符串常量池移到了堆内存中**，并且可以被垃圾收集器回收，这个改动降低了字符串常量池OOM的风险。

```java
static class Json {   public String value;    }

public static void main(String[] args) throws Exception {
  Gson gson = new Gson(); // {"value":"value"}
  Reader reader = new FileReader(new File("json"));
  Reader reader2 = new FileReader(new File("json"));
  Json json = gson.fromJson(reader, Json.class);
  Json json22 = gson.fromJson(reader2, Json.class);

  String v1 = "value";  // 常量池
  String v2 = new String("value");  // 堆中

  System.out.println(v1 == v2); // false
  System.out.println(v1 == json.value); // false
  System.out.println(v2 == json.value); // false
  System.out.println(json22.value == json.value); // false

  json.value.intern();
  System.out.println(v1 == json.value); // false
  System.out.println(v1 == json.value.intern()); // true
}
```

[90%的开发者都没搞清楚的字符串常量池](https://www.cnblogs.com/fengzheng/p/12782844.html)



[第5讲 | String、StringBuffer、StringBuilder有什么区别？](https://time.geekbang.org/column/article/7349)

[03 | 字符串性能优化不容小觑，百M内存轻松存储几十G数据](https://time.geekbang.org/column/article/97215)

