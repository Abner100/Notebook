 #

## 网络编程

### InterAdderss类   的使用

```java
String InetAddress getByName(String host)                 //确定主机名称的IP地址                          		String getHostName()                                      //获取IP地址中的主机名                                     Sring getHostAdress()									 	//返回文本显示的IP地址字符串
```

 

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

### 4.数据库

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

```
* SQL分类：
	1. DDL：操作数据库和表
	2. DML：增删改表中数据
	3. DQL：查询表中数据
	4. DCL：管理用户，授权

* DBA：数据库管理员

* DCL：管理用户，授权
	1. 管理用户
		1. 添加用户：
			* 语法：CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
		2. 删除用户：
			* 语法：DROP USER '用户名'@'主机名';
		3. 修改用户密码：
			
			UPDATE USER SET PASSWORD = PASSWORD('新密码') WHERE USER = '用户名';
			UPDATE USER SET PASSWORD = PASSWORD('abc') WHERE USER = 'lisi';
			
			SET PASSWORD FOR '用户名'@'主机名' = PASSWORD('新密码');
			SET PASSWORD FOR 'root'@'localhost' = PASSWORD('123');

			* mysql中忘记了root用户的密码？
				1. cmd -- > net stop mysql 停止mysql服务
					* 需要管理员运行该cmd

				2. 使用无验证方式启动mysql服务： mysqld --skip-grant-tables
				3. 打开新的cmd窗口,直接输入mysql命令，敲回车。就可以登录成功
				4. use mysql;
				5. update user set password = password('你的新密码') where user = 'root';
				6. 关闭两个窗口
				7. 打开任务管理器，手动结束mysqld.exe 的进程
				8. 启动mysql服务
				9. 使用新密码登录。
		4. 查询用户：
			-- 1. 切换到mysql数据库
			USE myql;
			-- 2. 查询user表
			SELECT * FROM USER;
			
			* 通配符： % 表示可以在任意主机使用用户登录数据库

	2. 权限管理：
		1. 查询权限：
			-- 查询权限
			SHOW GRANTS FOR '用户名'@'主机名';
			SHOW GRANTS FOR 'lisi'@'%';

		2. 授予权限：
			-- 授予权限
			grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';
			-- 给张三用户授予所有权限，在任意数据库任意表上
			
			GRANT ALL ON *.* TO 'zhangsan'@'localhost';
		3. 撤销权限：
			-- 撤销权限：
			revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';
			REVOKE UPDATE ON db3.`account` FROM 'lisi'@'%';
```

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

概念：查询中嵌套查询，称嵌套查询为子查询。

--查询工资最高的员工信息

--1查询最高的工资是多少9000

```sql
SELECT MAX（salary）FROM emp；
```

--2查询员工信息，并且工资等于9000的

```sql
SELECT * FROM emp WHERE emp."salary'=9000；
```

--一条sq1就完成这个操作。子查询

```sql
SELECT * FROM emp WHERE emp.'salary'=（SELECT MAX（salary）FROM emp）； 
```

子查询不同情况

1.子查询的结果是单行单列的：
子查询可以作为条件，使用运算符去判断。运算符：> >= < <=  =

```sql
--查询员工工资小于平均工资的人
SELECT * FROM emp WHERE emp.salary <（SELECT AVG（salary）FRoM emp）；
```

2.子查询的结果是多行单列的：
子查询可以作为条件，使用运算符in来判断

```sql
--查询‘财务部‘和’市场部‘所有的员工信息
SELECT id FROM dept WHERE NAME =‘财务部’ OR NAME=‘市场部’；
SELECT * FROM emp WHERE dept id=3 OR dept id=2；

--子查询
SELECT * FROM emp WHERE dept_id IN（SELECT id FROM dept WHERE NAME=‘财务部’OR NAME=‘市场部’）；
```

3.子查询的结果是多列的：

子查询可以作为一张虚拟表参与查询

```sql
-查询员工入职日期是2011-11-11日之后的员工信息和部门信息
-子查询
SELECT*FROM dept t1，（SELECT*FROM emp WHERE emp."join date'>‘2011-11-11'）t2
WHERE t1.id=t2.dept_id;

-普通内连接
SELECT*FROM emp t1，dept t2 WHERE t1."dept id'=t2."id AND t1.join date'>'2011-11-11' ;
```

###### 10.事务

1.概念：
	*如果一个包含多个步骤的业务操作，被事务管理，那么这些操作要么同时成功，要么同时失败。I
2.操作：

```SQL
---1.开后事务：
start transaction；
---2.回滚：
ro1lback；
---3.提交：
commit；
```

