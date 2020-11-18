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

### 数据库

#### 1.数据库基本概念

##### 1.什么是数据库

用于存储和管理数据的仓库

##### 2.数据库的特点

1.持久化存储数据的。其实数据库就是一个文件系统
2.方便存储和管理数据
3.使用了统一的方式操作数据库--SQL

##### 3.常见的数据库软件

 MYSQL     SQLSERVER

#### 2.什么的SQL

sql是一种操作所有关系型数据库的规则，每种数据库操作方式存在不一样的地方称为方言

#### 3.SQL通用语法

1.SQL语句可以单行或多行书写

2.可以使用空格和缩进来增强语句的可读性。

3.MYSQL的数据库SQL语句不区分大小写，关机字建议大写

4.三种注释

*单行注释：--注释内容或 # 注释内容(mysql特有) *多行注释：/*注释*/

#### 4.SQL分类

##### 1）DDL（Data Definition Language）数据定义语言用来定数据库对象：数据库，表，列等。关键字：create，drop，alter等

操作数据库(CRUD)

###### 1.C(Create):创建

创建数据库：

```sql
CREATE DATABASE 数据库名称；
```

创建数据库，判断不存再创建：

```SQL
CREATE DATABASE IF NOT EXISTS 数据库名称；
```

创建数据库，判断是否存在，并指定字符集：

```SQL
CREATE DATABASE 数据库名称 CHARACTER SET 字符集名；
```

```sql
CREATE DATABASE IF NOT EXISTS db4 set CHARACTER SET UTF-8;
```

###### 2.R(Retrieve)：查询

查询所有数据库的名称：

```SQL
SHOW DATABASES;
```

查询某个数据库的字符集和创建语句：

```SQL
SHOW CREATE DATABASE 数据库名称；
```

###### 3.U(Update)：修改

修改数据库的字符集：

```sql
ALTER DATABASE 数据库名称 CHARACTER SET 字符集名称；
```

###### 4.D(Delete)：删除 

修改数据库的字符集：

```SQL
DROP DATABASE 数据库名称；
```

判断数据库存在，存在则删除：

```SQL
DROP DATABASE IF EXISTS 数据库名称;
```

对表(CRUD)

###### 1.C(Create):创建

```sql
CREATE TABLE 表名(
    列名1 数据类型1，
    列名2 数据类型2，
    列名3 数据类型3，
    列名4 数据类型4
);
```

1.int：整数类型
*age int，

2.double：小数类型
*score double（5，2）

3.date：日期，只包含年月日，yyyy-MM-dd

4.datetime：日期，包含年月日时分秒 yyyy-MM-dd HH:mm:ss

5.timestamp：时间错类型包含年月日时分秒 yyyy-MM-dd HH:mm:SS
*如果捋来不给这个字段赋值，或赋值为nul1，则默认使用当前的系统时间，来自动赋值

6.varchar：字符串
*name varchar（20）：姓名最大20个字符
*zhangsan 8个字符张三2个字符

```sql
CREATE TABLE student(
    id int,
    name varchar(32),
    age int,
    score double(4,1),
    birthday date,
    insert_time timestamp
);
```

复制表

```sql
CREATE TABLE 表名 LIKE 被复制的表名 
```

###### 2.R(Retrieve)：查询

查询某个数据库中的所有表名称

```sql
SHOW TABLES;
```

查询表结构

```SQL
DESC 表名
```

###### 3.U(Update)：修改

1.修改表名

```sql
ALTER TABLE 表名 RENAME TO 新的表名；
```

2.修改表的字符集

```SQL
ALTER TABLE 表名 CHARACTER SET 字符集名称；
```

3.添加一列

```SQL
ALTER TABLE 表名 ADD 列名数据类型；
```

4.修改列名称类型

```
ALTER TABLE 表名 CHANGE 列名新列别新数据类型；
ALTER TABLE 表名 MODIFY 列名新数据类型；
```

5.删除列

```
ALTER TABLE 表名 DROP 列名；
```

###### 4.D(Delete)：删除 

```SQL
DROP TABLE 表名;
DROP TABLE IF EXISTS 表名;
```

###### 

##### 2）DML（Data Manipulation Language）数据操作语言用来对数据库中表的数据进行增删改。关键字：insert，delete，update等

