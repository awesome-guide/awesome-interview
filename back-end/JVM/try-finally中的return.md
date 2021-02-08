- 情况一：如果finally中有return语句，则会将try中的return语句”覆盖“掉，直接执行finally中的return语句，得到返回值，这样便无法得到try之前保留好的返回值。
- 情况二：如果finally中没有return语句，也没有改变要返回值，则执行完finally中的语句后，会接着执行try中的return语句，返回之前保留的值。
- 情况三：如果finally中没有return语句，但是改变了要返回的值，这里有点类似与引用传递和值传递的区别，分以下两种情况，：
1. 如果return的数据是基本数据类型或文本字符串，则在finally中对该基本数据的改变不起作用，try中的return语句依然会返回进入finally块之前保留的值。
1. 如果return的数据是引用数据类型，而在finally中对该引用数据类型的属性值的改变起作用，try中的return语句返回的就是在finally中改变后的该属性的值。



使用字节码举例

1.基本数据变量，finally中改变值，但没有return

源码：

```
public class Test {
    public static void main(String[] args) {
        get(1);
    }

    public static int get(int i){
        try {
            i = 2;
            return i;
        }catch (Exception e){
            return i;
        }finally {
            i = 2;
        }
    }
}
```

字节码：

 public static int get(int);
    Code:
       0: iconst_2
       1: istore_0
       2: iload_0
       3: istore_1
       4: iconst_2
       5: istore_0
       6: iload_1
       7: ireturn
       8: astore_1
       9: iload_0
      10: istore_2
      11: iconst_2
      12: istore_0
      13: iload_2
      14: ireturn
      15: astore_3
      16: iconst_2
      17: istore_0
      18: aload_3
      19: athrow

finally中有try

 public static int get(int);
    Code:
       0: iconst_2
       1: istore_0
       2: iload_0
       3: istore_1
       4: iconst_3
       5: istore_0
       6: iload_0
       7: ireturn
       8: astore_1
       9: iload_0
      10: istore_2
      11: iconst_3
      12: istore_0
      13: iload_0
      14: ireturn
      15: astore_3
      16: iconst_3
      17: istore_0
      18: iload_0
      19: ireturn



2.引用型变量，finally中改变值，但没有return

源码：

```java
public class Test {
    public static void main(String[] args) {
        StringBuffer stringBuffer = new StringBuffer(1);
        get(stringBuffer);
    }

    public static StringBuffer get(StringBuffer stringBuffer){
        try {
            stringBuffer.append(2);
            return stringBuffer;
        }catch (Exception e){
            return stringBuffer;
        }finally {
            stringBuffer.append(3);
        }
    }
}
```

字节码：

 public static java.lang.StringBuffer get(java.lang.StringBuffer);
    Code:
       0: aload_0
       1: iconst_2
       2: invokevirtual #5                  // Method java/lang/StringBuffer.append:(I)Ljava/lang/StringBuffer;
       5: pop
       6: aload_0
       7: astore_1
       8: aload_0
       9: iconst_3
      10: invokevirtual #5                  // Method java/lang/StringBuffer.append:(I)Ljava/lang/StringBuffer;
      13: pop
      14: aload_1
      15: areturn
      16: astore_1
      17: aload_0
      18: astore_2
      19: aload_0
      20: iconst_3
      21: invokevirtual #5                  // Method java/lang/StringBuffer.append:(I)Ljava/lang/StringBuffer;
      24: pop
      25: aload_2
      26: areturn
      27: astore_3
      28: aload_0
      29: iconst_3
      30: invokevirtual #5                  // Method java/lang/StringBuffer.append:(I)Ljava/lang/StringBuffer;
      33: pop
      34: aload_3
      35: athrow

finally中有return

public static java.lang.StringBuffer get(java.lang.StringBuffer);
    Code:
       0: aload_0
       1: iconst_2
       2: invokevirtual #5                  // Method java/lang/StringBuffer.append:(I)Ljava/lang/StringBuffer;
       5: pop
       6: aload_0
       7: astore_1
       8: aload_0
       9: iconst_3
      10: invokevirtual #5                  // Method java/lang/StringBuffer.append:(I)Ljava/lang/StringBuffer;
      13: pop
      14: aload_0
      15: areturn
      16: astore_1
      17: aload_0
      18: astore_2
      19: aload_0
      20: iconst_3
      21: invokevirtual #5                  // Method java/lang/StringBuffer.append:(I)Ljava/lang/StringBuffer;
      24: pop
      25: aload_0
      26: areturn
      27: astore_3
      28: aload_0
      29: iconst_3
      30: invokevirtual #5                  // Method java/lang/StringBuffer.append:(I)Ljava/lang/StringBuffer;
      33: pop
      34: aload_0
      35: areturn

结论：从上述可以，在finally中存在return时，字节码中最后会执行一次return，而在finally中没有return时，在finally中

改变基本数据类型时，不影响try中的返回，而对象是地址传递，所以在finally中没有return，但改变的对象属性时，是会

影响try中的返回值。