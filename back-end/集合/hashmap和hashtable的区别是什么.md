# hashmap 和 hashtable 的区别是什么？

## **1、继承的父类不同**

HashMap继承自AbstractMap类。但二者都实现了Map接口。
Hashtable继承自Dictionary类，Dictionary类是一个已经被废弃的类（见其源码中的注释）。父类都被废弃，自然而然也没人用它的子类Hashtable了。

## **2、HashMap线程不安全,HashTable线程安全**

```
  javadoc中关于hashmap的一段描述如下：此实现不是同步的。如果多个线程同时访问一个哈希映射，**而其中至少一个线程从结构上修改了该映射**，则它必须保持外部同步。
  Hashtable 中的方法大多是Synchronize的，而HashMap中的方法在一般情况下是非Synchronize的。在多线程并发的环境下，可以直接使用Hashtable，不需要自己为它的方法实现同步，但使用HashMap时就必须要自己增加同步处理。HashTable实现线程安全的代价就是效率变低，因为会锁住整个HashTable,而ConcurrentHashMap做了相关优化,因为ConcurrentHashMap使用了分段锁，并不对整个数据进行锁定,效率比HashTable高很多。
  **HashMap底层是一个Entry数组，当发生hash冲突的时候，hashmap是采用链表的方式来解决的，在对应的数组位置存放链表的头结点。对链表而言，新加入的节点会从头结点加入。**
  HashMap的put方法：
1234
void addEntry(int hash, K key, V value, int bucketIndex) { //新增Entry,将“key-value”插入指定位置，bucketIndex是位置索引。
        Entry<K,V> e = table[bucketIndex];  保存“bucketIndex”位置的值到“e”中
        table[bucketIndex] = new Entry<K,V>(hash, key, value, e); 
        if (size++ >= threshold)       // 若HashMap的实际大小 不小于 “阈值”，则调整HashMap的大小2倍
            resize(2 * table.length);
    }
123456
```

在hashmap的put方法调用addEntry()方法，假如A线程和B线程同时对同一个数组位置调用addEntry，两个线程会同时得到现在的头结点，然后A写入新的头结点之后，B也写入新的头结点，那B的写入操作就会覆盖A的写入操作造成A的写入操作丢失。
 故解决方法就是使用 使用ConcurrentHashMap。
 这里要说一下 就是HashMap的迭代器(Iterator)是**fail-fast**迭代器，故当有其它线程改变了HashMap的结构（增加或者移除元素），将会抛出ConcurrentModificationException异常，而Hashtable的enumerator迭代器**不是fail-fast**的。但迭代器本身的remove()方法移除元素则不会抛出ConcurrentModificationException异常。但这并不是一个一定发生的行为，要看JVM。这条同样也是Enumeration和Iterator的区别。

## 3.包含的contains方法不同

HashMap是没有contains方法的，而包括containsValue和containsKey方法；hashtable则保留了contains方法，效果同containsValue,还包括containsValue和containsKey方法。

## 4.是否允许null值

Hashmap是允许key和value为null值的，用containsValue和containsKey方法判断是否包含对应键值对；HashTable键值对都不能为空，否则报空指针异常。

## 5.计算hash值方式不同

为了得到元素的位置，首先需要根据元素的 KEY计算出一个hash值，然后再用这个hash值来计算得到最终的位置。

①：HashMap有个hash方法重新计算了key的hash值,因为hash冲突变高，所以通过一种方法重算hash值的方法：

```
static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
1234
```

注意这里计算hash值，先调用hashCode方法计算出来一个hash值，再将hash与右移16位后相**异或**，从而得到**新的hash值**。
 **②：**Hashtable通过计算**key的hashCode()**来得到hash值就为最终hash值。

它们计算索引位置方法不同：
 HashMap在求hash值对应的位置索引时，`index = (n - 1) & hash`。将哈希表的大小固定为了2的幂，因为是取模得到索引值，**故这样取模时，不需要做除法，只需要做位运算。位运算比除法的效率要高很多。**

HashTable在求hash值位置索引时计算index的方法：

```
int index = (hash & 0x7FFFFFFF) % tab.length;
1
```

&0x7FFFFFFF的目的是为了将负的hash值转化为正值，因为hash值有可能为负数，而&0x7FFFFFFF后，只有符号位改变，而后面的位都不变。

## 6.扩容方式不同（容量不够）

当容量不足时要进行resize方法，而resize的两个步骤：
 ①扩容；
 ②rehash:这里HashMap和HashTable都会会重新计算hash值而这里的计算方式就不同了（看5）；
 HashMap 哈希扩容必须要求为原容量的2倍，而且一定是2的幂次倍扩容结果，而且每次扩容时，原来数组中的元素依次重新计算存放位置，并重新插入；
 而Hashtable扩容为原容量2倍加1；

## 7.解决hash冲突方式不同（地址冲突）

先看jdk8之前：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190613192001360.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3h1aHVhYWJj,size_16,color_FFFFFF,t_70)
 查找时间复杂度慢慢变高；
 Java8，HashMap中，当出现冲突时可以：

```
1.如果冲突数量小于8，则是以链表方式解决冲突。
2.而当冲突大于等于8时，就会将冲突的Entry转换为**红黑树进行存储。**
3.而又当数量小于6时，则又转化为链表存储。
123
```

而在HashTable中， 都是以链表方式存储。