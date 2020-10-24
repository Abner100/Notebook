# 

## 网络编程

### InterAdderss类   的使用

String InetAddress getByName(String host)                 //确定主机名称的IP地址                          										String getHostName()                                                      //获取IP地址中的主机名                                                    			String getHostAdress()									 //返回文本显示的IP地址字符串

 

## 实现多线程

##### 进程

正在进行的应用程序

##### 线程

一个进程中的单个顺序控制流。是一个执行路径

单线程：一个进程中只有一条执行路径

多线程：一个进程中有 多个执行路径

##### 多线程实现方式1

继承Thread类，定义一个类MyThread继承Thread类，创建MyThread类中重新写run()方法，启动线程

##### 设置和获取多线程名称

设置名称：setName()

获取名称：getName() 

```java
//方法
public  class MyThreadDemo{
    public static void main(String[] args){
        MyThread my1 =new MyThread();
        MyThread my2 =new MyThread();
        
        my1.setName("风机");
        my2.setName("大炮");
        
        my1.start();
        my2.start();
    }
}
```

  

```java
//方法2
public  class MyThreadDemo{
    public static void main(String[] args){
        MyThread my1 =new MyThread("风机");
        MyThread my2 =new MyThread("大炮");
            
        my1.start();
        my2.start();
    }
}
```

#####  线程调度

分时调度模型：所有线程轮流使用cpu的使用权，平均分配使用权

抢占式调度模型：优先让优先级高的线程使用CPU，如果线程的优先级相同，那么就会随机选择，优先级高的线程获取的时间更长

JAVA使用的是抢占式调度模型

```java
public final int getPriority()  //返回此线程的优先级
public final void setPriority(int newPriority) 
```

优先级的范围是：1~10  默认值是5

```java
//设置优先级
my1.setPriority(5);
```

##### 线程控制

```java
static void sleep(long millis)
//使用当前正在执行的线程停留指定的毫秒数
void join() 
//等待这个线程死亡
void setDaemon(boolean on)
//标记此线程为守护线程，当运行的线程都是守护线程的时候，java虚拟机退出     
```

#####   线程的生命周期

![image-20201023184940523](image/image-20201023184940523.png)

##### 实现Runable接口的方式(多线程的实现方式2)

1.定义一个类MyRunable实现Runable接口

2.在MyRunable类中重写run()方法

3.创建MyRunbale类的对象

4.创建Thread类的对象，把MyRunable对象作为构造方法的参数

5.启动线程

```java
public static void main(String[] args){
    MyRunable my =new  MyRunable();
    
    Thread t1 = new Thread(my,"高铁");
    Thread t2 = new Thread(my,"飞机");
    
    t1.start();
    t2.start();
}
```

Runable接口的好处：

1.避免了java单继承的局限性

2.适合多个相同程序的代码去处理同一个资源的情况，把线程和程序的代码、数据有效的分离，较好的体现了面向对象的设计思想

##### 线程同步

###### 卖票

共有100张票，三个窗口同步卖票

```java
SellTicket.java

public class SellTickets implements Runnable {
    private int tickets = 100;

    @Override
    public void run() {
        while (true) {
            if (tickets > 0) {
                System.out.println(Thread.currentThread().getName() + "正在出售" + tickets + "张票");
                tickets--;
            }
        }
    }
}

```

```java
SellTicketsDemo.java

public class SellTicketsDemo {
    public static void main(String[] args) {
        SellTickets st = new SellTickets();
        Thread t1 = new Thread(st, "窗口1");
        Thread t2 = new Thread(st, "窗口2");
        Thread t3 = new Thread(st, "窗口3");

        t1.start();
        t2.start();
        t3.start();
    }
}

```

###### 同步代码块

格式

```java
synchronized(任意对象){
	多条语句操作共享数据的代码
}
```

用法

修改上方的线程代码例子

```java
public class SellTickets implements Runnable {
    private int tickets = 100;
    private Object object = new Object();

    @Override
    public void run() {
        while (true) {
            synchronized (object) {
                if (tickets > 0) {
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName() + "正在出售" + tickets + "张票");
                    tickets--;
                }
            }
        }
    }
}

```

###### 同步方法