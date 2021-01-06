# char 型变量中能不能存贮一个中文汉字

 char型变量是用来存储Unicode编码的字符的，unicode编码字符集中包含了汉字，
 所以，char型变量中当然可以存储汉字啦。

如果某个特殊的汉字没有被包含在unicode编码字符集中，那么，这个char型变量中就不能存储这个特殊汉字。

补充：unicode编码占用两个字节，所以，char类型的变量也是占用两个字节。




## Java9开始String使用byte[]实现

![](https://gitee.com/liuw5367/resource/raw/master/img/下载.jpg)

从 Java9 版本开始，String的实现将 char[]字段改为了 byte[]字段，又维护了一个新的属性 coder，它是一个编码格式的标识。

```java
/**
 * The value is used for character storage.
 *
 * @implNote This field is trusted by the VM, and is a subject to
 * constant folding if String instance is constant. Overwriting this
 * field after construction will cause problems.
 *
 * Additionally, it is marked with {@link Stable} to trust the contents
 * of the array. No other facility in JDK provides this functionality (yet).
 * {@link Stable} is safe here, because value is never null.
 */
@Stable
private final byte[] value;
/**
 * The identifier of the encoding used to encode the bytes in
 * {@code value}. The supported values in this implementation are
 *
 * LATIN1
 * UTF16
 *
 * @implNote This field is trusted by the VM, and is a subject to
 * constant folding if String instance is constant. Overwriting this
 * field after construction will cause problems.
 */
private final byte coder;
/** Cache the hash code for the string */
private int hash; // Default to 0

/**
 * Cache if the hash has been calculated as actually being zero, enabling
 * us to avoid recalculating this.
 */
private boolean hashIsZero; // Default to false;
```

为什么这样修改呢？一个 char 字符占 16 位，2 个字节。这个情况下，存储单字节编码内的字符（占一个字节的字符）就显得非常浪费。

JDK1.9 的 String 类为了节约内存空间，于是使用了占 8 位，1 个字节的 byte 数组来存放字符串。而新属性 coder 的作用是，在计算字符串长度或者使用 indexOf（）函数时，我们需要根据这个字段，判断如何计算字符串长度。coder 属性默认有 0 和 1 两个值，0 代表 Latin-1（单字节编码），1 代表 UTF-16。如果 String 判断字符串只包含了 Latin-1，则 coder 属性值为 0，反之则为 1。

```java
public int compareTo(String anotherString) {
    byte v1[] = value;
    byte v2[] = anotherString.value;
    byte coder = coder();
    if (coder == anotherString.coder()) {
        return coder == LATIN1 ? StringLatin1.compareTo(v1, v2)
                               : StringUTF16.compareTo(v1, v2);
    }
    return coder == LATIN1 ? StringLatin1.compareToUTF16(v1, v2)
                           : StringUTF16.compareToLatin1(v1, v2);
 }

byte coder() {
    return COMPACT_STRINGS ? coder : UTF16;
}
```