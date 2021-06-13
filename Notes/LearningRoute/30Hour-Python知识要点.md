#### 1.Python 常见数据类型

> 值、字符串、列表(list)、元组(tuple)、集合(set)、字典(dictionary)

- Python 中没有数组

- list 列表: 元素数据类型可不同，元素可以重新赋值。a[1] = "我爱Python"

  ```python
  # 创建新列表 
  list = [123,"abc",'123']
  # 列表赋值
  list[1] = "python"
  # list = [123,'python',123]
  # 列表取值
  list[0]
  ```

- tuple元组：元素数据类型可不同，元素不可重新赋值

  ```python
  tup1 = ('Google', 'Runoob', 1997, 2000)
  ```

- dictionary 字典:  取值格式：字典名["对应键名"]

  ```python
  # 创建字典
  dict = {"name":"Skyzc","age":"21","gender":"man"}
  dict1 = { 'abc': 123, 98.6: 37 }
  # 取值
  dict["name"]
  # 赋值
  dict["name"] = "newSkyzc"
  # 删除键
  del dict['name']
  # 清空字典
  dict.clear()
  # 删除字典
  del dict
  ```

  

- set 集合：常用于去重；并且可以求集合与集合的交集、并集、差集。例如：

  ```python
  
  e = set("sdfasdfadf")
  f = set("sdfasdfsdfgert")
  #{'r', 'e', 'g', 't'}
  ```

  

#### 2.流程控制

##### for

```python
# 遍历列表
list1 = ["aa",'sdf','asdf']
for i in a：
	print(i)
    
for i in range(0,10):
    print(i)
```

- 关于 for 循环你中 range() 函数的使用

  ```python
  range(stop)
  range(start, stop[, step])
  ```

  start: 计数从 start 开始。默认是从 0 开始。例如range（5）等价于range（0， 5）;
  stop: 计数到 stop 结束，但`不包括 stop`。例如：range（0， 5） 是[0, 1, 2, 3, 4]没有5
  step：步长，默认为1。例如：range（0， 5） 等价于 range(0, 5, 1)

  指定的结束值永远不会包括它本身：

  ```python
  for i in range(10,30,5):
      print(i)
  # result: 10 15 20 25
  ```

   当有两个参数时，第一个参数表示起始值，第二个参数表示结束值，步长默认为1 ：

  ```python
  for i in range(1,9):
      print(i)
  # 1 2 3 4 5 6 7 8
  ```

  

中断结构

break 全部退出

continue 跳出一次循环

#### 3.Python的模块

模块导入的两种方式：

```python
import 模块名
from 模块 import 方法
```

模块的类别

1. 自带模块

2. 第三方模块

   - pip (pip install ***)

   - whl (下载安装:[lfd.uci.edu]( https://www.lfd.uci.edu/~gohlke/pythonlibs/#wordcloud ))

     找到需要的包下载，下载后为***.whl

     ```bash
     pip install ***.whl
     ```

   -  直接复制（复制到 Python安装目录/lib/ ）

   - anaconda

3. 自定义模块

#### 4. 文件操作

``` python
open(name,mode,buffering，encodeing="utf-8")
```


- name 文件路径

- mode 打开文件的模式，只读、写入、追加等。常用 mode：

  w:写入

  r:读取

  b:二进制

  a+:追加

#### 5.异常处理

```python
try:
    可能会出异常的程序
except Exception as 异常名称:
    处理异常
```

#### 6. 面向对象

##### 类的创建与实例化

##### 构造方法

```python
class cl2:
    def __init__(self):
        print("Im cl2 self")
    def main(self):
        print("Im main())
```

##### 类属性

```python
在 Python 中 实例属性与静态属性。实例属性 就是在构造方法里的参数
class cl3:
    myname = "Skyzc-NNNNN"
    def __init__(self,name,job):
        self.name = name
        self.job = job
```

##### 类方法

```python
# 类方法必须带 self 参数
```

##### *继承

```python
class Animal:
    def eat(self):
        print("动物能吃东西")
class Dog(Animal):
    pass
```

#### 7. 正则表达式

##### 01.原子

- 普通字符作为原子

- 非打印字符作为原子 （例如  \n 换行符 \t 制表符 ）

- 通用字符作为原子    

  \w 字母、数字、下划线

  \W 除字母、数字、下划线

  \d 十进制数字

  \D 除十进制数字

  \s 空白字符

  \S 除空白字符

- 原子表

##### 02.元字符

. 除换行符外任意一个字符

^ 匹配开始位置

$ 匹配结束位置

\* 前面原子出现 0次1次或多次

? 前面原子出现 0次1次

\+ 前面原子出现1次或多次

{n} 恰好 n 次

{n,} 至少n次

{n,m} 至少n，至多m

| 模式选择符 或

() 模式单元

#### 8.Urllib基础

###### 常用方法

getcode()

geturl()

###### 超时设置



