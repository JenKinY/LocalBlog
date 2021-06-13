###  测试范围：

1. Java SE基础（变量、数组、流程控制、类与对象、封装、继承、多态、字符串、日期等）
2. Java SE中级 （异常处理、IO、集合框架、JDBC、多线程）

### 测试题目：

#### 一、选择题

1.  (单选题)如果一个方法或变量是"private"访问级别，那么它的访问范围是:【D】
   (A)在当前类，或者子类中
   (B)在当前类或者它的父类中
   (C)在当前类，或者它所有的父类中
   (D)在当前类中 

2.  (单选题)
   有如下一段程序：【B注意返回的是 i++,最后返回的是3++,容易误选C】

   ```java
   public class Test{ 
     private static int i=1;
     public int getNext(){ 
        return i++;
     } 
     public static void main(String [] args){ 
       Test test=new Test(); 
       Test testObject=new Test(); 
       test.getNext(); 
       testObject.getNext(); 
       System.out.println(testObject.getNext()); 
     } 
   }
   ```

   请问最后打印出来的是什么？

   (A)2
     (B)3
     (C)4
     (D)5

3.  (单选题)
   如下代码的 结果是什么 ? 【B:子类实例化时会调用父类构造方法】

   ```java
   class Base {
     Base() {
     System.out.print("Base"); 
     }
   }
   public class Alpha extends Base {
     public static void main( String[] args ) {
       new Alpha();
       new Base();
     } 
   }
   ```

   (A).Base

   (B).BaseBase

   (C).编译失败

   (D).代码运行但没有输出

   (E).运行时抛出异常

4. （单选题）下列说法正确的有【A:Java中数组为对象】

   (A)数组是一种对象
     (B)数组属于一种原生类
     (C)int number=[]={31,72,33,34,29,63}
     (D)数组的大小可以任意改变

5. （单选题）以下程序输出：【D：遇到break才跳出】

   ```java
   public static void main(String[] args) {
      int num = 2;
      switch (num) {
      case 1:
           ++num;
      case 2:
           ++num;
      case 3:
           ++num;
      default:
           ++num;
      break;
      }
      System.out.println(num);
    }
   }
   ```

   (A)2

   (B)3

   (C)4

   (D)5

6. （单选题）以下对继承的描述错误的是 【A:Java不能多继承】

   (A)Java中的继承允许一个子类继承多个父类

   (B)父类更具有通用性，子类更具体

   (C)Java中的继承存在着传递性

   (D)当实例化子类时会调用父类中的构造方法

7.  (单选题)关于容器下面说法正确的是? 【D:列表(List)的元素是有序、可重复的；集合(Set)的元素是无序、不可重复的】

   (A)列表(List)和集合(Set)存放的元素都是可重复的。

   (B)列表(List)和集合(Set)存放的元素都是不可重复的。

   (C)映射(Map)<key,value>中key是可以重复的。

   (D)映射(Map)<key,value>中value是可以重复的

8. 在Java中，已定义两个接口B和C，要定义一个实现两个接口的类，以下语句正确的是【C】

   (A)interface A extends B,C

   (B)interface A implements B,C

   (C)class A extends B,C

   (D)class A implements B,implements C

9. 以下哪个不是 Collection 的子接口？【D】

   (A)List

   (B)Set

   (C)SortedSet

   (D)Map

10. 下列 InputStream 类中的哪个方法用于关闭流？【B:  Binputstream的close方法用来关闭流；skip()用来跳过一些字节；mark（）用来标记流；reset（）复位流  】

    (A)skip()

    (B)close()

    (C)mark()

    (D)reset()

#### 二、操作题

1. 使用Java语言输出两种九九乘法表，正立直角&倒立直角

   [知识点：for循环的嵌套使用]

2. 将 [(点此下载)dc_learn01.sql](https://drinkcoffee.oss-cn-shenzhen.aliyuncs.com/FirstPractice/dc_learn01.sql) 导入数据库，使用Java获取数据库中数据：

   [知识点：JDBC]

   =新建一个名为 `DbUtils`的工具类

   - 编写方法 getAllData() ，方法返回数据库中所有数据
   - 编写方法 getMyData(String name)，方法返回一条个人信息
   - 编写方法 addUser(String name,Int qq,String gender)，方法可保存一条信息到数据库中。

   =新建一个名为`Main`的主类

   - 实现 main 方法，创建一个`DbUtils`对象，并实现DButils的3个方法：

     1.输出所有成员的信息

     2.输出自己的信息

     3.将 小花 的信息添加进数据库：

     | name | qq         | gender |
     | ---- | ---------- | ------ |
     | 小花 | 2457946139 | 女     |

3. 下载文件 [DrinkCoffee_FirstPractice.txt](https://drinkcoffee.oss-cn-shenzhen.aliyuncs.com/FirstPractice/DrinkCoffee_FirstPractice.txt) 将其放在项目根目录；

   - 读取其中内容并输出
   - 将自己的学号姓名追加到文件内容末尾