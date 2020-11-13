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

同步方法：就是把synchronized关键字加到方法上

格式：修饰符 synchronized返回值方法名（方法参数）{}

同步静态方法：就是把synchronized关键字加到静态方法上

格式：修饰符static synchronized返回值类型方法名（方法参数）{}

同步静态方法的锁对象是什么呢？
类名.class

##### Lock锁

虽然我们可以理解同步代码块和同步方法的锁对象问题，但是我们并没有直接看到在哪里加上了锁，在哪里释放了锁，为了更清晰的表达如何加锁和释放锁，JDK5以后提供了一个新的锁对象Lock 

Lock实现提供比使用synchronized方法和语句可以获得更广泛的锁定操作

Lock中提供了获得锁和释放锁的方法

```java
void lock0：获得锁
void unlock0：释放锁
```

Lock是接口不能直接实例化，这里采用它的实现类ReentrantLock来实例化ReentrantLock的构造方法

ReentrantLock0：创建一个ReentrantLock的实例

```java
public class SellTicket implements Runnable{
	private int tickets=100；
	private Lock lock=new ReentrantLock（）；
	@Override public void run（）{
		while（true）{
            try{
            lock.lock（）；
            if（tickets>e）{
		   try{
				Thread.sleep（millis：180）；
			}catch（InterruptedException e）{
			e.printStackTrace（）；
             System.out.println（Thread.current Thread（）.getName（）+"正在出售第"+tickets+“张票"）；tickets--；
                }finally{
			1ock.unlock（）；
                }
           }	
```

## java web

### 1.junit

#### 1.junit单元测试

测试分类：

1.黑盒测试：不需要写代码，给输入值，看程序是否能够输出期望的值。
2.白盒测试：需要写代码的。关注程序具体的执行流程。

Junit使用：白盒测词

 步骤

1.定义一个测试类（测试用例）

 建议：1.测试类名：被测试的类名Test CalculatorTest       包名：xxx.xxx.xx.test cn.itcast.test

2.定义测试方法：可以独立运行

建议：方法名：test测试的方法名testAdd（） 返回值：void 参数列表：空参

3.给方法加@Test

4.导入junit的依赖

5.判断结果

红色失败  绿色成功

6.使用断言处理结果

```java
Assert.assertEquals(3,result);
```

两个需要使用的注解

@Before 在所有的测试方法执行前先执行@After 在所有测试方法执行后再执行

### 2.反射

框架设计的灵魂

#### 1.概念

将类的各部分组成分装为其他对象，这就是反射机制

好处：1.可以在程序运行过程中，操作这些对象	2.可以解耦，提高程序的可拓展性

#### 2.获取Class对象的方式：

1.class.forName（"全类名”）：将字节码文件加载进内存，返回class对象

//多用于配置文件，将类名定义在配置文件中。读取文件，加载类。

2.类名.class：通过类名的属性class获取

//多用于参数传递

3.对象.getclass（）：getclass（）方法在object类中定义着。

//多用于对象获取字节码的方式

结论：

同一个字节码文件（*.class）在一次程序运行过程中，只会被加载一次，不论通过哪一种方式获取的Class对象都是同一个。

### 3.class对象功能

#### 1.获取功能

##### 1.获取成员变量们

```java
Field[]getFields（）
Field getField（string name）
Field[]getDeclaredFields（）
Field getDeclaredField（string name）

```

##### 2.获取构造方法们

```java
Constructor<？>[]getconstructors（）
Constructor<T>getconstructor（类<？>...parameterTypes）
Constructor<T>getpeclaredconstructor（类<？>...parameterTypes）
Constructor<？>[]getDeclaredconstructors（）

```



##### 3.获取成员方法

```java
Method[]getMethods（）
Method getMethod（string name，类<？>..…parameterTypes）
Method[]getpeclaredMethods（）
Method getDeclaredMethod（string name，类<？>...parameterTypes）

```



##### 4.获取类名

```java
string getName（）
```

