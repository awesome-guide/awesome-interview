## 多线程交替打印ABC10次的多种实现方法(请给出至少四种实现思路)（应该有5种）



#### 方法一：使用synchronized，wait\notify方式实现

##### 1：

~~~java
package com.unisound.thread.mulaltthread.Thread_swn;

/**
 * @ClassName LetterPrinter
 * @Description: TODO
 * @Author raox
 * @Date 2020/12/28 10:29
 * @Version V1.0
 **/
public class LetterPrinter {
    private char letter='A';

    void Print(){
        System.out.print(letter);
        if ('C'==letter) {
            System.out.println();
        }
    }

    void nextLetter() {
        switch (letter) {
            case 'A':
                letter='B';
                break;

            case 'B':
                letter='C';
                break;

            case 'C':
                letter='A';
                break;

            default:
                break;
        }
    }

    /**
     * @return the letter
     */
    public char getLetter() {
        return letter;
    }

}
~~~

~~~java
package com.unisound.thread.mulaltthread.Thread_swn;

/**
 * @ClassName PrintRunable
 * @Description: TODO
 * @Author raox
 * @Date 2020/12/28 10:28
 * @Version V1.0
 **/
public class PrintRunable implements Runnable{

    private LetterPrinter letterPrinter=null;

    private char letter=' ';

    public PrintRunable(LetterPrinter letterPrinter,char letter) {
        super();
        this.letterPrinter=letterPrinter;
        this.letter=letter;
    }

    public void run() {
        // TODO Auto-generated method stub
        for (int i = 0; i < 10; i++) {
            synchronized (letterPrinter) {
                while (letter!=letterPrinter.getLetter()) {
                    try {
                        letterPrinter.wait();//告知被调用的线程放弃管程进入休眠直到其他线程进入相同的管程并且调用notify()/notifyAll()
                    } catch (InterruptedException e) {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                    }
                }

                letterPrinter.Print();
                letterPrinter.nextLetter();
                letterPrinter.notifyAll();//恢复相同对象中第一个调用wait()的线程

            }
        }
    }

}
~~~

~~~
package com.unisound.thread.mulaltthread.Thread_swn;

/**
 * @ClassName PrintThreadExample
 * @Description: TODO
 * @Author raox
 * @Date 2020/12/28 10:30
 * @Version V1.0
 **/
public class PrintThreadExample {

    public static void main(String[] args) throws InterruptedException {

        LetterPrinter letterPrinter=new LetterPrinter();
        new Thread(new PrintRunable(letterPrinter, 'A')).start();
        Thread.sleep(100);
        new Thread(new PrintRunable(letterPrinter, 'B')).start();
        Thread.sleep(100);
        new Thread(new PrintRunable(letterPrinter, 'C')).start();

    }

}
~~~

##### 2:

~~~java
package com.unisound.thread.mulaltthread;

/**
 * @ClassName ABC_Synch
 * @Description: TODO
 * @Author raox
 * @Date 2020/12/26 17:35
 * @Version V1.0
 **/
public class ABC_Synch {
    private static int count = 30;

    static class ThreadA extends Thread {
        Thread c;

        public void setC(Thread c) {
            this.c = c;
        }

        @Override
        public void run() {
            while (count-- > 0) {
                synchronized (c) {
                    synchronized (this) {
                        System.out.print("A");
                        this.notify();
                    }

                    try {
                        c.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                }
            }
        }
    }

    static class ThreadB extends Thread {
        Thread a;

        public void setA(Thread a) {
            this.a = a;
        }

        @Override
        public void run() {
            while (count-- > 0) {
                synchronized (a) {
                    synchronized (this) {
                        System.out.print("B");
                        this.notify();
                    }

                    try {
                        a.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                }
            }
        }
    }

    static class ThreadC extends Thread {
        Thread b;

        public void setB(Thread b) {
            this.b = b;
        }

        @Override
        public void run() {
            while (count-- > 0) {

                synchronized (b) {
                    synchronized (this) {
                        System.out.print("C");
                        this.notify();
                    }

                    try {
                        b.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                }
            }
        }
    }

