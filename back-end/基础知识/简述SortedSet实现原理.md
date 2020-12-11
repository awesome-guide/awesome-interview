- ## 简述 SortedSet 实现原理

  <img src="https://img2020.cnblogs.com/blog/1102083/202008/1102083-20200804221603127-819017688.png" alt="img" style="zoom: 50%;" />

- **功能说明：**

  ​		该接口扩展了 Set 接口并提供了元素的排序功能。插入的元素需实现Comparable接口（或者被指定的Comparator接受）并可以互相比较。例：e1.compareTo(e2)（或 comparator.compare(e1, e2)）

  ​		TreeSet的实现了该接口，字符串、数字、日期等底层已默认实现了Comparable接口，故在添加后可直接按顺序排列

  **类支持排序实现方式：**

  ​		1、该类实现Comparator接口并实现compare方法（见例1）

  ​		2、单建一个实现类（比较器）SortedSet构造时传入该类（见例2）

  **例1：**

  ```java
  import java.util.*;
  public class SortedSetTest02{
   public static void main(String[] args){
     SortedSet users=new TreeSet();
     User u1=new User(12);
     User u2=new User(16);
     User u3=new User(23);
     User u4=new User(32);
     User u5=new User(43);
     
     users.add(u1);
     users.add(u2);
     users.add(u3);
     users.add(u4);
     users.add(u5);
     Iterator it=users.iterator();
     while(it.hasNext()){
       System.out.println(it.next());
     }
   }
  }
  class User implements Comparable{
   int age;
   User(int age){
     this.age=age;
   }
   public String toString(){
     return "User[age="+age+"]";
   }
   //实现java.lang.Comparable;接口中的compareTo方法
   //该方法程序员负责实现，SUN提供的程序已经调用了该方法
   //需求：按照User的年龄排序
   public int compareTo(Object o){  //u1.compareTo(u2)
     //编写一个比较规则
     int age1=this.age;   
     int age2=((User)o).age;
     return age1-age2;
   }
  }
  ```

  **例2：**

  ```java
  import java.util.*;
  public class SortedSetTest03{
   public static void main(String[] args){
     //创建TreeSet集合的时候提供一个比较器
     SortedSet products=new TreeSet(new ProductComparator());
     Product p1=new Product(3.4);
     Product p2=new Product(4.0);
     Product p3=new Product(3.6);
     Product p4=new Product(7.6);
     Product p5=new Product(3.7);
     
     products.add(p1);
     products.add(p2);
     products.add(p3);
     products.add(p4);
     products.add(p5);
     
     Iterator it=products.iterator();
     while(it.hasNext()){
       System.out.println(it.next());
       }
   }
  }
  class Product{
   double price;
   Product(double price){
     this.price=price;
   }
   public String toString(){
     return price + "";
   }
  }
  //单独编写一个比较器
  class ProductComparator implements Comparator{
   //需求：按照商品价格排序
   public int compare(Object o1,Object o2){
     double price1=((Product)o1).price;
     double price2=((Product)o2).price;
     if (price1==price2){
       return 0;
     }else if(price1>price2){
       return -1;
     }
     return 1;
   }
  }
  ```

  **查看源码看它在添加时是如何实现自动排序与去重的**

  ​	1、**SortedSet sortSet=new TreeSet()**

  ```java
  private transient NavigableMap<E,Object> m;
  private static final Object PRESENT = new Object();
  
  public TreeSet() {
          this(new TreeMap<E,Object>());
  }
  TreeSet(NavigableMap<E,Object> m) {
          this.m = m;
  }
  ```

  ​	2、**sortSet.add("a");**

  ```java
  public boolean add(E e) {
          return m.put(e, PRESENT)==null;
   }
  
  ```

  ​	**接着来看下put，这里看TreeMap**

  ```java
  public V put(K key, V value) {
          Entry<K, V> t = root;
      	//根节点为空则将当前设置为根节点
          if (t == null) {
              compare(key, key); // type (and possibly null) check
  
              root = new Entry<>(key, value, null);
              size = 1;
              modCount++;
              return null;
          }
          int cmp;
          Entry<K, V> parent;
          // 进行比较若key值相同则直接赋值返回；获取key的父节点
          Comparator<? super K> cpr = comparator;
          if (cpr != null) {
              do {
                  parent = t;
                  cmp = cpr.compare(key, t.key);
                  if (cmp < 0)
                      t = t.left;
                  else if (cmp > 0)
                      t = t.right;
                  else
                      return t.setValue(value);
              } while (t != null);
          } else {
              if (key == null)
                  throw new NullPointerException();
              @SuppressWarnings("unchecked")
              Comparable<? super K> k = (Comparable<? super K>) key;
              do {
                  parent = t;
                  cmp = k.compareTo(t.key);
                  if (cmp < 0)
                      t = t.left;
                  else if (cmp > 0)
                      t = t.right;
                  else
                      return t.setValue(value);
              } while (t != null);
          }
          Entry<K, V> e = new Entry<>(key, value, parent);
          if (cmp < 0)
              parent.left = e;
          else
              parent.right = e;
      	// 对插入的节点进行调整
          fixAfterInsertion(e);
          size++;
          modCount++;
          return null;
      }
  ```

  ```java
  //使用自身对象还是比较器
  final int compare(Object k1, Object k2) {
          return comparator == null ? ((Comparable<? super K>) k1).compareTo((K) k2)
                  : comparator.compare((K) k1, (K) k2);
      }
  ```

  **注意事项：**

  1、SortedSet 是“根据对象的比较顺序”，而不是“插入顺序”进行排序.

  2、SortedSet 的实现不是同步的，不是线程安全的

  3、SortedSet 允许 null 元素.

  4、SortedSet 的实现类 TreeSet 将使用其 compareTo（或 compare）方法执行所有的键比较，认为两个对象的键相等就表示它们两个对象是相等的。它违背了 Set 接口的常规协定。

  5、SortedSet 的 iterator 方法返回的迭代器是快速失败的

  在创建迭代器之后，如果对集合进行修改， 除非通过迭代器自身的 remove 方法，否则在任何时间以任何方式对其进行修改， Iterator 都将抛出 ConcurrentModificationException

  6、iterator() 返回的迭代器，里面的元素是以升序排序的

  7、当试图添加一个重复元素到TreeSet时，新元素并不会把旧元素替换掉，
  而只是新元素不会添加到TreeSet（不会抛异常。）

  8、 TreeSet 用红黑树的数据结构来为元素排序