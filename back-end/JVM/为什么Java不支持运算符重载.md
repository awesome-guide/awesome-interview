## 为什么Java不支持运算符重载

1. **简单性和清晰性**。清晰性是Java设计者的目标之一。设计者不是只想复制语言，而是希望拥有一种清晰，真正面向对象的语言。添加运算符重载比没有它肯定会使设计更复杂，并且它可能导致更复杂的编译器, 或减慢 JVM，因为它需要做额外的工作来识别运算符的实际含义，并减少优化的机会, 以保证 Java 中运算符的行为。

2. **避免编程错误**。Java 不允许用户定义的运算符重载，因为如果允许程序员进行运算符重载，将为同一运算符赋予多种含义，这将使任何开发人员的学习曲线变得陡峭，事情变得更加混乱。据观察，当语言支持运算符重载时，编程错误会增加，从而增加了开发和交付时间。由于 Java 和 JVM 已经承担了大多数开发人员的责任，如在通过提供垃圾收集器进行内存管理时，因为这个功能增加污染代码的机会, 成为编程错误之源, 因此没有多大意义。

3. **JVM复杂性**。从JVM的角度来看，支持运算符重载使问题变得更加困难。通过更直观，更干净的方式使用方法重载也能实现同样的事情，因此不支持 Java 中的运算符重载是有意义的。与相对简单的 JVM 相比，复杂的 JVM 可能导致 JVM 更慢，并为保证在 Java 中运算符行为的确定性从而减少了优化代码的机会。

4. **让开发工具处理更容易**。这是在 Java 中不支持运算符重载的另一个好处。省略运算符重载后使语言更容易处理，如静态分析等，这反过来又更容易开发处理语言的工具，例如 IDE 或重构工具。Java 中的重构工具远胜于 C++。

- C++ 运算符重载

  ```c++
  // 定义
  Box operator+(const Box&);
  Box operator+(const Box&, const Box&);
  
  #include <iostream>
  using namespace std;
   
  class Box{
     public:
        double getVolume(void){  return length * breadth * height; }
        void setLength( double len ) {   length = len;  }
        void setBreadth( double bre ) {   breadth = bre;  }
        void setHeight( double hei ) {   height = hei; }
        // 重载 + 运算符，用于把两个 Box 对象相加
        Box operator+(const Box& b)
        {
           Box box;
           box.length = this->length + b.length;
           box.breadth = this->breadth + b.breadth;
           box.height = this->height + b.height;
           return box;
        }
     private:
        double length;      // 长度
        double breadth;     // 宽度
        double height;      // 高度
  };
  // 程序的主函数
  int main( )
  {
     Box Box1;                // 声明 Box1，类型为 Box
     Box Box2;                // 声明 Box2，类型为 Box
     Box Box3;                // 声明 Box3，类型为 Box
     double volume = 0.0;     // 把体积存储在该变量中
     // Box1 详述
     Box1.setLength(6.0); 
     Box1.setBreadth(7.0); 
     Box1.setHeight(5.0);
     // Box2 详述
     Box2.setLength(12.0); 
     Box2.setBreadth(13.0); 
     Box2.setHeight(10.0);
     // Box1 的体积
     volume = Box1.getVolume();
     cout << "Volume of Box1 : " << volume <<endl;
     // Box2 的体积
     volume = Box2.getVolume();
     cout << "Volume of Box2 : " << volume <<endl;
   
     // 把两个对象相加，得到 Box3
     Box3 = Box1 + Box2;
   
     // Box3 的体积
     volume = Box3.getVolume();
     cout << "Volume of Box3 : " << volume <<endl;
   
     return 0;
  }
  ```

- Kotlin运算符重载

  ```kotlin
  // 一个简单的数据类
  data class Foo(val x: Int, val y: Int) {
      operator fun plus(other: Foo) : Foo {
          return Foo(x + other.x, y + other.y)
      }
  }
  
  fun main(args: Array<String>) {
      // 使用的时候
      val f1 = Foo(10, 20)
      val f2 = Foo(30, 40)
      // 直接用+运算符代替plus函数，事实上会调用plus函数
      println(f1 + f2) // 打印内容为Foo(x=40, y=60)
  }
  
  // Extension
  operator fun Foo.minus(other: Foo): Foo = Foo(x - other.x, y - other.y)
  operator fun Foo.div(other: Foo): Foo = Foo(x / other.x, y / other.y)
  
  data class Foo(val x: Int, val y: Int) {
      operator fun plus(other: Foo): Foo = Foo(x + other.x, y + other.y)
      operator fun times(other: Foo): Foo = Foo(x * other.x, y * other.y)
      operator fun rem(other: Foo): Foo = Foo(x % other.x, y % other.y)
  }
  
  fun main(args: Array<String>) {
      val f1 = Foo(30, 40)
      val f2 = Foo(10, 20)
      (f1 - f2).println() // 打印Foo(x=20, y=20)
      (f1 + f2).println() // 打印Foo(x=40, y=60)
      (f1 * f2).println() // 打印Foo(x=300, y=800)
      (f1 / f2).println() // 打印Foo(x=3, y=2)
      (f1 % f2).println() // 打印Foo(x=0, y=0)
  }
  ```

  ```kotlin
  class AA(var value: Int = 0) {
    operator fun plus(a: AA): AA {
      return AA(a.value + value)
    }
  }
  
  object X {
    fun main(args: Array<String>) {
      val a1 = AA(1)
      val a2 = AA(2)
      val a3 = a1 + a2
      println(a3.value)
    }
  }
  
  //////////////////////////////////////
  // 反编译成java，可以看到实际调用的是plus方法
  ////////////////////////////
  public final class AA {
     private int value;
  
     public final AA plus(@NotNull AA a) {
        return new AA(a.value + this.value);
     }
  
     public AA(int value) {
        this.value = value;
     }
  }
  
  public final void main(@NotNull String[] args) {
      AA a1 = new AA(1);
      AA a2 = new AA(2);
      AA a3 = a1.plus(a2); // 调用plus
      System.out.println(var5);
  }
  ```

  