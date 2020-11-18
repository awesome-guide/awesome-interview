# 如果你的Serializable类包含一个不可序列化的成员，会发生什么？你是如何解决的？

会出现 `NotSerializableException`。可以给不需要序列化的属性添加`trancient`关键字，避免字段序列化

```java
class Data implements Serializable {
   String value;
   People people;
   trancient People trancientPeople;
   static String value;
}
class People {
   String name;
}

public static void main(String[] args) {
    Data data = new Data();
    data.aaa = "111";

    People people = new People();
    people.name = "name111";
    people.email = "email@email.com";
    data.people = people;

    try {
        FileOutputStream fos = new FileOutputStream("Serializable");
        ObjectOutputStream oos = new ObjectOutputStream(fos);
        oos.writeObject(data);

        FileInputStream fis = new FileInputStream("Serializable");
        ObjectInputStream is = new ObjectInputStream(fis);
        Data p = (Data) is.readObject();
        System.out.println(p);
    } catch (Exception e) {
        e.printStackTrace();
    }
}
// 
java.io.NotSerializableException: SerializableTest$People
	at java.io.ObjectOutputStream.writeObject0(ObjectOutputStream.java:1184)
	at java.io.ObjectOutputStream.defaultWriteFields(ObjectOutputStream.java:1548)
	at java.io.ObjectOutputStream.writeSerialData(ObjectOutputStream.java:1509)
	at java.io.ObjectOutputStream.writeOrdinaryObject(ObjectOutputStream.java:1432)
	at java.io.ObjectOutputStream.writeObject0(ObjectOutputStream.java:1178)
	at java.io.ObjectOutputStream.writeObject(ObjectOutputStream.java:348)
	at algorithm.test.fun.SerializableTest.main(SerializableTest.java:18)
```

**trancient** ：短暂的; 转瞬即逝的

- 变量被transient修饰不能被序列化
- transient关键字只能修饰变量，而不能修饰方法和类
- 变量如果是用户自己定义的类变量，则该类也需要实现Serializable接口。
- 静态变量不能被序列化。反序列化后类中 static 型变量 的值为当前 JVM 中对应的静态变量的值，而不是反序列化得出的
- 如果父类未实现Serializable，不会报错，但不会序列化父类的变量

## transient关键字修饰过得变量真的不能被序列化吗

实现`Externalizable`接口，需要在`writeExternal()`方法中进行手工指定所要序列化的变量，这与是否被transient修饰无关。没有被transient修饰的变量，如果writeExternal中没有写入也不会被序列化

```java
static class People implements Externalizable {
    public transient String name;
    public String email;
    public People() {}

    @Override
    public void writeExternal(ObjectOutput out) throws IOException {
      out.writeObject(name);
  
    }

    @Override
    public void readExternal(ObjectInput in) throws IOException, ClassNotFoundException {
      name = (String) in.readObject();
    }
}
```

## serialVersionUID作用

serialVersionUID 是一个 private static final long 型 ID，当它被印在对象上时，它通常是对象的哈希码。serialVersionUID 可以自己定义，也可以自己去生成

**不指定 serialVersionUID 的后果是**：当你添加或修改类中的任何字段时，已序列化类将无法恢复，因为新类和旧序列化对象生成的 serialVersionUID 将有所不同。Java 序列化的过程是依赖于正确的序列化对象恢复状态的，并在序列化对象序列版本不匹配的情况下引发 java.io.InvalidClassException 无效类异常。

```log
java.io.InvalidClassException: SerializableTest$Data; 
    local class incompatible: stream classdesc serialVersionUID = 4312185179874517942, 
    local class serialVersionUID = 182408184359394773
```

## Java序列化缺陷

- 无法跨语言

- [易被攻击](https://www.jianshu.com/p/c25c3eea9276)

  对象是通过在 ObjectInputStream 上调用 readObject() 方法进行反序列化的，这个方法其实是一个神奇的构造器，它可以将类路径上几乎所有实现了 Serializable 接口的对象都实例化。这也就意味着，在反序列化字节流的过程中，该方法可以执行任意类型的代码，这是非常危险的。

- 序列化后的流太大

- 序列化性能太差
