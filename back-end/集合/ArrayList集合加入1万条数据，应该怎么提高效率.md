# **ArrayList集合加入1万条数据，应该怎么提高效率？**

因为ArrayList的底层是数组实现,并且jdk1.8之前数组的默认值是10,如果插入10000条要不断的扩容,耗费时间,所以我们调用ArrayList的指定容量的构造器方法ArrayList(int size) 就可以实现不扩容,就提高了性能。

# **ArrayList初始化**

创建ArrayList对象时，若未指定集合容量，**集合默认容量为0**；

```java
    //默认创建一个空集合
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = new Object[0];

    //ArrayList空参构造
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }
```

当集合对象调用add方法存储数据时，进行容量初始化。底层实现如下：

```java
    //ArrayList集合对象添加元素方法 
    public boolean add(E var1) {
        //扩容方法
        this.ensureCapacityInternal(this.size + 1);
        //存储元素
        this.elementData[this.size++] = var1;
        return true;
    }
    //添加元素时调用扩容方法
    private void ensureCapacityInternal(int var1) {
        //初始化状态时  默认容量变为10
        if (this.elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            var1 = Math.max(10, var1);
        }
        //非初始化状态时，判断集合是否需要扩容
        this.ensureExplicitCapacity(var1);
    }
```

**结论1:**
 当集合对象**第一次**添加元素的时候，集合大小扩容为10(若使用addAll方法添加元素,则初始化大小为**10**和**添加集合长度**的**较大值**)

当集合初始化完毕后，再次添加元素时。调用其ensureExplicitCapacity方法

```java
   private void ensureExplicitCapacity(int var1) {
        ++this.modCount;
        //如果最小需要空间比elementData的内存空间要大，扩容
        if (var1 - this.elementData.length > 0) {
            //核心扩容方法
            this.grow(var1);
        }
    }
    private void grow(int var1) {
        //获取原集合长度
        int var2 = this.elementData.length;
        //集合长度扩容1.5倍
        int var3 = var2 + (var2 >> 1);
        //如果扩容后仍小于添加集合的长度,新建集合长度为添加元素集合大小
        if (var3 - var1 < 0) {
            var3 = var1;
        }
        //若预设值大于默认的最大值检查是否溢出  
        if (var3 - 2147483639 > 0) {
            var3 = hugeCapacity(var1);
        }
        //调用Arrays.copyOf方法将elementData数组指向新的长度为扩容后长度的内存空间
        this.elementData = Arrays.copyOf(this.elementData, var3);
    }
```

**结论2：**
  集合初始化后，再次调用add方法，**先将集合扩大1.5倍**，如果仍然不够，新长度为传入**集合大小**。并调用Arrays.copyOf方法将elementData数组指向新的长度为扩容后长度的内存空间

# 总结

1.ArrayList创建对象时，若未指定集合大小初始化大小为0；若已指定大小，集合大小为指定的大小；

2.当第一次调用add方法时，集合长度变为**10**和**addAll内容之间**的**较大值**；

3.之后再调用add方法，**先将集合扩大1.5倍**，如果仍然不够，新长度为传入**集合大小；**