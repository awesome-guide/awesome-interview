### 简述 Java的反射机制 

#### 什么是反射机制？

1、在运行状态中，对于任意一个类，都能够知道这个类的属性和方法。
2、对于任意一个对象，都能够调用它的任何方法和属性。这种动态获取信息以及动态调用对象的方法的功能称为JAVA的反射。

#### 反射的作用

1、在运行时判断任意一个对象所属的类；
2、在运行时构造任意一个类的对象；
3、在运行时判断任意一个类所具有的成员变量和方法；
4、在运行时调用任意一个对象的方法；生成动态代理。

#### 反射的原理

反射的实现主要借助以下四个类：Class、Constructor、Field、Method ；
1、通过Class类获取类对象的三种方法：
第一种：通过类名获得:Class<?> class = *ClassName*.class;
第二种：通过类名全路径获得：Class<?> class = Class.forName(“类名全路径”);
第三种：通过实例对象获得：Class<?> class = object.getClass();

2、通过Class类获取实现类实例化：
      1)Object o = (Object) c.newInstance() ; (其中c为Class类的实例化对象）

​      2)指定的所有公共构方法中的一种

```java
Class clazz = NluHelper.class;
Constructor constructor=clazz.getConstructor(String.class);
constructor.newInstance("张三");
```

​	3)指定当前class构造方法中的一种

```java
Class clazz = NluHelper.class;
Constructor constructor=clazz.getDeclaredConstructor(String.class);
constructor.newInstance("张三");
```

​	4)获取当前类所有构造方法及其属性

```java
Class clazz = NluHelper.class;
        Constructor[] constructors = clazz.getDeclaredConstructors();
        for (Constructor c : constructors) {
            System.out.println(c.getName());
            Class params[] = c.getParameterTypes();
            for (Class param:params){
                System.out.println(param.getName());
            }
        }
```



3、获取类中的构造方法：![在这里插入图片描述](https://images2015.cnblogs.com/blog/831179/201703/831179-20170311161343451-803699800.png)![在这里插入图片描述](https://images2015.cnblogs.com/blog/831179/201703/831179-20170311161408982-698240323.png)
4、获取类中的属性：

![在这里插入图片描述](https://images2015.cnblogs.com/blog/831179/201703/831179-20170311161735389-1882210002.png)
![在这里插入图片描述](https://images2015.cnblogs.com/blog/831179/201703/831179-20170311161755498-1621406504.png)

5、获取类中的方法：

![在这里插入图片描述](https://images2015.cnblogs.com/blog/831179/201703/831179-20170311161823873-503164825.png)
![在这里插入图片描述](https://images2015.cnblogs.com/blog/831179/201703/831179-20170311161907357-844512641.png)

#### 反射的使用场景

-   Java编码时知道类和对象的具体信息，此时直接对类和对象进行操作即可，无需反射
-   如果编码时不知道类或者对象的具体信息，此时应该使用反射来实现

​      比如类的名称放在XML文件中，属性和属性值放在XML文件中，需要在运行时读取XML文件，动态获取类的信息
​      在编译时根本无法知道该对象或类可能属于哪些类，程序只依靠运行时信息来发现该对象和类的真实信息