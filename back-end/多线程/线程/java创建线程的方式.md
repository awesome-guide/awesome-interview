## Thread

#### 一、Java创建线程的方式

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



2. 通过Runnable接口创建线程类

   - 实现Runnable接口，复写run方法，创建Thread类对象，将Runnable子类对象传递给Thread类对象。调用start方法开启线程。此方法2较之方法1好，将线程对象和线程任务对象分离开。降低了耦合性，利于维护。此方式没有返回值。

   - 定义runnable接口的实现类，并重写该接口的run()方法，该run()方法的方法体同样是该线程的线程执行体。

   - 创建 Runnable实现类的实例，并依此实例作为Thread的target来创建Thread对象，该Thread对象才是真正的线程对象。

   - 调用线程对象的start()方法来启动该线程。

```java
public class MyRunnable implements Runnable {
    @Override
    public void run() {
        // 执行业务逻辑
    }
    
    public static void main(String[] args) {
        MyRunnable myRunnable = new MyRunnable();
        Thread thread = new Thread(myRunnable);
        thread.start();
    }
}
```

3. 使用Callable接口和FutureTask类实现创建有返回结果的线程

   FutureTask 的出现是为了弥补 Thread 的不足而设计的,可以让[程序员](http://www.liuhaihua.cn/archives/tag/程序员)跟踪、获取任务的执行情况、计算结果

   - 创建Callable接口的实现类，并实现call()方法，该call()方法将作为线程执行体，并且有返回值。
   - 创建Callable实现类的实例，使用FutureTask类来包装Callable对象，该FutureTask对象封装了该Callable对- 象的call()方法的返回值。
   -  使用FutureTask对象作为Thread对象的target创建并启动新线程。
   - 调用FutureTask对象的get()方法来获得子线程执行结束后的返回值

   ```java
   public class MyCallable implements Callable<Integer>{
       @Override
       public Integer call() throws Exception {
           System.out.println("子线程开始计算");
           Thread.sleep(2000);
           System.out.println("子线程结束计算");
           return 100 * 100;
       }
   
       public static void main(String[] args) throws ExecutionException, InterruptedException {
           // 实例化Callable对象
           MyCallable myCallable = new MyCallable();
           // 使用FutureTask包装
           FutureTask<Integer> futureTask = new FutureTask<>(myCallable);
           // 创建一个线程执行任务
           Thread thread = new Thread(futureTask);
           thread.start();
           Thread.sleep(1000);
           System.out.println("主线程执行");
           // 使用get()方法会阻塞主线程，直到子线程执行完成返回结果
           int result = futureTask.get();
           System.out.println("子线程计算结果: " + result);
       }
   
   }
   ```

   4.使用线程池创建线程

   当不是执行一次性任务的时候，如果不使用线程池，那么就要频繁的创建和销毁线程，这是一个比较消耗资源的操作，使用线程池可以灵活的调整线程资源的占用，防止消耗过多的内存

   - 创建线程池（可以利用JDK的Executors，也可自己实现）
   - 创建Callable 或 Runnable任务，提交到线程池
   - 通过返回的 `Future#get` 获取返回的结果

   在Java中已经提供了ExecutorSerice、Executors等工具类为我们快速的创建线程池	

   ```java
   			ExecutorService pool = Executors.newFixedThreadPool(5);
   
           Future<String> future = pool.submit(new Callable<String>(){
               @Override
               public String call() throws Exception {
                   String result = "第4种方式:new Thread 4";
                   return result;
               }
           });
   
           pool.shutdown();
           System.out.println(future.get());
   ```

   

   ###### 二.创建线程的三种方式的对比

   ###### 1.实现Runnable/Callable接口相比继承Thread类的优势

   （1）适合多个线程进行资源共享
   （2）可以避免java中单继承的限制
   （3）增加程序的健壮性，代码和数据独立
   （4）线程池只能放入Runable或Callable接口实现类，不能直接放入继承Thread的类

   ###### 2.Callable和Runnable的区别

   (1) Callable重写的是call()方法，Runnable重写的方法是run()方法
   (2) call()方法执行后可以有返回值，run()方法没有返回值
   (3) call()方法可以抛出异常，run()方法不可以
   (4) 运行Callable任务可以拿到一个Future对象，表示异步计算的结果 。通过Future对象可以了解任务执行情况，可取消任务的执行，还可获取执行结果