3. 例子：

    ```SQL
    3. CREATE TABLE account (
       		id INT PRIMARY KEY AUTO_INCREMENT,
       		NAME VARCHAR(10),
       		balance DOUBLE
       	);
       	-- 添加数据
       	INSERT INTO account (NAME, balance) VALUES ('zhangsan', 1000), ('lisi', 1000);
    		SELECT * FROM account;
    		UPDATE account SET balance = 1000;
    		-- 张三给李四转账 500 元
    		
    		-- 0. 开启事务
    		START TRANSACTION;
    		-- 1. 张三账户 -500
    		
    		UPDATE account SET balance = balance - 500 WHERE NAME = 'zhangsan';
    		-- 2. 李四账户 +500
    		-- 出错了...
    		UPDATE account SET balance = balance + 500 WHERE NAME = 'lisi';
    		
    		-- 发现执行没有问题，提交事务
    		COMMIT;
    		
    		-- 发现出问题了，回滚事务
    		ROLLBACK;
    ```

    4.MySQL数据库中事务默认自动提交
    事务提交的两种方式：
    自动提交：
    mysq1就是自动提交的
    一条DML（增删改）语句会自动提交一次事务。
    手动提交：
    Oracle数据库默认是手动提交事务
    需要先开后事务，再提交
    修改事务的默认提交方式：
    查看事务的默认提交方式：SELECT@@autocommit；--1代表自动提交。代表手动提交
    修改默认提交方式：set@@autocommit=e；

    2.事务的四大特征

      1.原子性：是不可分割的最小操作单位，要么同时成功，要么同时失败。
    2. 持久性：当事务提交或回滚后，数据库会持久化的保存数据。
    3. 隔离性：多个事务之间。相互独立。
    4. 一致性：事务操作前后，数据总量不变

    3.事务的隔离级别（了解）

    3.事务的隔离级别（了解）
    
    

### 5.JDBC

##### 1.基本概念

Java DataBase Connectivity Java数据库连接，Java语言操作数据库

JDBC本质：其实是官方（sun公司）定义的一套操作所有关系型数据库的规则，即接口。各个数据库厂商去实现这套接口，提供数据库驱动jar包。我们可以使用这套接口（JDBC）编程，真正执行的代码是驱动jar包中的实现类

###### 2.JDBC快速入门

1.导入jar包 mysql-connector-java-bin-jar

2.注册驱动

3.获取数据库连接对象

4.定义sql

5.获取执行sql语句的对象Statement

6.执行sql，接受返回结果

7.处理结果

8.释放资源

```java
 代码实现：
	  	//1. 导入驱动jar包
        //2.注册驱动
        Class.forName("com.mysql.jdbc.Driver");
        //3.获取数据库连接对象
        Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/db3", "root", "root");
        //4.定义sql语句
        String sql = "update account set balance = 500 where id = 1";
        //5.获取执行sql的对象 Statement
        Statement stmt = conn.createStatement();
        //6.执行sql
        int count = stmt.executeUpdate(sql);
        //7.处理结果
        System.out.println(count);
        //8.释放资源
        stmt.close();
        conn.close();

```



###### 3.详解各个对象

1.DriverManager：驱动管理对象

注册驱动：高数程序应该使用哪一个数据库驱动jar

```java
static void reisterDriver(Driver driver)
//注册与给定驱动程序DriverManager
写代码使用：Class.forName("com.mysql.jdbc.Driver");
通过查看源码发现：在com.mysql.jdbc.Driver类中存在静态代码块
				 static {
				        try {
				            java.sql.DriverManager.registerDriver(new Driver());
				        } catch (SQLException E) {
				            throw new RuntimeException("Can't register driver!");
				        }
					}

//注意：mysql5之后的驱动jar包可以省略注册驱动的步骤。
```

​	2. 获取数据库连接：

```java
方法：static Connection getConnection(String url, String user, String password) 
				参数：
					 url：指定连接的路径
						 语法：jdbc:mysql://ip地址(域名):端口号/数据库名称
						 例子：jdbc:mysql://localhost:3306/db3
						 细节：如果连接的是本机mysql服务器，并且mysql服务默认端口是3306，则url可以简写为：jdbc:mysql:///数据库名称
					 user：用户名
					 password：密码 
```

2.Connection：数据库连接对象

1. 功能：
			1. 获取执行sql 的对象
				* Statement createStatement()
				* PreparedStatement prepareStatement(String sql)  
			2. 管理事务：
				* 开启事务：setAutoCommit(boolean autoCommit) ：调用该方法设置参数为false，即开启事务
				* 提交事务：commit() 
				* 回滚事务：rollback() 

3.Statement

