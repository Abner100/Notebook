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

###### 1.单个字符：

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

#### DOM

功能：控制html文档的内容

获取页面标签（元素）对象：Element

document.getElementById（“id值"）：通过元素的id获取元素对象

操作Element对象：

1.修改属性值：
1.明确获取的对象是哪一个？
2.查看API文档，找其中有哪些属性可以设置
2.修改标签体内容：

属性：innerHTML
1.获取元素对象
2.使用innerHTML属性修改标签体内容

##### 1.概念

Document object Model文档对象模型

捋标记语言文档的各个组成部分，封装为对象。可以使用这些对象，对标记语言文档进行CRUD的动态操作

##### 2.W3C DOM标准被分为3个不同的部分

###### 1.核心DoM-针对任何结构化文档的标准模型

*Document：文档对象

1.创建：在html dom模型中可以使用window获取

2.方法

(1)获取Element对象：

1.getElementById（）：根据id属性值获取元素对象。id属性值一般唯一2.getEIementsBy 

```js
var id=document.getElementsByid（"yoyo"）；
alert（id）；
```

2.getElementTagName（）：根据元素名称获取元素对象们。返回值是一个数组

```js
//2.根据元素名称获取元素对象们。返回值是一个数组
var divs=document.getElementsByTagName（"div"）；
alert（divs.length）；
```



3.getElementsByClassName（）：根据class属性值获取元素对象们。返回值是一个数组

```js
//3.根据C1ass属性值获取元素对象们。返回值是一个数组
var divcls=document.getElementsByClassName（"c1s1"）；
alert（div_cls.length）；
```



4.getElementsByName（）：根据name属性值获取元素对象们。返回值是一个数组组

```js
//4.根据name属性值获取元素对象们。返回值是一个数组
var ele_username=document.getElementsByName（"username"）；
alert（efe_username.1ength）
```



(2)创建其他DOM对象

createAttribute（name）
createComment（）createElement（）
createTextNode（）

*Element：元素对象
*Attribute：属性对象
*Text：文本对象
*Commeft：注释对象

Element:元素属性

1.获取/创建：通过document来获取和创建

2.方法：

1.removeAttribute():删除属性

2.setAttribute():设置属性

```js
//获取btn
var btn_set=document.getElementById("btn_set");
btn_set.onclick=function(){
    var element_a=document.getElementsByTagName("a")[0];
    element_a.setAttribute("href","http://www.baidu.com");
}

//删除btn
var btn_remove=document.getElementById("btn_remove");
btn_set.onclick=function(){
    var element_a=document.getElementsByTagName("a")[0];
    element_a.removeAttribute("href","http://www.baidu.com");
}

```

Node节点对象，其他5个的父节点

特点：所有Dom对象都可以被认为是一个节点

CRUD dom树：

appendchild（）：向节点的子节点列表的结尾添加新的子节点。
removeChild（）：删除（并返回）当前节点的指定子节点。
replacechild（）：用新节点替换一个子节点。

```html
<body>
    <div id="div1">
        <div id="div2">
            div2
        </div>
    </div>
    <a href="javascript:void(0)" id="del">删除子节点</a>
    <a href="javascript:void(0)" id="sff">添加</a>
    <script>
        var element_a= document.getElementById("del");
        element_a.onclick=function(){
            var div1 =document.getElementById("div1");
            var div2=document.getmentById("div2");
            div1.removeChile(div2);
        }
        
      var element_a= document.getElementById("add");
        element_a.onclick=function(){
            var div1 =document.getElementById("div1");
            var div3 = document.creatAttribute("id","div3");
            div1.appendChile(div2);
        }  
    </script>
</body>
```

属性：parentNode  返回节点的父节点

###### 2.XML DOM-针对XML文档的标准模型

###### 3.HTML DOM-针对HTML文档的标准模型

#### BOM

##### 1.概念：Browser Object Model浏览器对象模型

捋浏览器的各个组成部分封装成对象。

##### 2.组成：