    public static void main(String[] args) throws InterruptedException {
        ThreadA a = new ThreadA();
        ThreadB b = new ThreadB();
        ThreadC c = new ThreadC();

        a.setC(c);
        b.setA(a);
        c.setB(b);


        b.start();
        Thread.sleep(100);
        a.start();
        Thread.sleep(100);
        c.start();
    }

}
~~~

3:

~~~java
package com.unisound.thread.mulaltthread;

/**
 * @ClassName Thread1
 * @Description: TODO
 * @Author raox
 * @Date 2020/12/24 9:55
 * @Version V1.0
 **/
public class ThreadA {

    public static class ThreadPrinter implements Runnable {
        private String name;
        private Object prev;
        private Object self;

        private ThreadPrinter(String name, Object prev, Object self) {
            this.name = name;
            this.prev = prev;
            this.self = self;
        }

        public void run() {
            int count = 10;
            while (count > 0) {
                synchronized (prev) {
//                    System.out.println(Thread.currentThread().getName());
                    synchronized (self) {
//                        System.out.println(prev + "-" + name + "-" + Thread.currentThread().getName());
                        System.out.print( name);
                        count--;
                        self.notifyAll();
                    }
                    try {
                        prev.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }
    public static void main(String[] args) throws Exception {
        Object a = new Object();
        Object b = new Object();
        Object c = new Object();
        ThreadPrinter pa = new ThreadPrinter("A", c, a);
        ThreadPrinter pb = new ThreadPrinter("B", a, b);
        ThreadPrinter pc = new ThreadPrinter("C", b, c);

        new Thread(pb,"T-B").start();
        Thread.sleep(100);


        new Thread(pa,"T-A").start();
        Thread.sleep(100);//保证初始ABC的启动顺序

        new Thread(pc,"T-C").start();


    }
}

~~~

##### 3:

~~~java
package com.unisound.thread.mulaltthread;

public class ThreadB extends Thread{

    private static int currentCount=0;

    public ThreadB(String name) {
        this.setName(name);
    }

    @Override
    public void run() {
        while (currentCount<30) {

            switch (currentCount%3) {
                case 0:
                    if ("A".equals(getName())) {
                        printAndIncrease();
                    }
                    break;

                case 1:
                    if ("B".equals(getName())) {
                        printAndIncrease();
                    }
                    break;

                case 2:
                    if ("C".equals(getName())) {
                        printAndIncrease();
                    }
                    break;

                default:
                    break;
            }
        }
    }

    private void printAndIncrease() {
        // TODO Auto-generated method stub
        print();
        increase();
    }

    private void increase() {
        currentCount++;
    }

    private void print() {
        System.out.print(getName());
        if ("C".equals(getName())) {
            System.out.println();
        }
    }

    public static void main(String[] args) {
        new ThreadB("A").start();
        new ThreadB("B").start();
        new ThreadB("C").start();
    }

}
~~~

##### 4:

~~~java
package com.unisound.thread.mulaltthread;

/**
 * @ClassName ThreadJ
 * @Description: TODO
 * @Author raox
 * @Date 2020/12/28 15:20
 * @Version V1.0
 **/

import java.util.concurrent.atomic.AtomicInteger;

public class ThreadJ {

    public static void main(String[] args) {
        new ABCPrint("A",1).start();//A打印1次
        new ABCPrint("B",1).start();//B打印1次
        new ABCPrint("C",1).start();//C打印1次
    }

}

class ABCPrint extends Thread {

    //打印次数
    private int count;

    private final String str[] = { "B", "C", "A" };
    private final static AtomicInteger atomCount= new AtomicInteger();

    public ABCPrint(String name,int count) {
        this.setName(name);
        this.count = count;
    }

    @Override
    public void run() {
        while (true) {
            // 循环满2轮退出打印
            if (atomCount.get() / 3 == 10) {
                break;
            }
            synchronized (atomCount) {
                // 顺序打印A、B、C
                if (str[atomCount.get() % 3].equals(getName())) {
                    atomCount.getAndIncrement();//自增

                    //对应打印几次
                    for (int i = 0; i < count; i++) {
                        System.out.print(getName());
                    }

                    //表示一轮打印结束 方便观察打印下分隔符
                    if ("A".equals(getName())) {
                        System.out.println(";");
                    }
                    // 当前线程打印打印完成后唤醒其它线程
                    atomCount.notifyAll();
                } else {
                    // 非顺序线程wait()
                    try {
                        atomCount.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }

}
~~~



#### 方法二：使用ReentrantLock方式实现

##### 1：

~~~java
package com.unisound.thread.mulaltthread;

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

/**
 * @ClassName ThreadD
 * @Description:
 * @Author raox
 * @Date 2020/12/28 13:21
 * @Version V1.0
 **/
public class ThreadD {

    private static int state=0;

    public static void main(String[] args) {
        final Lock lock=new ReentrantLock();

        Thread A=new Thread(new Runnable() {
            public void run() {
                while (state<30) {
                    lock.lock();
                    if (state%3==0) {
                        System.out.print("A");
                        state++;
                    }
                    lock.unlock();
                }
            }
        });

        Thread B=new Thread(new Runnable() {

            public void run() {
                // TODO Auto-generated method stub
                while (state<30) {
                    lock.lock();//get lock
                    if (state%3==1 ) {
                        System.out.print("B");
                        state++;
                    }
                    lock.unlock();//release lock
                }
            }
        });

        Thread C=new Thread(new Runnable() {

            public void run() {
                // TODO Auto-generated method stub
                while (state<=30 ) {
                    lock.lock();//get lock
                    if (state%3==2 ) {
                        System.out.print("C");
                        state++;
                    }
                    lock.unlock();//release lock
                }
            }
        });

        A.start();
        B.start();
        C.start();
    }

}
~~~

##### 2：

~~~java
package com.unisound.thread.mulaltthread;

/**
 * @ClassName ThreadE
 * @Description: TODO
 * @Author raox
 * @Date 2020/12/28 14:21
 * @Version V1.0
 **/

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class ThreadE {
    private static Lock lock = new ReentrantLock();// 通过JDK5中的Lock锁来保证线程的访问的互斥
    private static int state = 0;//通过state的值来确定是否打印

    static class ThreadA extends Thread {
        @Override
        public void run() {
            for (int i = 0; i < 10; ) {
                lock.lock();
                try {

                    while (state % 3 == 0) {// 多线程并发，不能用if，必须用循环测试等待条件，避免虚假唤醒
                        System.out.print("A");
                        state++;
                        i++;
                    }
                } finally {
                    lock.unlock();// unlock()操作必须放在finally块中
                }
            }
        }
    }

    static class ThreadB extends Thread {
        @Override
        public void run() {
            for (int i = 0; i < 10; ) {
                lock.lock();
                try {
                    while (state % 3 == 1) {// 多线程并发，不能用if，必须用循环测试等待条件，避免虚假唤醒
                        System.out.print("B");
                        state++;
                        i++;
                    }
                } finally {
                    lock.unlock();// unlock()操作必须放在finally块中
                }
            }
        }
    }

    static class ThreadC extends Thread {
        @Override
        public void run() {
            for (int i = 0; i < 10; ) {
                lock.lock();
                try {

                    while (state % 3 == 2) {// 多线程并发，不能用if，必须用循环测试等待条件，避免虚假唤醒
                        System.out.print("C");
                        state++;
                        i++;
                    }
                } finally {
                    lock.unlock();// unlock()操作必须放在finally块中
                }
            }
        }
    }

    public static void main(String[] args) {
        new ThreadA().start();
        new ThreadB().start();
        new ThreadC().start();
    }
}
~~~

##### 3:

~~~java
package com.unisound.thread.mulaltthread;

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

/**
 * @ClassName ThreadH
 * @Description: TODO
 * @Author raox
 * @Date 2020/12/28 15:15
 * @Version V1.0
 **/
public class ThreadH {
    private static Lock lock = new ReentrantLock();
    private static int state = 0;

    static class MyThread extends Thread{
        int num;
        String letter;

        public MyThread(String letter, int num) {
            this.num = num;
            this.letter = letter;
        }

        public void run() {
            for (int i = 0; i < 10; ) {
                try {
                    lock.lock();
                    while (state % 3 == num){
                        System.out.print(letter);
                        state++;
                        i++;//变量自增必须写在这
                    }
                }finally {
                    lock.unlock();
                }

            }
        }
    }

    public static void main(String[] args) {
        new MyThread("A",0).start();
        new MyThread("B",1).start();
        new MyThread("C",2).start();
    }
}

~~~



#### 方法三:使用Condition方式实现

~~~java
package com.unisound.thread.mulaltthread;

import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

/**
 * @ClassName ThreadF
 * @Description: ReentrantLock结合Condition
 * @Author raox
 * @Date 2020/12/28 14:47
 * @Version V1.0
 **/
public class ThreadF {
    private static Lock lock = new ReentrantLock();
    private static Condition A = lock.newCondition();
    private static Condition B = lock.newCondition();
    private static Condition C = lock.newCondition();

    private static int count = 0;

    static class ThreadA extends Thread {
        @Override
        public void run() {
            try {
                lock.lock();
                for (int i = 0; i < 10; i++) {
                    while (count % 3 != 0) {
                        A.await(); // A释放lock锁
                    }
                    System.out.print("A");
                    count++;
                    B.signal(); // A执行完唤醒B线程
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            } finally {
                lock.unlock();
            }
        }
    }

    static class ThreadB extends Thread {
        @Override
        public void run() {
            try {
                lock.lock();
                for (int i = 0; i < 10; i++) {
                    while (count % 3 != 1)
                        B.await();// B释放lock锁，当前面A线程执行后会通过B.signal()唤醒该线程
                    System.out.print("B");
                    count++;
                    C.signal();// B执行完唤醒C线程
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            } finally {
                lock.unlock();
            }
        }
    }

    static class ThreadC extends Thread {
        @Override
        public void run() {
            try {
                lock.lock();
                for (int i = 0; i < 10; i++) {
                    while (count % 3 != 2)
                        C.await();// C释放lock锁
                    System.out.print("C");
                    count++;
                    A.signal();// C执行完唤醒A线程
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            } finally {
                lock.unlock();
            }
        }
    }

    public static void main(String[] args) throws InterruptedException {
        new ThreadA().start();
        new ThreadB().start();
        new ThreadC().start();
    }
}
~~~

#### 方法四：使用Semaphore信号量方式实现

~~~java
package com.unisound.thread.mulaltthread;

/**
 * @ClassName ThreadG
 * @Description: Semaphore信号量方式
 * @Author raox
 * @Date 2020/12/28 15:05
 * @Version V1.0
 **/

import java.util.concurrent.Semaphore;

public class ThreadG {
    // 以A开始的信号量,初始信号量数量为1
    private static Semaphore A = new Semaphore(1);
    // B、C信号量,A完成后开始,初始信号数量为0
    private static Semaphore B = new Semaphore(0);
    private static Semaphore C = new Semaphore(0);

    static class ThreadA extends Thread {
        @Override
        public void run() {
            try {
                for (int i = 0; i < 10; i++) {
                    A.acquire();// A获取信号执行,A信号量减1,当A为0时将无法继续获得该信号量
                    System.out.print("A");
                    B.release();// B释放信号，B信号量加1（初始为0），此时可以获取B信号量
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    static class ThreadB extends Thread {
        @Override
        public void run() {
            try {
                for (int i = 0; i < 10; i++) {
                    B.acquire();
                    System.out.print("B");
                    C.release();
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    static class ThreadC extends Thread {
        @Override
        public void run() {
            try {
                for (int i = 0; i < 10; i++) {
                    C.acquire();
                    System.out.println("C");
                    A.release();
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String[] args) throws InterruptedException {
        new ThreadA().start();
        new ThreadB().start();
        new ThreadC().start();
    }
}

~~~

