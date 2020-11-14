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

### JaveScript

#### 1.概念

一门客户端脚本语言,运行在客户端浏览器中的。每一个浏览器都有Javascript的解析引擎
脚本语言：不需要编译，直接就可以被浏览器解析执行了

#### 2.功能：

可以来增强用户和html页面的交互过程，可以来控制html元素，让页面有一些动态的效果，增强用户的体验。

#### 3.发展史

1.1992年，Nombase公司，开发出第一门客户端脚本语言，专门用于表单的校验。命名为：C--，后来更名为：scriptEase

2.1995年，Netscape（网景）公司，开发了一门客户端脚本语言：Livescript。后来，请来SUN公司的专家，修改Livescript，命名为Javascript

3.1996年，微软抄袭Javascript开发出Jscript语言

4.1997年，ECMA（欧洲计算机制造商协会），ECMAScript，就是所有客户端脚本语言的标准。

#### 4.ECMAScript

##### 1.与html的结合

###### 1.内部JS

```html
<script>
    alert("")
</script>
```

###### 2.外部JS

```html
<script src="js/xx.js"></script>
```

###### 3.注意

1.<script>可以定义在html页面的任何地方。但是定义的位置会影响执行顺序。
2.<script>可以定义多个。

##### 5.注释

```html
//注释
/*注释*/
```



##### 6.数据类型

###### 1.原始数据类型

1.number：数字。整数/小数/NaN（not a number一个不是数字的数字类型）

2.string：字符串。字符串“abc""a"'abc'

3.boolean:true和false

4.null：一个对象为空的占位符

5.undefined：未定义。如果一个变量没有给初始化值，则会被默认赋值为undefined

###### 2.引用

对象

##### 7.变量

变量：一块储存数据的内存空间

语法：var 变量名 = 初始化值

```js
//定义number类型
var num =1;
var num2 =1.2;
var num3=NaN;
//定义string类型
var str="abc";
var str2="edf";
//定义boolean类型
var flag= true;
//定义null，underfined类型
var obj= null;
var obj2=underfined;
var obj3;
```



##### 8.运算符

###### 1.一元运算符：只有一个运算数的运算符

++，--，+（正号）

###### 2.算数运算符

+-*/%...

###### 3.赋值运算符

= += -+....

###### 4.比较运算符

< > >= <= == ===

###### 5.逻辑运算符

&& || !

###### 6.三元运算符

？ ：

##### 9.流程控制语句  

if else

switch

while

do...while

for

##### 10.对象

```js
function 方法名称(参数){
    
}

var arr3 = new Array(5);
var arr1 = [1,2,3,4];
```

计算随机数（1-100）

```js
var num=Math.floor((math.random()*100)+1)
```

##### 11.正则表达式

###### 1.单个字符：[]

如：[a] [ab] [a-zA-Z0-9]

特殊符号代表特殊含义的单个字符：

\d:单个数字字符[0-9]

\w:单个单词字符[a-zA-Z0-9_]

###### 2.量词符号：

？：表示出现0次或1次

*：表示出现0次或多次

+：表示出现1次或多次

{m,n}:表示m<=数量 <=n

m如果缺省：{,n}最多n次

n如果缺省：{m,}最少m次

```js
var reg =new RegExp("正则表达式");
var reg =/正则表达式/

reg.test(xxxxx)
```

#### DOM简单学习

### Bootstrap框架

#### 1.概念

一个前端开发的框架，Bootstrap，来自Twitter，是目前很受欢迎的前端框架。Bootstrap是基于HTML、CSS、JavaScript的，它简洁灵活，使得Web 开发更加快捷。

 *框架：一个半成品软件，开发人员可以在框架基础上，在进行开发，简化编码

*好处：1.响应式布局 2.便捷的样式

#### 2.快速入门

1.下载bootstrap

2.引入bootstrap文件 

3.创建页面，引用样式 

基本页面

```html
<！DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="utf-8">
<meta http-equiv="X-UA-Compatible"content="IE=edge">
<meta name="viewport"content="width=device-width，initial-scale=1">
<！--上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！-->
<title>Bootstrap HelloWorld</title>
<！--Bootstrap-->
<link href="css/bootstrap.min.css"rel="stylesheet">
<！--jQuery（Bootstrap的所有JavaScript插件都依赖jQuery，所以必须放在前边）-->
<script src="js/jquery-3.2.1.min.js"></script>
<！--加载 Bootstrap的所有Javascript插件。你也可以根据需要只加载单个插件。->
<script src="js/bootstrap.min.js"></script>
</head>
<body>
<h1>你好，世界！</h1>
</body></html>
```

####  3.响应式布局

*同一套页面可以兼容不同分辨率的设备。

*实现：依赖于栅格系统：捋一行平均分成12个格子，可以指定元素占几个格子

*步骤：
1.定义容器。相当于之前的table、
*容器分类：
1.container：两边留白
2.container-fluid：每一种设备都是100%宽度

2.定义行。相当于之前的tr样式：row

3.定义元素。指定该元素在不同的设备上，所占的格子数目。样式：co1-设备代号-格子数目
*设备代号：
1.xs：超小屏幕手机（<768px）：co1-xs-12																																				2.sm：小屏幕平板（2768px）
3.md：中等屏幕桌面显示器（2992px）
4.1g：大屏幕大桌面显示器（21200px）

*注意：
1.一行中如果格子数目超过12，则超出部分自动换行

2.栅格类属性可以向上兼容。栅格类适用于与屏幕宽度大于或等于分界点大小的设备

3.如果真实设备宽度小于了设置栅格类属性的设备代码的最小值，会一个元素沾满一整行。

##### 1.全局css样式

```css
按钮：class="btn btn-default"
图片: class="img-responsive":图片再任意尺寸都占100%
图片形状:
<img src="..." alt="..." class="img-rounded">
<img src="..." alt="..." class="img-circle">
<img src="..." alt="..." class="img-thumbnail">
表格:talbe
table-bordered
table-hover
表单:class="form-sontrol"
```