###### 1.添加数据

```SQL
INSERT INTO 表名(列名1,列名2.....列名n) VALUES(值1,值2...值n)
```

1.列名和值要一一对应。
2.如果表名后，不定义列名，则默认给所有列添加值insert into 表名 values（值1，值2，...值n）；
3.除了==数字类型==，其他类型需要使用==引号==（单双都可以）引起来

###### 2.删除数据

```SQL
DELETE FROM 表 WHERE 条件;
```

1.如果不加条件，则删除表中所有记录。
2.如果要删除所有记录
1.delete from表名；--不推荐使用。有多少条记录就会执行多少次删除操作
2.TRUNCATE TABLE 表名；--推荐使用，效率更高先删除表，然后再创建一张一样的表。

###### 3.修改数据

```SQL
UPDATE 表名 SET 列名1=值1，列名2=值2，列名3=值3 WHERE 条件;
```

如果不添加任何条件则会修改所有的数据

##### 3）DQL（Data Query Language）数据查询语言用来查询数据库中表的记录（数据）。关键字：select，where等

###### 	1.查询语法

```sql
select
字段列表
from
表名列表
where
I条件列表
group by
分组字段
having
分组之后的条件
order by
排序
limit
分页限定
```

###### 2.基础查询

1.多个字段的询
select 字段名1，字段名2...from表名；
注意：
如果查询所有字段，则可以使用*来替代字段列表。
2.去除重复：
distinct
3.计算列
一般可以使用四则运算计算一些列的值。（一般只会进行数值型的计算）
ifnull（表达式1，表达式2）：nu11参与的运算，计算结果都为nul1
表达式1：哪个字段需要判断是否为nul1
如果该字段为nu11后的替换值。
4.起别名：
as:as也可以省略

###### 3.条件查询

1.where子句后跟条件

2.运算符
*>、<、<=、>=、=、<>

BETWEEN...AND

IN（集合）

like：*占位符：_单个任意字符 ; %：多个任意字符

IS NULL 

and 或&&

or或||*

not或！

###### 4.排序查询

语法

```sql
ORDER BY语句
ORDER BY 排序字段1，排序方式1， 排序字段2，排序方式2；
```

排序方式

升序：ASC（默认）

降序：DESC

```sql
SELECT * FROM student ORDER BY math;
SELECT * FROM student ORDER BY math ASC;
SELECT * FROM student ORDER BY math DESC;
SELECT * FROM student ORDER BY math ASC , english DESC;
```

注意：如果有多个排序条件，则当前边的条件值一样时，才会判断第二条件。

###### 5.聚合函数

将一列数据作为一个整体，进行纵向的计算。

count：计算总数

```SQL
SELECT COUNT(没有空值的列名) FROM 表名;
SELECT COUNT(english) FROM student;
SELECT COUNT(*) FROM student;
SELECT COUNT(id) FROM student;
```

max:计算最大值

```sql
SELECT MAX(需要计算的那一列) FROM 表名；
SELECT MAX(math) FROM 表名；---数学成绩最大
```

min:计算最小值

```sql
SELECT MIN(需要计算的那一列) FROM 表名；
SELECT MIN(math) FROM 表名；---数学成绩最小
```

sum：求和

```sql
SELECT SUM(需要计算的那一列) FROM 表名；
SELECT SUM(math) FROM 表名；---数学成绩之和
```

avg：求平均分

```sql
SELECT AVG(需要计算的那一列) FROM 表名；
SELECT AVG(math) FROM 表名；---数学成绩平均值
```

注意：聚合函数的计算，排除NULL值。

解决方法：1.选择不包含NULL值的一列 2.使用IFNULL函数

```sql
SELECT count(IFNULL(math,0) FROM 表名；
```

###### 6.分组查询

1.语法：group by 分组字段；

2.注意：

1).分组之后查询的字段：分组字段、聚合字段

2).where和having的区别

​	*where 在分组之前进行限定，如果不满足要求，则不参与分组。having在分组之后进行限定，如果不满足结果，则不会被查询出来

​	*where后不能跟聚合函数，having可以进行聚合函数的判断。

