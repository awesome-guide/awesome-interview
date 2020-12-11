# java 创建对象的几种方式

| 使用new关键字                      | 调用了构造函数   |
| ---------------------------------- | ---------------- |
| 使用Class类的newInstance方法       | 调用了构造函数   |
| 使用Constructor类的newInstance方法 | 调用了构造函数   |
| 使用clone方法                      | 没有调用构造函数 |
| 使用反序列化                       | 没有调用构造函数 |
| 使用Unsafe                         | 没有调用构造函数 |

1. New关键字

   ```java
   new String("")
   ```

2. Class.newInstance()

   ```java
   People.class.newInstance()
   Class.forName("People").newInstance()
   ```

3. Constructor.newInstance()

   ```java
   Constructor<People> constructor = People.class.getConstructor();
   constructor.newInstance();
   ```

4. clone()

   要使用clone方法，需要先实现Cloneable接口并实现其定义的clone方法。

   jvm会创建一个新的对象，将前面对象的内容全部拷贝进去

   ```java
   class People implements Cloneable {
       @Override
       protected Object clone() throws CloneNotSupportedException {
         return super.clone();
       }
   }
   ```

5. 反序列化

   如果有父类，但是父类没有实现Serializable，会调用父类的构造方法

   ```java
   ObjectInputStream in = new ObjectInputStream(new FileInputStream("people"));
   People people = (People) in.readObject();
   ```

6. unSafe

   sun.misc.Unsafe中提供`allocateInstance`方法，仅通过Class对象就可以创建此类的实例对象，而且不需要调用其构造函数、初始化代码、JVM安全检查等。它抑制修饰符检测，也就是即使构造器是private修饰的也能通过此方法实例化，只需提类对象即可创建相应的对象。由于这种特性，allocateInstance在java.lang.invoke、Objenesis（提供绕过类构造器的对象生成方式）、Gson（反序列化时用到）中都有相应的应用。

   ```java
   import java.lang.reflect.Field;
   
   import sun.misc.Unsafe;
   public class Demo {
   
       static Unsafe unsafe;
   
       static {
           //获取Unsafe对象
           try {
               Field field = Unsafe.class.getDeclaredField("theUnsafe");
               field.setAccessible(true);
               unsafe = (Unsafe) field.get(null);
           } catch (Exception e) {
               e.printStackTrace();
           }
       }
   
       static class C1 {
           private String name;
   
           private C1() {
               System.out.println("C1 default constructor!");
           }
   
           private C1(String name) {
               this.name = name;
               System.out.println("C1 有参 constructor!");
           }
           
           public void test(){
               System.out.println("执行了test方法");
           }
       }
   
       public static void main(String[] args) throws InstantiationException {
           C1 c= (C1) unsafe.allocateInstance(C1.class);
           System.out.println(c);
           c.test();
       }
   }
   ```
   [美团：Java魔法类：Unsafe应用解析](https://tech.meituan.com/2019/02/14/talk-about-java-magic-class-unsafe.html)

   