1. 执行sql
			1. boolean execute(String sql) ：可以执行任意的sql 了解 
			2. int executeUpdate(String sql) ：执行DML（insert、update、delete）语句、DDL(create，alter、drop)语句
				* 返回值：影响的行数，可以通过这个影响的行数判断DML语句是否执行成功 返回值>0的则执行成功，反之，则失败。
			
			3. ResultSet executeQuery(String sql)  ：执行DQL（select)语句
			
		2. 练习：
			
			1. account表 添加一条记录
	2. account表 修改记录
			3. account表 删除一条记录
		
			代码：
			
		   ```java
		   Statement stmt = null;
		        Connection conn = null;
		        try {
		            //1. 注册驱动
		            Class.forName("com.mysql.jdbc.Driver");
		            //2. 定义sql
		            String sql = "insert into account values(null,'王五',3000)";
		            //3.获取Connection对象
		            conn = DriverManager.getConnection("jdbc:mysql:///db3", "root", "root");
		            //4.获取执行sql的对象 Statement
		            stmt = conn.createStatement();
		            //5.执行sql
		            int count = stmt.executeUpdate(sql);//影响的行数
		            //6.处理结果
		            System.out.println(count);
		            if(count > 0){
		                System.out.println("添加成功！");
		            }else{
		             System.out.println("添加失败！");
		            } 
		   } catch (ClassNotFoundException e) {
		        e.printStackTrace();
		    } catch (SQLException e) {
		        e.printStackTrace();
		    }finally {
		        //stmt.close();
		        //7. 释放资源
		        //避免空指针异常
		        if(stmt != null){
		            try {
		                stmt.close();
		            } catch (SQLException e) {
		                e.printStackTrace();
		            }
		     }
		   
		        if(conn != null){
		            try {
		                conn.close();
		            } catch (SQLException e) {
		                e.printStackTrace();
		            }
		        }
			 }
			```
			

4.Resultset 