```sql
SELECT sex,AVG(math) FROM student GROUP BY sex; ----查看男女数学平均分
SELECT COUNT(id),AVG(math) FROM student GROUP BY sex; ----查看男女数学平均分，人数
SELECT COUNT(id),AVG(math) FROM student WHERE math>70 GROUP BY sex ---分数低于70的不参与分组
SELECT COUNT(id),AVG(math) FROM student WHERE math>70 GROUP BY sex HAVING COUNT(id) >2 ; ---且分组后要大于两个人
```

###### 7.分页查询

1.语法：limit开始的索引，每页查询的条数；
2.公式：开始的索引=（当前的页码-1）

```sql
*每页显示的条数
--每页显示3条记录
SELECT*FROM student LIMIT 0，3；--第1页
SELECT*FROM student LIMIT 3，3；--第2页
SELECT*FROM student LIMIT 6，3；--第3页
```

3.limit是一个MysQL"方言"

##### 4）DCL（Data control Language）数据控制语言（了解）用来定义数据库的访问权限和安全级别，及创建用户。关键字：GRANT，REVOKE等

##### 5.约束

概念：对表中的数据进行限定，保证数据的正确性、有效性和完整性。

###### 1.主键约束：primary key

1.注意：
1.含义：非空且唯一
2.一张表只能有一个字段为主键3.主键就是表中记录的唯一标识
2.在创建表时，添加主键约束

```sql
create table stu（
id int primary key，--给id添加主键约束name varchar（2e）
）；
```

3.删除主键

```sql
alter table stu modify id int；---错误

ALTER TABLE Stu DROP PRIMARY KEY；
```

4.创建完表后，添加主键

```sql
ALTER TABLE Stu MODIFY id INT PRIMARY KEY；
```

5.自动增长：
1.概念：如果某一列是数值类型的，使用auto increment可以来完成值得自动增长
2.在创建表时，添加主键约束，并且完成主键自增长

```sql
create table stu（
id int primary key auto increment，--给id添加主键约束name varchar（20）
）；
```

3.删除自动增长

```sql
ALTER TABLE Stu MODIFY id INT；
```

4.添加自动增长

```sql
ALTER TABLE Stu MODIFY id INT AUTO_INCREMENT；
```

4.添加自动增长
ALTER TABLE Stu MODIFY id INT AUTO_INCREMENT；

###### 2.非空约束：not null

```sql
1.创建表时添加约束
CREATE TABLE stu（
		id INT，
		NAME VARCHAR(20) NOT NULL--name为非空；
		
2.创建表完后，添加非空约束
ALTER TABLE Stu MODIFY NAME VARCHAR(20)NOT NULL；

3.删除name的共空约束
ALTER TABLE stu MODIFY NAME VARCHAR(20)；
```

###### 3.唯一约束：unique

```sql
1.注意：
唯一约束可以有NULL值，但是只能有一条记录为nul12.在创建表时，条件唯一约束
CREATE TABLE stu（
	id INT，
	phone_number VARCHAR（20）UNIQUE--手机号）;
	
3.删除唯一约束
ALTER TABLE stu DROP INDEX phone number;

4.在表创建完后，添加唯一约束
ALTER TABLE stu MODIFY phone_number VARCHAR（20）UNIQUE;
```

###### 4.外键约束：foreign key

1.在创建表时，可以添加外键
语法：

```sql
create table表名（外键列
constraint 外键名称foreign key（外键列名称）references 主表名称（主表列名称）
）；
```

2.删除外键

```sql
ALTER TABLE 表名DROP FOREIGN KEY 外键名称；
```

3.创建表之后，添加外键

```sql
ALTER TABLE 表名ADD CONSTRAINT 外键名称 FOREIGN KEY（外键字段名称）REFERENCES主表名称（主表列名称）；
```

4.级联操作
1.添加级联操作
语法：

```sql
ALTER TABLE 表名ADD CONSTRAINT 外键名
FOREIGNKEY（外键字段名称）REFERENCES 主表名称（主表列名称）ON UPDATE CASCADE ON DELETE CASCADE；
```

2.分类：
1 .级联更新：ON UPDATE CASCADE

2.级联删除：ON DELETE CASCADE

##### 6.多表之间的关系

###### 1.一对一（了解）：

