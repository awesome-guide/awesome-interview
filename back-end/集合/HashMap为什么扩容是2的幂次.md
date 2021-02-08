# **HashMap为什么扩容是2的幂次**?

#### 一、HashMap通过哈希算法得出哈希值之后，将键值对放入哪个索引的方法

```
static int indexFor(int h, int length) {
	return h & (length-1);	
}
```

HashMap的容量为16转化成二进制为10000，length-1得出的二进制为01111 

哈希值为1111 

![img](https://img-blog.csdn.net/20180313143947566?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeGluXzM2OTEwMzAw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

可以得出索引的位置为15

假设 

HashMap的容量为15转化成二进制为1111，length-1得出的二进制为1110 

哈希值为1111(15)和1110 (14)

![img](https://img-blog.csdn.net/20180313144241826?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeGluXzM2OTEwMzAw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)  

那么两个索引的位置都是14，产生了相同的结果，也就是说它们会定位到数组中的同一个位置上去，这就产生了碰撞，15和14会被放到同一个链表上，那么查询的时候就需要遍历这个链表，这样就降低了查询的效率。同时，我们也可以发现，当数组长度为15的时候，hashcode的值会与14（1110）进行“与”，那么最后一位永远是0，而0001，0011，0101，1001，1011，0111，1101这几个位置永远都不能存放元素了，空间浪费相当大，更糟的是这种情况中，数组可以使用的位置比数组长度小了很多，这意味着进一步增加了碰撞的几率，减慢了查询的效率！ 
所以说，当数组长度为2的n次幂的时候，不同的key算得得index相同的几率较小，那么数据在数组上分布就比较均匀，也就是说碰撞的几率小，相对的，查询的时候就不用遍历某个位置上的链表，这样查询效率也就较高了。

**总结：**
如果不是2的n次幂的时候：

**增加了碰撞的几率，**

**减慢了查询的效率，**

**造成空间的浪费。** 