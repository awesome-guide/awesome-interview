1.final的内存语义

- 写内存语义：在构造函数内对一个 final 域的写入，与随后将对象引用赋值给引用变量，这两个操作不能重排序。

- 释义：写内存语义可以确保在对象的引用为任意线程可见之前，final 域已经被初始化过了。

- 读内存语义：初次读一个包含 final 域的对象的引用，与随后初次读这个 final 域，这两个操作不能重排序。

- 释义：读内存语义可以确保如果对象的引用不为 null，则说明 final 域已经被初始化过了。

  总结：final 域的内存语义提供了初始化安全保证。存在这样的特性，猜想是与final的特点有关。

  详解：https://www.cnblogs.com/senlinyang/p/7875468.html

  扩展：指令重排序涉及到JMM

  代码

  public class FinalTest {
      int i;//普通变量
      final int j;
      static FinalTest obj;

  ```java
  public FinalTest(){
      i = 1;
      j = 2;
  }
  
  public static void writer(){
      obj = new FinalTest();
  }
  
  public static void reader(){
      FinalTest tets = obj;//读对象引用
      int a = tets.i;//读取i之前，tets可能为空，这样就报异常
      int b = tets.j;//读取j之前，tets一定不会为空，会设置屏障
  }
  ```
  }

final有哪些用法和注意事项

修饰类：

当final修饰一个类时，表明其为最终类，它不能被继承，并且类中所有的属性和方法都默认是final类型，如String，Integer等包装类均为final类。

被final修饰的方法不可被重写。它可以防止任何继承类修改方法的意义和实现，而且，使用final修饰方法的执行效率一般高于普通方法，这里不得不提一下Java的内联机制。

> 当我们调用方法时，实际上是将程序的执行转移到该方法所在的内存地址上，将该方法执行完后，再返回到执行该方法前的位置，这种转移操作要求在转移前保存当前的数据以及内存地址，在执行完后再恢复现场，继续按照转移前的地址执行，也就是通常所说的压栈和出栈（这段文字有点绕口，简单来说，比如A方法在执行到第10行的时候调用了B方法，JVM会先保存A方法当前数据和执行地址，然后跳转到B方法所在的内存地址执行B方法，执行完后，再返回A方法第十行继续执行），因此，函数调用有一定时间和空间方面的开销，对于函数体积不大，但是频繁调用的函数来说，这个开销就会放大。
> 因此，对于这种函数体积不大又频繁调用的的方法，我们可以通过内联函数来提升运行效率，当我们对一个方法使用final修饰时，这个方法就有可能成为内联函数（JVM会根据方法的执行效率决定是否内联）。

看下面代码，解释函数内联前后的区别。

内联前：

```java
class A {
    int value;
    public final int get(){
        return value;
    }
}

public class B {
    public void sum() {
        A a = new A(); 
        //调用a的get方法
        int x = a.get();
    }
}
```

内联后：

```java
class A {
    int value;
    public final int get(){
        return value;
    }
}

public class B {
    public void sum() {
        A a = new A(); 
        //此处将get方法展开为内联函数
        int x = a.value;
    }
}
```

局部变量

赋值后不可变，基本变量的值不可变，引用类型内存地址不可能边，成员变量的值可变

成员变量

1.初始化时可在构造器中赋值

2.直接赋值