如：人和身份证
分析：一个人只有一个身份证，一个身份证只能对应一个人

实现方式：一对一关系实现，可以在任意一方添加唯一外键指向另一方的主键。

###### 2.一对多（多对一）：

如：部门和员工

分析：一个部门有多个员工，一个员工只能对应一个部门

实现方式：在多的一方建立外键，指向一的一方的主键。

###### 3.多对多：

如：学生和课程

分析：一个学生可以选择很多门课程，一个课程也可以被很多学生选择

实现方式：多对多关系实现需要借助第三张中间表。中间表至少包含两个字段，这两个字段作为第三张表的外键，分别指向两张表的主键

##### 7.范式

###### 1.概念

设计数据库时，需要遵循的一些规范。要遵循后边的范式要求，必须先遵循前边的所有范式要求设计关系数据库时，遵从不同的规范要求，设计出合理的关系型数据库，这些不同的规范要求被称为不同的范式，各种范式呈递次规范，越高的范式数据库冗余越小。
目前关系数据库有六种范式：第一范式（1NF）、第二范式（2NF）、第三范式（3NF）、巴斯-科德范式（BCNF）、第四范式（4NF）和第五范式（5NF，又称完美范式）。

###### 2.分类

1.第一范式（1NF）：每一列都是不可分割的原子数据项

2.第二范式（2NF）：在1NF的基础上，非码属性必须完全依赖于码（在1NF基础上消除主属性对主码的部分函数依赖）
几个概念：

1.函数依赖：A-->B，如果通过A属性（属性组）的值，可以确定唯一B属性的值。则称B依赖于A例如：学号-->姓名。（学号，课程名称）-->分数
2.完全函数依赖：A-->B，如果A是一个属性组，则B属性值得确定需要依赖于A属性组中所有的属性值。
例如：（学号，课程名称）-->分数
3.部分函数依赖：A-->B，如果A是一个属性组，则B属性值得确定只需要依赖于A属性组中某一些值即可。
例如：（学号，课程名称）->姓名
4.传递函数依赖：A-->B，B-->C.如果通过A属性（属性组）的值，可以确定唯一B属性的值，在通过B属性（属性组）的值可以确定唯一C属性的值，则称C传递函数依赖于A例如：学号-->系名，系名-->系主任
5.码：如果在一张表中，一个属性或属性组，被其他所有属性所完全依赖，则称这个属性（属性组）为该表的码例如：该表中码为：（学号，课程名称）
*主属性：码属性组中的所有属性I
*非主属性：除过码属性组的属性

3.第三范式（3NF）：在2NF基础上，任何非主属性不依赖于其它非主属性（在2NF基础上消除传递依赖）

##### 8.数据库的备份和还原

###### 1.命令行：

语法：
备份：

```
mysq1dump-u用户名-p密码>保存的路径
```

还原：
1.登录数据库2.创建数据库3.使用数据库
4.执行文件。source文件路径

###### 2.图形化工具：

略

##### 9.多表查询

###### 1.内连接查询

1.隐式的内连接:使用WHERE 消除无用数据

```sql
SELECT * FROM emp,dept WHERE emp.dept_id = dept_id;

SELECT emp.name,emp.gender,dept.name FROM emp,dept WHERE emp.dept_id=dept_id;
---查询员工表的名称，性别。部门表的名称

SELECT 
	t1.name,
	t1.gender,
	t2.name
FROM
	emp t1,dept t2
WHERE
	t1.dept_id = t2.id;
```

2.显示内连接

语法：

```SQL
SELECT 字段列表 from 表名1 [inner] JOIN 表名2 ON 条件
```

例如：

```sql
SELECT * FROM emp INNER J0IN dept oN emp."dept id'=dept."id';

SELECT * FROM emp J0IN dept ON emp.dept id'=dept."id';
```

###### 2.外连接查询

1.左外连接：

语法：

```SQL
select 字段列表 from 表1 left [outer] join 表2 on 条件；
```

查询的是左表所有数据以及其交集部分。

2.右外连接：

语法：

```SQL
select 字段列表 from 表1 right [outer] join 表2 on 条件；
```

查询的是右表所有数据以及其交集部分。

###### 3.子查询

##### 10.事务

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

