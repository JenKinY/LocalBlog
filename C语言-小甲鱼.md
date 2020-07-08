## 4. 变量

```c
printf("格式化输出 %d",num)
```

| 符号 | 意义                                           |
| ---- | ---------------------------------------------- |
| %d   | （int , short int）整数                        |
| %c   | （char）字符                                   |
| %s   | （string）字符串                               |
| %f   | （float）浮点数，默认只输出6位                 |
| %lf  | （double）双精度浮点数，默认只输出6位          |
| %o   | 以八进制数形式输出整数，                       |
| %x   | 以十六进制数形式输出整数，或输出字符串的地址。 |
| %e   | 以指数形式输出实数，                           |

## 5. 常量和宏定义

### 常量（符号常量是宏定义）

1. 整型常量：123，520

2. 实型常量：3.14，5.12

3. 字符常量

   - 普通字符：'a'，'b'
   - 转义字符：'\n'，'\t'，'\b'

4. 字符串常量："Skyzc"

   *【计算机是怎么知道字符串结尾在何处呢？C语言在字符串末尾加入了一个'\0'，用来表示字符串结尾。所以字符串长度要加上'\0'】*

5. 符号常量：**[#define 标识符(通常全大写) 常量]**

### 标识符（identifier）

1. 只能由 `英文`、`数字`、`下划线`组成
2. 第一个必须是**字母或下划线**
3. 不能是关键字

## 6. 数据类型

### 1.常见数据类型

![](E:\LocalBlog\images\C-xiaojiayu\C数据类型.jpg)

可以为一些数据类型加上长度限定符：

![](E:\LocalBlog\images\C-xiaojiayu\C基本类型.jpg)

### 2.sizeof()

sizeof()用于获得`数据类型`或`表达式`的长度。单位：字节（不同编译系统值不同）

- sizeof(object); // sizeof(对象)；
- sizeof(type_name);//sizeof(类型)；
- sizeof object; //sizeof 对象；

### 3.signed 和 unsigned

用于限定整型变量（包括char）的取值范围。默认为 signed

signed: 有符号。[负无穷，正无穷]

unsigned: 无符号。[0,正无穷]

## 7. 取值范围

### 1. 基本原理

CPU 能够识别的最小单位 bit （b，比特位）

内存的最小寻址单位 byte (B，字节)

1 B = 8b

![](E:\LocalBlog\images\C-xiaojiayu\C-Byte与bit.jpg)

bit 只能存放一个二进制数字，0或1。

### 2.补码

计算机是用补码来存储数据的。

![](E:\LocalBlog\images\C-xiaojiayu\正负数补码.jpg)

例如 -7  补码表示如下：

![](E:\LocalBlog\images\C-xiaojiayu\-7biaoshi.jpg)

### 3. 常见数据类型的取值范围

![](E:\LocalBlog\images\C-xiaojiayu\C-基本数据类型的取值范围.jpg)

![](E:\LocalBlog\images\C-xiaojiayu\浮点型取值范围.jpg)

### 4.Q&A

Q： 1字节能够存放最大的数是多少？

A：1B = 8b 二进制表示 11111111 ，转为10进制就是 2^8-1 



Q：如何确定 int 的取值范围？

A：根据字节来计算。sizeof(int) = 4 Byte

4 Byte = 4*8 bit => 转为10进制为 2^32-1 （公式为：2^n-1）

```C
    int int_result = pow(2,32)-1;
    printf("int_result：%d",int_result); // int_result：2147483647
```

但是，这里的程序计算是错误的  2^32-1 应该是等于 4,294,967,295。

为什么这里会出现错误呢？那是因为我们这里的 int_result  使用的默认 signed int。但是signed 左边第一位是符号位（正数为 0 ，复数为 1），实际只有 31 个 bit 。上面的程序表示的是 32 bit 即是 unsigned int 的最大值。

因此，int 应该只有 31 bit 程序如下：

```c
    int int_result = pow(2,31)-1;
    printf("int_result：%d",int_result); // int_result：2147483647
```

所以，int最大值为：2147483647，最小值为：-2147483648。

## 8. 字符与字符串

### 1. 字符

如下程序：

```C
int main(){
    char temp = 'C';
    printf("%c == %d\n",temp,temp);
    return 0;
}
// C == 67
```

可见，char 类型是一种特殊 int 型。

char 也分为 signed char 与 unsigned。但是默认为谁，由编译器确定。

### 2. 字符串

字符串本质是 char 数组

声明字符串： 

- char 变量名[大小]；
- char 变量名[大小] = {'','','',...}

**注意：字符串结尾需要加上 '\0'** ，因此我们在使用上述方法声明字符串时，需要在数组末尾加上 '\o'，同时数组大小也要加1。 

但是这样做很麻烦，因此我们也可以使用以下这种方式声明字符串：

- char 变量名[] = {'','','',....,'\0'};   // C会帮我们自动计算大小
- char 变量名[] = {"直接写字符串常量"}；// 不用写大小，不用写'\o'
- char 变量名[] = "直接写字符串常量"； `常用`

例如：

```c
    char my_name[] = {'S','k','y','z','c','\0'};
    printf("我的名字是：%s\n",my_name);

    char hh_name[] = {"Skyzc"};
    printf("我的名字是：%s\n",hh_name);

    char wow_name[] = "Skyzc";
    printf("我的名字是：%s\n",wow_name);
```



## 9. 运算符

### 01，算术运算符

![](E:\LocalBlog\images\C-xiaojiayu\算数运算符.jpg)



**求余只能对整数计算。**



**什么是单目、双目？**

目在这里指有多少个操作数，单个操作数叫单目，两个操作数叫双目。



### 02，运算符的优先级及结合性

### 03，类型转换

### 04，关系运算符

![](E:\LocalBlog\images\C-xiaojiayu\关系运算符.jpg)

用关系运算符将两边的变量、数据或表达式链接起来，称之为关系表达式。

关系表达式得到的一个值是一个逻辑值，即真或假。

**C 语言中，1表示 True 0表示 False**（非0 也为True）



### 05，逻辑运算符

![](E:\LocalBlog\images\C-xiaojiayu\逻辑运算符.jpg)

用逻辑运算符将两边的变量、数据或表达式链接起来，称之为逻辑表达式。

#### 001.短路求值

短路求值也称最小化求值，是一种逻辑运算符的求值策略。只有当第一个运算数的值无法确定逻辑的结果时，才对第二个运算数进行求值。

C 语言对`逻辑与`和 `逻辑或`采用短路求值的方式。

例如：

```c
#include <stdio.h>
//
// Created by skyzc on 2020/3/19.
//
int main(){
    int a = 3,b = 3;
    (a = 0) && (b = 5);  // 
    printf("a = %d,b = %d\n",a,b);
    (a = 1) || (b = 6);
    printf("a = %d,b = %d\n",a,b);
}

//a = 0,b = 3
//a = 1,b = 3
```

## 10. if分支

## 11.switch语句与分支嵌套

```c
	char ch;
    printf("请输入你的等级：");
    scanf("%c",&ch);
    switch (ch){
        case 'A':
            printf("你的成绩在90分以上！\n");
            break;
        case 'B':
            printf("你的成绩在90分到60分之间！\n");
            break;
        case 'C':
            printf("你的成绩在60分以下！\n");
            break;
        default:
            printf("请输入正确的等级！！！（A、B、C）\n");
            break;
    }
```

## 12.字符串处理函数

### 1. 获取字符串长度：strlen

strlen() 只包含字符个数，不包含最后的'\0'；

sizeof()表示整个字符数组的大小,包含最后的'\0'

例如：

```c
#include <stdio.h>
#include <string.h>
int main(){
    char str[] = "I name is Skyzc";
    printf("字符串尺寸为：%d\n",sizeof(str)); // 字符串尺寸为：16
    printf("字符串长度为：%u\n",strlen(str)); // 字符串尺寸为：15
    return 0;
}
```

### 2. 拷贝字符串：strcpy 和 strncpy

### 3. 链接字符串：strcat 和 strncat

### 4. 比较字符串：strcmp 和 strncmp

strcmp 原理是取字符串里每个字符的 ASCII 码相减，如果`两个字符串完全相等则返回0`，`不等则返回非0`。

strncmp 表示只取最前面N个字符对比。

## 13. 二维数组(矩阵)

### 1. 二维数组的定义

*类型 数组名 \[常量表达式][常量表达式]*

例：

```c
int array[4][5]; // 表示 4*5，4行5列
```

### 2. 二维数组的初始化

1. 由于二维数组在内存中是`线性存放`的，因此可以将所有的数据写在一个"{}"内。
   例如：

```c
int array[3][4] = {1,2,3,4,5,6,7,8,9,0,1,2}
```

2.  为了更直观的表示元素的分布，可以用大括号将每一行的元素括起来：

```c
int array[3][4] = {{1,2,3,4},{5,6,7,8},{9,0,11,12}}
```



## 21. 指针

指针其实是存的一个变量的起始地址，定义指针类型过后，程序才知道从这个地址中去几个字节的数据出来，怎么解码