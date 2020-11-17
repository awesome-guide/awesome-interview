## Thread

##### Java创建线程的方式

1. 继承Thread类，并复写run方法，创建该类对象，调用start方法开启线程。此方式没有返回值 

```java
public class MyThread extends Thread {
   @Override
    public void run() {
        // 执行业务逻辑
    }

    public static void main(String[] args) {
        MyThread myThread = new MyThread();
        myThread.start();
    }
}
```



2. 实现Runnable接口，复写run方法，创建Thread类对象，将Runnable子类对象传递给Thread类对象。调用start方法开启线程。此方法2较之方法1好，将线程对象和线程任务对象分离开。降低了耦合性，利于维护。此方式没有返回值。