###### 1.Window：窗囗对象

1.创建

2.方法

1.与弹出框有关的方法

alert（）显示带有一段消息和一个确认按钮的警告框。

```js
alert("耗子尾汁");
window.alert("耗子尾汁");
```



confirm（）显示带有一段消息以及确认按钮和取消按钮的对话框。
*如果用户点击确定按钮，则方法返回true
*如果用户点击取消按钮，则方法返回false

```js
var flag=confirm("确定要退出吗");
if(flag){
    alert("bye");
}else{
    alert("em")
}
```



prompt（）显示可提示用户输入的对话框。

返回值为用户输入的值

```js
var result=prompt("输入名字")
```

2.与打开关闭有关的方法

close()关闭当前窗口

```js
close();
//关闭指定窗口
newwindow.close();
```

open()打开新窗口

返回值为新窗口对象

```js
var newwindow=open("www.baidu.com")
```

3.与定时器有关的方法

setTimeout()

在指定的毫米数后调用函数或者计算表达式

参数：1.js代码或者方法  2.毫秒值

返回值：唯一标识，用于取消定时器

```js
var id=setTimeout("alert("boom~")",2000);
两秒后执行
```

clearTimeout()

取消由setTimeout（）方法设置的timeout。

```js
clearTimeout(id);
```

setInterval（）

按照指定的周期（以毫秒计）来调用函数或计算表达式。

```js
var id =setInterval("alert("boom~")",2000);
每两秒执行一次
```

参数：1.js代码或者方法  2.毫秒值

返回值：唯一标识，用于取消定时器

clearInterval（）

取消由setInterval（）设置的timeout。

```js
clearInterval(id);
```

3.属性    

4.特点

Navigator：浏览器对象

screen：显示器屏幕对象

History：历史记录对象

###### 2.Location：地址栏对象

1.创建

方法：

roload()

刷新页面

```js
location.reload();
```

href

设置或获取href

```js
var hred =location.href;
//用于跳转
location.href="www.baidu.com"
```

#### 4.HTMLDOM

innerHTML

##### 1.标签体的设置和获取：innerHTM

```js
var div =document.getElementById("div1");
var innerHTML =div.innerHTML;
alert(innerHTML);
```

##### 2.使用html元素对象的属性

##### 3.控制样式

```js
//使用元素的style属性
var div1 =document.getElementById("div1");
div1.onclick=function(){
    div1.style.border="1px solid red";
}
```

```html
//提前定义好类元素选择器的样式，通过元素的className属性来设置其class

<style>
    .d2{
        border:1 px solid blue;
        width:20px;
        height:200px;
    }
</style>
var div2 = document.getElementId("div2");
div2.onclick=function(){
div2.className="d2";
}
```

#### 事件监听机制

##### 1.概念

*事件：某些操作。如：单击，双击，键盘按下了，鼠标移动了
*事件源：组件。如：按钮文本输入框...
监听器：代码。
*注册监听：捋事件，事件源，监听器结合在一起。当事件源上发生了某个事件，则触发执行某个监听器代码。

##### 2.常见的事件

###### 1.点击事件

1.onclick：单击事件
2.ondblclick：双击事件

###### 2.焦点事件

1.onblur：失去焦点2.onfocus：元素获得焦点。
3.加载事件：
1.onload：一张页面或一幅图像完成加载。

###### 4.鼠标事件

1.onmousedown 鼠标按钮被按下。
2.onmouseup鼠标按键被松开。
3.onmousemove 鼠标被移动。
4.onmouseover 鼠标移到某元素之上。
5.onmouseout鼠标从某元素移开。

###### 5.键盘事件

1.onkeydown某键盘按键被按下。
2.onkeyup某个键盘按键被松开。
3.onkeypress某个键盘按键被按下并松开。

###### 6.选择和改变

1.onchange域的内容被改变。
2.onselect文本被选中。

###### 7.表单事件

1.onsubmit 确认按钮被点击。
.onreset 重置按钮被点击。

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

