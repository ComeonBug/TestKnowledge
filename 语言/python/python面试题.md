# 基础面试题：

## python是什么类型的语言

解释型 通用编程语言





## python是否区分大小笑

区分

python对大小写敏感、对缩进、格式敏感，最好使用4个空格，不建议使用tab键



## python数据类型有哪些，哪些可变哪些不可变

number、bool、string、list、set、tuple、dict

可变：list、dict、set

不可变：bool、number、string、tuple



## 列表和元组的区别

列表可变，元组不可变



## 数据和列表有什么区别

数组只能容纳一种数据类型

列表容纳任何数据类型

以及：pyhton中基本数据不包含数组，使用数组需要导入包：

```python
import array as arr

my_array1 = arr.array(['a','b','b'])
my_array1 = arr.array([1,2,3])
```



## python中的类型转化

类型转化就是把一种类型转化为另一个种类型

比如：number——》string，直接使用str(number)函数即可

int()函数：将其他类型转为number类型

float()函数：将其他类型转为float类型

ord()：将字符串转化为unicode

同样逻辑：set()、tuple()、list()、dict()



## 常见的操作符有哪些

in、is、not



## python中的局部变量和全局变量

局部变量：在函数内部声明的变量，生效范围也只有该函数内

全局变量：在函数外部声明的变量，是在全局空间中，项目中任何位置都可以使用该变量



## python中的变量作用范围



## python中函数是什么

函数是一段只有在调用时才生效的代码块，使用关键字def定义



## *args和 **kwargs是什么意思

如果我们在写程序是，无法确定参数的个数，就可以用 *arg和 **kwargs来说明｜占位，所以这里能看出，他们是位置参数，位置参数要写在参数列表的前面

*ages指的是：非键值对参数的参数列表，args是一个tuple类型

**Kwargs指的是：多个k-v键值对的参数列表，kwargs是一个dict类型

```python
def print_func(x,*args,**kwargs):
    print(x)
    print(args)
    print(kwargs)
    print(kwargs['y'])
print_func(1,2,3,'a',name='zhangsan',age='10',y='yy',z='zz')
```

补充：其实，* 就是拆包的概念



## break、continue、pass的区别

break：结束整个循环

continus：结束当前轮次的循环，进行下一轮次

pass：跳过，无任何实际作用，只是占位符



## [::-1]是什么含义

倒序输出一个列表

第一个冒号是切片的意思，[包含-切片起始index:不包含-切片终止index]，中间冒号分割，最后的-1是步长的意思，-1反向步长，即倒叙,ps：原始列表不变

步长需要和前面的切片顺序保持一致，比如：l[2,5,1] 和 l[5,3,-1]这俩都有结果，而l[2,5,-1]则是[]

```python
l = [1,2,3,4]
b = l[::-1]
print(b)  # [4,3,2,1]
print(l)  # [1,2,3,4]
```



## 如何随机化列表顺序

```python
from random import shuffle
l = ['a','b','c','d']
shuffle(l)
print(l)
```



## 如何生成随机数

random模块

```python
import random
random.random()  # [0-1)之间的一个随机数,如：0.3714299550740372
random.randrange(1,999)  # [1-999)之间的一个随机整数
```



## range和xrange的区别

range返回一个list对象，返回结果会生成一个静态数据

xrange返回一个xrange对象，返回结果是一个yield类型的生成器，可以减少内存损耗



## 如何使用python代码删除文件

```python
import os
os.remove('xxx.txt')
```



# 高级面试题

## 什么是python迭代器

迭代器：可以遍历或者迭代的对象

任意对象，只要定义了__ next __()方法，就是一个迭代器



## 什么是迭代

迭代：顾名思义，中文语义中，【迭代】，组词【更新迭代】，有 逐渐、挨个的含义

所以python中迭代的意思可以理解为【以此取出｜遍历元素的过程】

当我们循环遍历一个对象时，这个过程本身就叫做迭代



## 什么是python生成器

返回可迭代项目集的函数，称为 生成器

【生成器】 是一种 【迭代器】

特别的点在于，【生成器】只能迭代一次



## 什么是init

init是类的初始化方法，一般用于创建类的对象或者实例时，默认调用该方法，用来为该对象｜实例分配内存，初始化等操作，常见写法为:

```python
class Student:
    def __init__(self,name):
        self.name = name
a = Student('张三')
print(a.name)
```



## 什么是lambda函数

lambda是python的匿名函数，顾名思义，即：没有名字的函数

使用简单的一行lambda代替一个函数的功能

常见使用方法为：

下面例子中，lambda函数和add()的效果是一样的， 但是lambda一行代码即可完成此功能

对于一些只用到很少次数，功能简单的函数可以用lambda代替

```python
def add(x,y):
  return x+y
a = lambda x,y:x+y
print(a(1,2))
```



## python中的self是什么

‘self’即‘自己’，即类的对象 或者 实例

类中实例方法的第一个参数必须为self

```python
class Student:
    name = ''
    def set_stu_name(self,name):
        self.name = name
a = Student()
a.set_stu_name('张三')
print(a.name)
```



## python的异常处理

try-except-else



## PYTHONPATH是什么

pythonpath：一个环境变量

当我们导入python模块的时候，会查找pythonpath

通过pythonpath来检查目录是是否有要导入的模块

python解释器 通过 pythonpath 来决定 导入哪个模块



## 什么是python模块，列举几个常用内置模块

python模块就是包含了python代码的文件，可以是函数、也可以是类，必须含.py文件

常见的几个内置模块：

操作系统os、数学操作math、系统sys、随机random



## python内存管理机制



## python中如何管理内存

由python专用堆空间管理，所有的对象和数据结构都存在 【python堆空间】里

【python内存管理器】 来分配 【python堆空间】

【垃圾收集器】 回收未使用的内存， 回收完后的内存给 【python堆空间】使用





## python垃圾回收机制



## python中如何实现多线程

GIL锁