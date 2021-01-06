## 你对String对象的intern熟悉么

- 字符串对象在JVM中可能有两个存放的位置：
    1. 字符串常量池  `String value = "aaaaa"`  , 使用**常量字符串初始化**的字符串对象
    2. 堆内存 `String value = new String()` , 使用**字符串构造方法创建**的字符串对象
- **`intern()`** 方法会从字符串常量池中查询当前字符串是否存在，若不存在就会将当前字符串放入常量池中
- 历史
    - JDK1.7之前，字符串常量池是在 **PermGen（永久代）** 区域，这个区域的大小是固定的——不能在运行时根据需要扩大，也不能被垃圾收集器回收。因此如果程序中有太多的字符串调用了intern方法的话，就可能造成OOM。
    - JDK1.7开始，**字符串常量池移到了堆内存中**，并且可以被垃圾收集器回收，这个改动降低了字符串常量池OOM的风险。

### Native实现

```java
/** 
 * @return  a string that has the same contents as this string, but is 
 *          guaranteed to be from a pool of unique strings. 
 */  
public native String intern();  
```

openJdk7 源码 intern 的实现：

**C++中实现了StringTable存储字符串**

```c++
// 路径：\openjdk7\jdk\src\share\native\java\lang\String.c
Java_java_lang_String_intern(JNIEnv *env, jobject this)  {  
    return JVM_InternString(env, this);  
}  

// 路径：\openjdk7\hotspot\src\share\vm\prims\jvm.h
/* * java.lang.String */  
JNIEXPORT jstring JNICALL  
JVM_InternString(JNIEnv *env, jstring str);   

// 路径：\openjdk7\hotspot\src\share\vm\prims\jvm.cpp
// String support ///////////////////////////////////////////////////////////////////////////  
JVM_ENTRY(jstring, JVM_InternString(JNIEnv *env, jstring str))  
  JVMWrapper("JVM_InternString");  
  JvmtiVMObjectAllocEventCollector oam;  if (str == NULL) return NULL;  
  oop string = JNIHandles::resolve_non_null(str);  
  oop result = StringTable::intern(string, CHECK_NULL);
  return (jstring) JNIHandles::make_local(env, result);  
JVM_END   
    
// 路径：\openjdk7\hotspot\src\share\vm\classfile\symbolTable.cpp
oop StringTable::intern(Handle string_or_null, jchar* name, int len, TRAPS) {  
  unsigned int hashValue = java_lang_String::hash_string(name, len);  
  int index = the_table()->hash_to_index(hashValue);  
  // 计算hash、index从表中查找string
  oop string = the_table()->lookup(index, name, len, hashValue);  
  // 找到值，返回
  if (string != NULL) return string;  
  // 未找到，添加到表中
  return the_table()->basic_add(index, string_or_null, name, len, hashValue, CHECK_NULL); 
}
```

大体实现结是: JAVA 使用 jni 调用c++实现的`StringTable`的`intern()`方法, `StringTable`的`intern`方法跟Java中的`HashMap`的实现是差不多的,  只是不能自动扩容。

JDK6中String的String Pool是一个固定大小的`Hashtable`，默认值大小长度是1009，如果数据太多会造成Hash冲突严重，导致链表很长，而链表长了后的影响就是： 调用`String.intern`时的性能大幅下降（因为要一个一个找）。

在Jdk7中，`StringTable`的长度可以通过一个参数指定：

- `-XX:StringTableSize=99991`

`7u40 `以后默认缓存大小修改为 60013 ,下面的参数打印具体数字: `-XX:+PrintStringTableStatistics`

Intern 是一种**显式地排重机制**，它有一定的副作用，因为需要开发者写代码时明确调用，一是不方便，每一个都显式调用是非常麻烦的；另外就是我们很难保证效率，应用开发阶段很难清楚地预计字符串的重复情况，有人认为这是一种污染代码的实践。

在 Oracle JDK 8u20 之后，推出了一个新的特性，也就是 G1 GC 下的字符串排重。它是通过将相同数据的字符串指向同一份数据来做到的，是 JVM 底层的改变，并不需要 Java 类库做什么修改。