* boolean next(): 游标向下移动一行，判断当前行是否是最后一行末尾(是否有数据)，如果是，则返回false，如果不是则返回true
		* getXxx(参数):获取数据
			* Xxx：代表数据类型   如： int getInt() ,	String getString()
			* 参数：
				1. int：代表列的编号,从1开始   如： getString(1)
				2. String：代表列名称。 如： getDouble("balance"
		

```java
* 使用步骤：
				1. 游标向下移动一行
				2. 判断是否有数据
				3. 获取数据

			   //循环判断游标是否是最后一行末尾。
	            while(rs.next()){
	                //获取数据
	                //6.2 获取数据
	                int id = rs.getInt(1);
	                String name = rs.getString("name");
	                double balance = rs.getDouble(3);
	
	                System.out.println(id + "---" + name + "---" + balance);
	            }
```

###### 4.PreparedStatement：执行sql的对象

1. SQL注入问题：在拼接sql时，有一些sql的特殊关键字参与字符串的拼接。会造成安全性问题
			1. 输入用户随便，输入密码：a' or 'a' = 'a
			2. sql：select * from user where username = 'fhdsjkf' and password = 'a' or 'a' = 'a' 

		3. 解决sql注入问题：使用PreparedStatement对象来解决
		
		4. 预编译的SQL：参数使用?作为占位符
		
		4. 步骤：
			
			```java
			String sql="insert into account value(null,?,?)";
			PreparedStatement pstm= Connection.prepareStatement(String sql); 
			pstm.setString(1,"王五");
			pstm.setString(2,"2000");
			```
			
			1. 导入驱动jar包 mysql-connector-java-5.1.37-bin.jar
			2. 注册驱动
			3. 获取数据库连接对象 Connection
	4. 定义sql
				* 注意：sql的参数使用？作为占位符。 如：select * from user where username = ? and password = ?;
		5. 获取执行sql语句的对象 PreparedStatement  pstm= Connection.prepareStatement(String sql) 
			6. 给？赋值：
				* 方法： setXxx(参数1,参数2)
					* 参数1：？的位置编号 从1 开始
					* 参数2：？的值
			7. 执行sql，接受返回结果，不需要传递sql语句
			8. 处理结果
			9. 释放资源
			
		6. 注意：后期都会使用PreparedStatement来完成增删改查的所有操作
		
		  1. 可以防止SQL注入
		  2. 效率更高

##### 2.数据库连接池

###### 1.概念

一个容器(集合)，存放数据库连接的容器

当系统初始化好后，容器被创建，容器中会申请一些访问对象，当用户来访问时，从容器中获取数据库连接对象，当访问结束后就吧对象归还到容器中。

###### 2.好处

1.节约资源

2.用户访问高效

###### 3.实现

1.标准接口：DataSource  javax.sql包下的

1.方法：

​	获取连接:getConnection()

​	归还连接：Connection.close()  如果connection对象是从连接池获得的，那么调用Connection.close()方法不会关闭连接而是将connection对象归还连接池

2.一般我们不去实现他，有数据库厂商实现

1.c3p0：数据库连接池技术

2.Druid：数据库连接池实现技术，由阿里巴巴提供

###### 4.CP30

数据库连接池技术

步骤：
1.导入jar包（两个）

c3pe-e.9.5.2.jar 			mchange-commons-java-0.2.12.jar，

*不要忘记导入数据库驱动jar包

2.定义配置文件：

*名称：c3pe.properties 或者c3pe-config.xml

*路径：直接捋文件放在src目录下即可。

3.创建核心对象数据库连接池对象 ComboPooledDatasource

4.获取连接：getconnection

```xml
<!--连接参数-->
<!-- 类地址-->
<property name="driverClass">com.mysql.jdbc.Driver</property>
<!--数据库地址-->
<property name="jdbcUrl">jdbc:mysql://localhost:3306/day25</property>
<!--数据库用户名-->
<property name="user">root</property>
<!--数据库用户密码-->
<property name="password">root</property>
    
<!--连接池参数-->
<!--初始化申请的连接数量-->
<property name="initialPoolSize">5</property>
<!--最大的连接数量-->
<property name="maxPoolSize">10</property>
<!--连接超时最大能接受时间-->
<property name="checkoutTimeout">3000</property>
```

配置文件

```java
//创建数据库连接池对象
DataSource ds =new ComboPooledDataSource();
//获取连接对象
Connection conn =ds.getConnection();
//打印
System.out.println(conn);
```

连接语法

```java
//使用指定配置名的数据库连接池引用
DataSource ds =new ComboPooledDataSource("xxxxxxx");
```

使用指定配置文件连接

###### 5.Druid

*步骤：

1.导入jar包 druid-1.0.9.jar

2.定义配置文件：
*是properties形式的

```java
//获取properties的方法
Properties pro=new Properties();
InputStream is =DruidDemo.class.getClassLoader().getResourceAsStream(name:"druid.properties");
 pro.load（is）；
```

*可以叫任意名称，可以放在任意目录下

```properties
driverClassName=com. mysql. jdbc. Driver 
url=jdbc: mysql://127.0.0,1:3306/db3
username=root
password=root
initialSize=5
maxActive=10
maxWait=3000
```

3.获取数据库连接池对象：通过工厂来来获取 DruidDataSourceFactory

```java
DataSource ds = new DruidDataSourceFactory().createDataSource(pro);
```

4.获取连接：

```java
 Connection conn = ds.getConnection();
```

例子：

```java
 public static void main(String[] args) {
        Properties pro = new Properties();
        try (InputStream is = druid.class.getClassLoader().getResourceAsStream("druid.properties")) {
            pro.load(is);
            DataSource ds = new DruidDataSourceFactory().createDataSource(pro);

            Connection conn = ds.getConnection();
            System.out.println(conn);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

###### 6.JDBCTemplate

Spring框对JDBC的简单到装。提供了一个JDBCTemplate对家简化JDBC的升友

步骤：

1.导入jar包

2.创建dbcTemplate对象。依赖于数据源Datasource
*JdbcTemplate template=new JdbcTemplate（ds）；

3.调用JdbcTemplate的方法来完成CRUD的操作
*update（）：执行DML语句。增、、改语句

*queryForMap（）：查询结果将列名作为key，将值作为value结果集封装为map集合

这个结果集只能是1为map集合 

queryForList（）：查询结果将结果集封装为list集合
*query（）：查询结果，将结果封装为JavaBean对象
*queryForObject：查询结果，捋结果封装为对象

```java
public static void main（String[]args）{
//1.导入jar包
//2.创建JDBCTemplate对象
JdbcTemplate template=new JdbcTemplate（JDBCUtils.getDataSource（））；
//3.调用方法
String sql="update account set balance=5000 where id=？"；int count=template.update（sql，.…args：3）；System.out.print1n（count）；
```

```java
update（）
//执行DML语句。增、删、改语句
queryForMap（）：
//查询结果捋结果集封装为map集合，将列名作为key，将值作为value 将这条记录封装为一个map集合
//注意：这个方法查询的结果集长度只能是1
queryForList（）：
//查询结果捋结果集封装为list集合
//注意：将每一条记录封装为一个Map集合，再将Map集合装载到List集合中
query（）：
//查询结果，捋结果封装为JavaBean对象
query的参数：RowMapper
//一般我们使用BeanPropertyRowMapper实现类。可以完成数据到JavaBean的自动封装
new BeanPropertyRowMapper<类型>（类型.class）
queryForobject：查询结果，捋结果封装为对象
//一般用于聚合函数的查询
```





















### 6.JaveScript

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

#### 5.DOM

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

#### 6.BOM

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

#### 7.HTMLDOM

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

### 7.Bootstrap框架

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

###  8.XML

#### 1.概念

Extensible Markup Language 可扩展标记语言

##### 1.可扩展

标签都是自定义的。

##### 2.功能

###### 1.存储数据

1.配置文件
2.在网络中传输

##### 3.xml与htm1的区别

1.xml标签都是自定义的，html标签是预定义。
2.xml的语法严格，html语法松散
3.xml是存储数据的，html是展示数据

##### 4.W3C

##### 万维网联盟

#### 2.语法

```xml
<? xml version='1.01' ?>
<users>  //根标签
	<user id='1'> 
		<name>zhangsan</name>
		<age>23</age>
		<gender>male</gender>
	</user>
	<user id='2'>
		<name>lisi</name>
		<age>24</age>
		<gender>female</gender>
	</user>
</users>
```

##### 1.基本语法

1.xml文档的后缀名  .xml

2.xml的第一行必须定义为文档声明

3.xml文档中有且仅有一个根标签

4.属性值必须使用引号引起来

5.标签必须正确关闭

6.xml标签名称区分大小写

##### 2.组成部分

###### 1.文档声明

1.格式：

<？xml属性列表？>

2.属性列表：

version：版本号，r必须的属性

encoding：编码方式。告知解析引擎当前文档使用的字符集，默认值：IS0-8859-1

standalone：是否独立

取值：

yes：不依赖其他文件
no：依赖其他文件

###### 2.指合（了解）

结合css的

*<？xml-stylesheet type="text/css"href="a.css"？>

###### 3.标签

标签名称自定义的

规则：

名称可以包含字母、数字以及其他的字符
名称不能以数字或者标点符号开始
名称不能以字母xml（或者XML、Xml等等）开始
名称不能包含空格

###### 4.属性

id属性值唯一

###### 5.文本

CDATA区：在该区域中的数据会被原样展示

格式：<![CDATA[数据]]>

#### 3.约束

###### 1.基本概念

规定XML文档的书写规则

###### 2.作为框架的使用者（程序员）：

1.能够在xm1中中引人约束文档
2.能够简单的读懂约束文档

##### 4.两种不同的约束 

###### 1.DTD

1.概念

一种简单的约束技术

2.分类

*内部dtd：捋约束规则定义在xm1文档中
*外部dtd：捋约束的规则定义在外部的dtd文件中

3.用法

```xml
*引人dtd文档到xm1文档中
*本地：<！DOCTYPE根标签名 SYSTEM"dtd文件的位置"）
*网络：<！DOCTYPE 根标签名 PUBLIC "dtd文件名字" "dtd文件的位置URL">
```

4.缺点

无法对内容进行限定

###### 2.Schema

1.概念

一种复杂的约束技术

2.用法

1.填写xm1文档的根元素
2.引入xsi前缀.xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
3.引入xsd文件命名空间.xsi:schemalocation="http://www.itcast.cn/xml student.xsd"
4.为每一个xsd约束声明一个前缀，作为标识xmlns="http://www.itcast.cn/xml"

```xml
<students xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns="http://www.itcast.cn/xml"
xsi:schemalocation="http://www.itcast.cn/xml student.xsd">
```

#### 5.解析xml的方式：

###### 1.DoM

捋标记语言文档一次性加载进内存，在内存中形成一颗dom树

*优点：操作方便，可以对文档进行CRUD的所有操作
*缺点：占内存

###### 2.SAx

逐行读取，基于事件驱动的。

*优点：不占内存。
*缺点：只能读取，不能增删改

#### 6.常见的解析器

1.JAxP:sun公司提供的解析器，支持dom和sax两种思想

2.DOM4]：一款非常优秀的解析器

3.Jsoup:jsoup是一款Java的HTML解析器，可接解析某个URL地址、HTML文本内容。它提供了一套非常省力的API，可通过DOM，CSs以及类似于jQuery的操作方法来取出和操作数据。

4.PULL:Android操作系统内置的解析器，sax方式的。

jsoup:jsoup是一款Java的HTML解析器，可直接解析某个URL地址、HTML文本内容。它提供了一套井常省力的API，可通过DoM，Css以及类似于jQuery的操作方法来取出和操作数据。

```java
public static void main（String[]args）throws IOException{
//2.获取Document对象，根据xm1文档获取
//2.1获取student.xml的path 
    String path=JsoupDemol.class.getClassLoader（）.getResource（name:"student.xm1"）.getPath（）；
//2.2解析xm1文档，加载文档进内存，获取dom树--->Document 
    Document document=Jsoup.parse（new File（path），charsetName:"utf-8"）；
//3.获取元素对象Element 
    Elements elements=document.getElementsByTag（tagName:"name"）；System.out.println（elements.size（））；
//3.1获取第一个name的Element对象
	Element element=elements.get（0）；
//3.2获取数据
	String name=element.text（）；
     System.out.print1n（name）；
```

#### 7.对象的使用

###### 1.Jsoup

工具类，可以解析html或xm1文档，返回Document h*parse：解析html或xm1文档，返回Document

```java
parse（File in，string charsetName）：解析xml或html文件的。
parse（String html）：解析xml或html字符串
parse（URL url，int timeoutMillis）：通过网络路径获取指定的html或xml的文档对象
```

###### 2.Document

文档对象。代表内存中的dom树

```java
//获取Element对象
getE1ementById（String id）：根据id属性值获取唯一的element对象*getElementsByTag（String tagName）：根据标签名称获取元素
//对象集合
getElementsByAttribute（string key）：根据属性名称获取元素对象集合
getElementsByAttributeValue（String key，String value）：根据对应的属性名和属性值获取元素对象集合
```

###### 3.Elements

元素Element对象的集合。可以当做ArrayList<Element>来使用

4.Element：元素对象

1.获取子元素对象
getElementById（string id）：根据id属性值获取唯一的element对象*getElementsByTag（string tagName）：根据标签名称获取元素对象集合
getElementsByAttribute（string key）：根据属性名称获取元素对象集合
getElementsByAttributevalue（String key，string value）：根据对应的属性名和属性值获取元素对象集合

2.获取属性值
string attr（string key）：根据属性名称获取属性值

3.获取文本内容
string text（）：获取文本内容
string html（）：获取标签体的所有内容（包括字标签的字符串内容）

###### 4.Node

节点对象
是Document和Element的父类

###### 5.XPATH

XPath即为XML路径语言，它是一种用来确定XML（标准通用标记语言的子集）文档中某部分位置的语言

使用Jsoup的Xpath需要额外导入jar包

查询w3cshool参考手册，使用xpath的语法完成查询

```java
代码：
				//1.获取student.xml的path
		        String path = JsoupDemo6.class.getClassLoader().getResource("student.xml").getPath();
		        //2.获取Document对象
		        Document document = Jsoup.parse(new File(path), "utf-8");
		
		        //3.根据document对象，创建JXDocument对象
		        JXDocument jxDocument = new JXDocument(document);
		
		        //4.结合xpath语法查询
		        //4.1查询所有student标签
		        List<JXNode> jxNodes = jxDocument.selN("//student");
		        for (JXNode jxNode : jxNodes) {
		            System.out.println(jxNode);
		        }
		
		        System.out.println("--------------------");
		
		        //4.2查询所有student标签下的name标签
		        List<JXNode> jxNodes2 = jxDocument.selN("//student/name");
		        for (JXNode jxNode : jxNodes2) {
		            System.out.println(jxNode);
		        }
		
		        System.out.println("--------------------");
		
		        //4.3查询student标签下带有id属性的name标签
		        List<JXNode> jxNodes3 = jxDocument.selN("//student/name[@id]");
		        for (JXNode jxNode : jxNodes3) {
		            System.out.println(jxNode);
		        }
		        System.out.println("--------------------");
		        //4.4查询student标签下带有id属性的name标签 并且id属性值为itcast
		
		        List<JXNode> jxNodes4 = jxDocument.selN("//student/name[@id='itcast']");
		        for (JXNode jxNode : jxNodes4) {
		            System.out.println(jxNode);
		        }
```

### #.jQuery

#### 1. 概念

 一个JavaScript框架。简化JS开发

jQuery是一个快速、简洁的JavaScript框架，是继Prototype之后又一个优秀的JavaScript代码库（或JavaScript框架）。jQuery设计的宗旨	是“write Less，Do More”，即倡导写更少的代码，做更多的事情。它封装JavaScript常用的功能代码，提供一种简便的JavaScript设计模式，优	化HTML文档操作、事件处理、动画设计和Ajax交互。

JavaScript框架：本质上就是一些js文件，封装了js的原生代码而已

#### 2. 快速入门

##### 1. 步骤

###### 1. 下载JQuery

目前jQuery有三个大版本：

1.x：兼容ie678,使用最为广泛的，官方只做BUG维护， 功能不再新增。因此一般项目来说，使用1.x版本就可以了，
最终版本：1.12.4 (2016年5月20日)

2.x：不兼容ie678，很少有人使用，官方只做BUG维护，
功能不再新增。如果不考虑兼容低版本的浏览器可以使用2.x，
最终版本：2.2.4 (2016年5月20日)

3.x：不兼容ie678，只支持最新的浏览器。除非特殊要求，一般不会使用3.x版本的，很多老的jQuery插件不支持这个版本。
目前该版本是官方主要更新维护的版本。最新版本：3.2.1（2017年3月20日）

jquery-xxx.js 与 jquery-xxx.min.js区别：				

jquery-xxx.js：开发版本。给程序员看的，有良好的缩进和注释。体积大一些

jquery-xxx.min.js：生产版本。程序中使用，没有缩进。体积小一些。程序加载更快

###### 2. 导入JQuery的js文件

导入min.js文件

##### 2. 使用

```js
var div1 = $("#div1");
alert(div1.html());
```

##### 3. JQuery对象和JS对象区别与转换

1. JQuery对象在操作时，更加方便。
2. JQuery对象和js对象方法不通用的.
3. 两者相互转换
    * jq -- > js : jq对象[索引] 或者 jq对象.get(索引)
    * js -- > jq : $(js对象)

##### 4. 选择器：筛选具有相似特征的元素(标签)

```js
1. 基本操作学习：
		1. 事件绑定
			//1.获取b1按钮
            $("#b1").click(function(){
                alert("abc");
            });
		2. 入口函数
			 $(function () {
	           
   			 });
			 window.onload  和 $(function) 区别
                 * window.onload 只能定义一次,如果定义多次，后边的会将前边的覆盖掉
                 * $(function)可以定义多次的。
		3. 样式控制：css方法
			 // $("#div1").css("background-color","red");
      		$("#div1").css("backgroundColor","pink");
2. 分类
		1. 基本选择器
			1. 标签选择器（元素选择器）
				* 语法： $("html标签名") 获得所有匹配标签名称的元素
			2. id选择器 
				* 语法： $("#id的属性值") 获得与指定id属性值匹配的元素
			3. 类选择器
				* 语法： $(".class的属性值") 获得与指定的class属性值匹配的元素
			4. 并集选择器：
				* 语法： $("选择器1,选择器2....") 获取多个选择器选中的所有元素
		2. 层级选择器
			1. 后代选择器
				* 语法： $("A B ") 选择A元素内部的所有B元素		
			2. 子选择器
				* 语法： $("A > B") 选择A元素内部的所有B子元素
		3. 属性选择器
			1. 属性名称选择器 
				* 语法： $("A[属性名]") 包含指定属性的选择器
			2. 属性选择器
				* 语法： $("A[属性名='值']") 包含指定属性等于指定值的选择器
			3. 复合属性选择器
				* 语法： $("A[属性名='值'][]...") 包含多个属性条件的选择器 
		4. 过滤选择器
			1. 首元素选择器 
				* 语法： :first 获得选择的元素中的第一个元素
			2. 尾元素选择器 
				* 语法： :last 获得选择的元素中的最后一个元素
			3. 非元素选择器
				* 语法： :not(selector) 不包括指定内容的元素
			4. 偶数选择器
				* 语法： :even 偶数，从 0 开始计数
			5. 奇数选择器
				* 语法： :odd 奇数，从 0 开始计数
			6. 等于索引选择器
				* 语法： :eq(index) 指定索引元素
			7. 大于索引选择器 
				* 语法： :gt(index) 大于指定索引元素
			8. 小于索引选择器 
				* 语法： :lt(index) 小于指定索引元素
			9. 标题选择器
				* 语法： :header 获得标题（h1~h6）元素，固定写法
		5. 表单过滤选择器
			1. 可用元素选择器 	
				* 语法： :enabled 获得可用元素
			2. 不可用元素选择器 
				* 语法： :disabled 获得不可用元素
			3. 选中选择器 
				* 语法： :checked 获得单选/复选框选中的元素
			4. 选中选择器 
				* 语法： :selected 获得下拉框选中的元素

```

##### 5. DOM操作

```js
1. 内容操作
		1. html(): 获取/设置元素的标签体内容   <a><font>内容</font></a>  --> <font>内容</font>
		2. text(): 获取/设置元素的标签体纯文本内容   <a><font>内容</font></a> --> 内容
		3. val()： 获取/设置元素的value属性值
	2. 属性操作
		1. 通用属性操作
			1. attr(): 获取/设置元素的属性
			2. removeAttr():删除属性
			3. prop():获取/设置元素的属性
			4. removeProp():删除属性

			* attr和prop区别？
				1. 如果操作的是元素的固有属性，则建议使用prop
				2. 如果操作的是元素自定义的属性，则建议使用attr
		2. 对class属性操作
			1. addClass():添加class属性值
			2. removeClass():删除class属性值
			3. toggleClass():切换class属性
				* toggleClass("one"): 
					* 判断如果元素对象上存在class="one"，则将属性值one删除掉。  如果元素对象上不存在class="one"，则添加
			4. css():
	3. CRUD操作:
		1. append():父元素将子元素追加到末尾
			* 对象1.append(对象2): 将对象2添加到对象1元素内部，并且在末尾
		2. prepend():父元素将子元素追加到开头
			* 对象1.prepend(对象2):将对象2添加到对象1元素内部，并且在开头
		3. appendTo():
			* 对象1.appendTo(对象2):将对象1添加到对象2内部，并且在末尾
		4. prependTo()：
			* 对象1.prependTo(对象2):将对象1添加到对象2内部，并且在开头
        5. after():添加元素到元素后边
			* 对象1.after(对象2)： 将对象2添加到对象1后边。对象1和对象2是兄弟关系
		6. before():添加元素到元素前边
			* 对象1.before(对象2)： 将对象2添加到对象1前边。对象1和对象2是兄弟关系
		7. insertAfter()
			* 对象1.insertAfter(对象2)：将对象2添加到对象1后边。对象1和对象2是兄弟关系
		8. insertBefore()
			* 对象1.insertBefore(对象2)： 将对象2添加到对象1前边。对象1和对象2是兄弟关系

		9. remove():移除元素
			* 对象.remove():将对象删除掉
		10. empty():清空元素的所有后代元素。
			* 对象.empty():将对象的后代元素全部清空，但是保留当前对象以及其属性节点
```

#### 3.JQuery 高级

##### 1.动画

1. 三种方式显示和隐藏元素
		1. 默认显示和隐藏方式
			1. show([speed,[easing],[fn]])
				1. 参数：
					1. speed：动画的速度。三个预定义的值("slow","normal", "fast")或表示动画时长的毫秒数值(如：1000)
					2. easing：用来指定切换效果，默认是"swing"，可用参数"linear"
						* swing：动画执行时效果是 先慢，中间快，最后又慢
						* linear：动画执行时速度是匀速的
					3. fn：在动画完成时执行的函数，每个元素执行一次。

			2. hide([speed,[easing],[fn]])
			3. toggle([speed],[easing],[fn])
		
		2. 滑动显示和隐藏方式
			1. slideDown([speed],[easing],[fn])
			2. slideUp([speed,[easing],[fn]])
			3. slideToggle([speed],[easing],[fn])

		3. 淡入淡出显示和隐藏方式
			1. fadeIn([speed],[easing],[fn])
			2. fadeOut([speed],[easing],[fn])
			3. fadeToggle([speed,[easing],[fn]])

##### 2.遍历

1. js的遍历方式
		* for(初始化值;循环结束条件;步长)
	2. jq的遍历方式
		1. jq对象.each(callback)
			1. 语法：
				jquery对象.each(function(index,element){});
					* index:就是元素在集合中的索引
					* element：就是集合中的每一个元素对象

					* this：集合中的每一个元素对象
			2. 回调函数返回值：
				* true:如果当前function返回为false，则结束循环(break)。
				* false:如果当前function返回为true，则结束本次循环，继续下次循环(continue)
		2. $.each(object, [callback])
		3. for..of: jquery 3.0 版本之后提供的方式
			for(元素对象 of 容器对象)
	

##### 3.事件绑定

1. jquery标准的绑定方式
		* jq对象.事件方法(回调函数)；
		* 注：如果调用事件方法，不传递回调函数，则会触发浏览器默认行为。
			* 表单对象.submit();//让表单提交
	2. on绑定事件/off解除绑定
		* jq对象.on("事件名称",回调函数)
		* jq对象.off("事件名称")
			* 如果off方法不传递任何参数，则将组件上的所有事件全部解绑
	3. 事件切换：toggle
		* jq对象.toggle(fn1,fn2...)
			
			* 当单击jq对象对应的组件后，会执行fn1.第二次点击会执行fn2.....
			
		* 注意：1.9版本 .toggle() 方法删除,jQuery Migrate（迁移）插件可以恢复此功能。
			 
			 <script src="../js/jquery-migrate-1.0.0.js" type="text/javascript" charset="utf-8"></script>

##### 4.案例

1. 广告显示和隐藏
		<!DOCTYPE html>
		<html>
		<head>
		    <meta charset="UTF-8">
		    <title>广告的自动显示与隐藏</title>
		    <style>
		        #content{width:100%;height:500px;background:#999}
		    </style>
	
		

##### 