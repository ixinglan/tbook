# 线程同步

## 同步代码块
```java
    synchronized(同步监视器){
       //需要被同步的代码
    }
```
说明：  
1.操作共享数据的代码，即为需要被同步的代码。  -->不能包含代码多了，也不能包含代码少了。  
2.共享数据：多个线程共同操作的变量。比如：ticket就是共享数据。  
3.同步监视器，俗称：锁。任何一个类的对象，都可以充当锁。  
4.要求：多个线程必须要共用同一把锁。  
补充：在实现Runnable接口创建多线程的方式中，我们可以考虑使用this充当同步监视器。  

```java
public class SyncTest1 {
    public static void main(String[] args) {
        Window1 w = new Window1();

        Thread t1 = new Thread(w);
        Thread t2 = new Thread(w);
        Thread t3 = new Thread(w);

        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");

        t1.start();
        t2.start();
        t3.start();
    }

}
//处理实现Runnable接口的纯种安全
class Window1 implements Runnable{
    private int ticket = 100;
    @Override
    public void run() {
        while(true){
            synchronized (this){
                if (ticket > 0) {
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                    System.out.println(Thread.currentThread().getName() + ":卖票，票号为：" + ticket);

                    ticket--;
                } else {
                    break;
                }
            }
        }
    }
}
```

## 同步方法
1.同步方法仍然涉及到同步监视器，只是不需要我们显式的声明。  
2.非静态的同步方法，同步监视器是：this  
3.静态的同步方法，同步监视器是：当前类本身  
```java
public class SyncTest3 {
    public static void main(String[] args) {
        Window3 w = new Window3();

        Thread t1 = new Thread(w);
        Thread t2 = new Thread(w);
        Thread t3 = new Thread(w);

        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");

        t1.start();
        t2.start();
        t3.start();
    }
}
class Window3 implements Runnable {

    private int ticket = 100;

    @Override
    public void run() {
        while (true) {
            show();
        }
    }
    private synchronized void show(){//同步监视器：this
        //synchronized (this){

        if (ticket > 0) {

            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            System.out.println(Thread.currentThread().getName() + ":卖票，票号为：" + ticket);

            ticket--;
        }
        //}
    }
}
```

## Lock锁
参考juc 中的Lock锁

## 释放锁操作
* 当前 线程的同步方法、同步代码块 执行结束。
* 当前 线程在同步代码块、同步方法中遇到 break 、 return 终止了该代码块、该方法的继续执行。
* 当前 线程在同步代码块、同步方法中出现了未处理的 Error 或 Exception 导致异常结束。
* 当前 线程在同步代码块、同步方法中执行了线程对象的 wait() 方法，当前线程暂停，并释放锁。

## 线程死锁
* 死锁
    > 不同的线程分别占用对方需要的同步资源不放弃，都在等待对方放弃自己需要的同步资源，就形成了线程的死锁  
    > 出现死锁后，不会出现异常，不会出现提示，只是所有的线程都处于阻塞状态，无法继续  
* 解决方法
    > 专门的算法、原则  
    > 尽量减少同步资源的定义  
    > 尽量避免嵌套同步

```java
public class DeadLockTest {
    public static void main(String[] args) {
        StringBuffer s1 = new StringBuffer();
        StringBuffer s2 = new StringBuffer();

        new Thread() {
            @Override
            public void run() {
                synchronized (s1) {
                    s1.append("a");
                    s2.append("1");
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    synchronized (s2) {
                        s1.append("b");
                        s2.append("2");

                        System.out.println(s1);
                        System.out.println(s2);
                    }


                }

            }
        }.start();


        new Thread(new Runnable() {
            @Override
            public void run() {
                synchronized (s2) {
                    s1.append("c");
                    s2.append("3");
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    synchronized (s1) {
                        s1.append("d");
                        s2.append("4");

                        System.out.println(s1);
                        System.out.println(s2);
                    }
                }
            }
        }).start();
    }
}
```  