1.exception和error都是继承了throwable类，在java中只有throwable类型的实例才可以被抛出(throw)或者捕获(catch)，它是异常处理机制的基本组成类型

2.exception和error体现了java平台设计者对不同异常情况的分类。exception是程序正常运行中，可以预料的意外情况，并且应该被捕获，进行相应的处理

3.error是指在正常情况下，不大可能出现的情况，绝大部分的error都会导致程序(比如jvm自身)处于非正常的、不可恢复的状态。既然是非正常情况，所以不便于也不需要捕获，常见的比如OutOfMemoryError之类，都是Error的子类

4.Exception又分为可检查异常和不可检查异常，可检查异常在源代码里必须显示的进行捕获处理，这是编译期检查的一部分。不可检查异常就是所谓的运行时异常，类似NullPointerException、ArrayIndexOutOfBoundsException之类，通常是可以编码避免的逻辑错误，具体可以根据需要来判断是否需要捕获，并不会在编译期强制要求

总结：Error是不可在程序中处理的，一般是系统导致的，例如jvm内存不足，栈深度不够。二Exception则是由于代码错误导致的，可以被捕获处理的。

图解：

![9CD3D207-C8AE-4913-AFE3-5BFE4A008B59](https://gitee.com/liuy363/blogImage/raw/master/9CD3D207-C8AE-4913-AFE3-5BFE4A008B59.png)