注意这个功能目前是默认关闭的，你需要使用下面参数开启，并且记得指定使用 G1 GC：

- `-XX:+UseStringDeduplication`

### 例子分析

```java
public static void main(String[] args) {
    // 第一种
    String s = new String("1");
    s.intern();
    String s2 = "1";
    System.out.println(s == s2);
    // 第二种
 	String s = new String("1");
    String s2 = "1";
    s.intern();
    System.out.println(s == s2);
}
// jdk6下 false
// jdk7下 false
```

因为在s创建的时候，先在常量池中放入了"1"字符串，然后才new String()有了S。所以s2的值是从常量池中取出来的， s.intern()顺序没有影响。

#### 堆内存和永久区

```java
public static void main(String[] args) {
    // 第一种
    String s3 = new String("1") + new String("1");
    s3.intern();
    String s4 = "11";
    System.out.println(s3 == s4);
    // jdk6下 false
	// jdk7下 true
    
    // 第二种
    String s3 = new String("1") + new String("1"); // 堆内存中
    String s4 = "11"; // 将值放入常量池
    s3.intern(); // 常量池中已经存在值了
    System.out.println(s3 == s4);
    // jdk6下 false
	// jdk7下 false
}
```

- 在第一种情况中为什么JDK6是false，而在7中是true呢

##### JDK6

注：图中绿色线条代表 string 对象的内容指向。 黑色线条代表地址指向。

![image-20201127111449986](https://gitee.com/liuw5367/resource/raw/master/img/image-20201127111449986.png)

因为 jdk6中的常量池是放在 Perm 区中的，Perm 区和 JAVA Heap 区域是完全分开的。

```java
 String s3 = new String("1") + new String("1"); // 放到堆内存中
 s3.intern(); // 将s3的值放入PermGen中
 String s4 = "11"; // 在PermGen中取到值
 System.out.println(s3 == s4); // false 
```
##### JDK7

![image-20201127112419827](https://gitee.com/liuw5367/resource/raw/master/img/image-20201127112419827.png)

```java
 String s3 = new String("1") + new String("1"); // 放到堆内存中
 s3.intern(); // 将s3的值放入常量池，但是s3已经在堆中了，所以常量池中是指向s3的引用
 String s4 = "11"; // 从常量池中获取到s3的引用
 System.out.println(s3 == s4); // true 
```

### intern不当使用

在使用 fastjson 进行接口读取的时候，发现在读取了近70w条数据后，我们的日志打印变的非常缓慢，每打印一次日志用时30ms左右，如果在一个请求中打印2到3条日志以上会发现请求有一倍以上的耗时

```java
// com.alibaba.fastjson.parser.JSONScanner#scanFieldSymbol()
if (ch == '\"') {
    bp = index;
    this.ch = ch = buf[bp];
    strVal = symbolTable.addSymbol(buf, start, index - start - 1, hash);
    break;
}

//  com.alibaba.fastjson.parser.SymbolTable#addSymbol()
/**
 * Constructs a new entry from the specified symbol information and next entry reference.
 */
public Entry(char[] ch, int offset, int length, int hash, Entry next){
    characters = new char[length];
    System.arraycopy(ch, offset, characters, 0, length);
    symbol = new String(characters).intern();
    this.next = next;
    this.hashCode = hash;
    this.bytes = null;
}
```

fastjson 中对所有的 json 的 key 使用了 intern 方法，缓存到了字符串常量池中，这样每次读取的时候就会非常快，大大减少时间和空间。而且 json 的 key 通常都是不变的。这个地方没有考虑到大量的 json key 如果是变化的，那就会给字符串常量池带来很大的负担。

这个问题 fastjson 在1.1.24版本中已经将这个漏洞修复了。程序加入了一个最大的缓存大小，超过这个大小后就不会再往字符串常量池中放了。

```java
[1.1.24版本的com.alibaba.fastjson.parser.SymbolTable#addSymbol() Line:113]代码
public static final int MAX_SIZE           = 1024;

if (size >= MAX_SIZE) {
    return new String(buffer, offset, len);
}
```