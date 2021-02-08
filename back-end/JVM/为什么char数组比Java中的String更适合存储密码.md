## 为什么char数组比Java中的String更适合存储密码

**安全问题**

因为字符串是不可变对象，如果作为普通文本存储密码，那么它会一直存在堆内存中直至被垃圾收集器回收。

任何可以访问内存的人都能以明码的方式把密码dump出来。

![image-20201127095801625](https://gitee.com/liuw5367/resource/raw/master/img/image-20201127095801625.png)

**应该始终使用加密密码而不是纯文本**

由于字符串是不可变的，任何更改都会产生新的字符串，而如果使用**`char[]`可以将所有元素设置为空白或零**。

```java
String strPassword =“Unknown”;
char [] charPassword = new char [] {'U'，'n'，'k'，'w'，'o'，'n'};
System.out.println(“字符串密码：”+ strPassword);
System.out.println(“字符密码：”+ charPassword);

==========

字符串密码：Unknown
字符密码：[C @110b053
```
