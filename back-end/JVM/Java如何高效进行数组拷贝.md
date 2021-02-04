# Java如何高效进行数组拷贝

## 最快速：System.arraycopy()

```java
public static native void arraycopy(Object src,  int  srcPos,Object dest, int destPos,int length);
```

`@HotSpotIntrinsicCandidate` 这个注解是 HotSpot VM 标准的注解，被它标记的方法代表它为 HotSpot VM 的固有方法，它经过手工编写汇编或其余优化方法来进行 Java 数组拷贝， HotSpot VM 会对其作一些加强处理以提升它的执行性能。

虽然这里被声明为 native 方法，可是它跟 JDK 中其余的本地方法实现地方不一样，固有方法会在 JVM 内部实现，而其余的会在 JDK 库中实现。在调用方面，因为直接调用 JVM 内部实现，不走常规 JNI lookup，因此也省了开销。

## for循环为数组逐一赋值

循环遍历数组每个元素并进行拷贝，如果没有被编译器优化，它对应的就是遍历数组操作的字节码，执行引擎就根据这些字节码循环获取数组的每个元素再执行拷贝操作。

这个在小数据量级别既简单又快速，数据量大以后就很慢

## 克隆

```java
protected native Object clone() throws CloneNotSupportedException;
```

数组调用`clone`方法克隆新对象赋给目标数组，但clone()方法的返回值是Object，还需要强制转换成具体的数据类型，会耗费那么一些性能。但整体而言，如果数组元素非常多的话，使用clone()性能已经非常不错了

## 使用Arrays.copyOf()

```java
public static <T,U> T[] copyOf(U[] original, int newLength, Class<? extends T[]> newType) {
    T[] copy = ((Object)newType == (Object)Object[].class)
        ? (T[]) new Object[newLength]
        : (T[]) Array.newInstance(newType.getComponentType(), newLength);
    System.arraycopy(original, 0, copy, 0,
                     Math.min(original.length, newLength));
    return copy;
}
```

**Arrays.copyOf()**也是native修饰的，但从源码可知它实际又调用了System.arraycopy()，并且也存在着强制类型转换，因此性能稍微比System.arraycopy()低了点。

[arrayCopy()源码](https://www.shangmayuan.com/a/5d073af1cda54928b5095351.html)
