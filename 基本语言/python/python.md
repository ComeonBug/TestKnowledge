# 基础知识

我为什么选择python？

1.入门简单，快速上手，易于测试组内提升

2.生态丰富

3.轻量级，简单几行代码完成脚本

4.为了未来做准备：大数据处理测试、可能还有AI相关

5.格式优美

## 数据类型

python基础数据类型：7大基本数据类型

​	数字、字符串、字典、列表、元组、集合文件

字符串常用方法：format、split、join、find、replace、count、index、lower、upper、strip

列表常用方法：append、extend、insert、count、sort、reverse、index、clear、pop、remove、copy

sort：升序  reverse：降序

引申：copy、deeepcopy：

copy：本身的改变不影响拷贝对象，但是本身对象里包含的可变对象的改变会影响

deepcopy：完全的copy的

面试题：

列表去重

```python
# 列表去重
a=[1,2,5,6,1]
# b=list(set(a))
b=[]
for i in a:
  if i not in b：
  	b.append(i)
# b = list({}.fromkeys(a).keys())
```

列表降序排列：

```python
a=[3,1,7,4,2,7,9]
a.sort(reverse=True)
a.reverse()
```

字典（键值对）常用方法：get、keys、values、items、fromkeys、pop、clear、popitem、setdefault、update

字段的key是哈希的

集合（无序、不可重复）常用方法：add、union、clear、pop、difference、intersection、remove

元组（不可变更）常用方法：index、count

文件：file1=open('test.txt','r')





### 进阶

1.赋值是传输对象，既非单传值也非单传址

Python参数传递采用的是“传对象引用”的方式

不可变对象：传值

可变对象：传址

引申：赋值、复制（深拷贝、浅拷贝）、偏函数

经典问题：

```python
class buy:
    def __init__(self, people, goods=[]):
        self.people = people
        self.goods = goods

    def buy(self, good):
        self.goods.append(good)

    def pay(self):
        print(self.goods)


if __name__=='__main__':
    custom1 = buy('tom')
    custom2 = buy('jerry')

    custom1.buy('apple')
    custom2.buy('banana')

    custom2.pay()
   # 此时的结果是['apple', 'banana']
   # 出问题的原因是在：【def __init__(self, people, goods=[])】这句设置的默认值上面
   # 因为列表是可变类型，goods分别赋值给custom1的self.goods和custom2的self.goods，所以其实这俩的实例变量在内存中的地址就是goods这个变量的地址，所以最后打印出来的不对
```

正确的写法：

```python
class buy:
    def __init__(self, people, goods=None):
        self.people = people
        if goods:
            self.goods = goods
        else:
            self.goods = []

    def buy(self, good):
        self.goods.append(good)

    def pay(self):
        print(self.goods)


if __name__=='__main__':
    custom1 = buy('tom')
    custom2 = buy('jerry')

    custom1.buy('apple')
    custom2.buy('banana')

    custom2.pay()
```

2.LEGB

3.可变类型与不可变类型

4.扁平序列与容器序列

​	扁平序列：字符串（有序，不能容纳）

​	容器序列：列表、元组（有序、能容纳其它类型）

​	以后再遇到【列表与元组的区别】这样的面试题就可以这样回答：

​	【他们都是容器序列，是有序的且能容纳其它类型，列表是可变类型，元组是不可变类型，但由于元组可以容纳其它类型，所以如果它容纳了可变类型的话，元组也是可能发生变化】

5.其它数据类型：比如collection中提供了其他的数据类型：namedtuple、双队列、counter

```python
from collections import namedtuple

Point = namedtuple('Point',['x','y','z'])
p1 = Point(1,2,3)
p2 = Point(4,5,6)
print(p1.x)
print(p2.x)
```

6.变量的内部基础关系与魔术方法初步：

变量的内部继承关系：

https://docs.python.org/zh-cn/3/library/collections.html

https://docs.python.org/zh-cn/3/library/collections.abc.html

学习变量的方法：

父类方法学会，子类记得继承了哪些父类就行



## 逻辑控制：

if、while、for、break、contine

for其实是挨个迭代，和其他语言中不一样





## 高级表达式

列表推倒式：

```python
l=[i*2 for i in range(1,101,2)] 
```

lambda表达式：

map

filter

reduce

## 异常

```python
try:
  ***
except Exception as e:
  ***
else:
  # 没有异常时执行
  ***
finally:
  # 无论如何都会执行
  ***
```

```python
class MyException(Exception):
  def __init__(Exception):
    pass
raise MyException
```

```python
raise Exception('exception msg')
```



常见的异常：

ZeroDivisionError、IndexError、TypeError、KeyError、AttributeError、AssertionError、NoSuchElementException

# 面向对象

重写、重载、继承、多态

子类如何调用父类方法：super().方法名()

python支持多继承

类方法、实例方法、类变量、实例变量

## 函数

定义方式：

```python
def fun():
  pass
```

函数叫“可调用对象”

​	fun是函数对象

​	fun()是调用函数

ps：如果想让一个类也能通过【类名()】这样的方式调用的话，这个类需要实现call魔术方法

```python
class ClassFun:
    def __call__(self):
        return 123


cf = ClassFun()
print(cf)
print(cf())
cf2 = cf
print(cf2())
# 结果：
# <__main__.ClassFun object at 0x7fb8bcf9ae20>
# 123
# 123
```

5种函数参数：可选参数、默认参数、可变参数、关键字参数、命名关键字参数

顺序：必选、默认、可变、关键字、命名关键字

可变参数：

​	*args：获取其它参数

​	**kwargs：获取关键字参数

```python
def fun(*args, **kwargs):
  print(args)
  print(kwargs)

fun(1,2,'a',name='tom',age=18)
# 结果为：
# (1, 2, 'a')
# {'name': 'tom', 'age': 18}
```



## 高阶函数

定义：参数是函数或者返回值是函数

常见的高阶函数：map、reduce、filter



## 偏函数

定义：固定函数的一个参数

```python
import functools


def add(x, y):
    print(f'x:{x}')
    print(f'y:{y}')
    return x+y


add_1 = functools.partial(add, 1)
res = add_1(10)
print(f'res:{res}')
add_2 = functools.partial(add,5)
res2 = add_2(10)
print(f'res2:{res2}')

# 结果：
# x:1
# y:10
# res:11
# x:5
# y:10
# res2:15
```





函数返回值：

无返回值、return、yield

如果返回值是函数的话，就是闭包

闭包是装饰器的原理



## 装饰器

闭包：延伸了作用域的函数

@ 这个符号叫【语法糖】

```python
# 装饰器写法
def decoratefun1(fun):
    print('decorate running')

    def innerfun():
        return fun()

    return innerfun


@decoratefun1
def fun1():
    print('fun1')


fun1()
```

```python
# 非装饰器写法
def decoratefun2(fun):
    print('decorate running')

    def innerfun():
        return fun()

    return innerfun


def fun2():
    print('fun2')


fun2 = decoratefun2(fun2)
fun2()

```

上面两种写法的结果是一样的，但是执行的过程不一样，第一种使用装饰器在fun1函数定义的时候decoratefun1函数已经被调用了，而第二种非装饰器的方式，在函数fun2定义时decoratefun2函数没有被调用，验证的方法是注释掉【fun1()】【fun2 = decoratefun2(fun2)】【fun2()】这三句，然后发现运行第一种方式的时候，【decorate running】打印出来了，而第二种则没有打印



装饰器在导入时回自动执行，如下两段代码证明：

```python
# 这是decoratetest.py文件
def decoratefun(fun):
    print('decorate running')

    def innerfun():
        return fun()

    return innerfun


@decoratefun
def fun1():
    print('fun1')
```

```python
# 在python交互模式中输入【import decoratetest】,打印出了【decorate running】
>>> import decoratetest
decorate running
```



PEP 318 引入了【装饰器】

PEP 3129 引入了【类装饰器】

* PEP是什么【https://www.biaodianfu.com/python-pep.html】







装饰器的作用，比如：

绑定、包装、关键功能的额外的设置、验证等





被装饰函数带参数

被装饰函数带可变参数

被装饰的函数有返回值



- 装饰器带参数

  

```python

```





装饰器堆叠（一般外层是嵌套系统定义的，比如：@classmethod）



系统内置的装饰器：



【functlools.wraps()】这个wraps这个函数装饰器(实际是执行partial方法)，用在包装器函数发起时调用，可以保留被装饰函数的性质，比如

```python
foo.__name__
```



wraps常见的使用场景：加日志

```python
from functools import wraps
import  datetime

def logit(func):

    @wraps(func)
    def wraps_fun(*args, **kwargs):
        print(f'{datetime.datetime.now()}:{func.__name__} start called')
        func(*args, **kwargs)
        print(f'{datetime.datetime.now()}:{func.__name__} end called')
    return wraps_fun

@logit
def myfun():
    sum = 0
    for i in range(100000):
        sum = sum * i


myfun()
```



但是有一个缺点，就是不需要log的时候没法全部去除，不如logging模块好用



另一个使用到的高级的装饰器：

functools.lru_cache(maxsize = 128, typed = False)

maxsize:设置缓存的内存占用值，超过这个值以后，结果会被释放

typed:如果是true，则会把不同的参数类型得到的结果分开保存 

```python
import functools
import timeit

@functools.lru_cache()
def fibonacci(n):
    if n<2:
        return n
    return fibonacci(n-1)*fibonacci(n-2)

if __name__=='__main__':
    print(timeit.timeit('fibonacci(6)', setup='from __main__ import fibonacci'))
```

1.不加【@functools.lru_cache()】，运行时间为：4.288

2.加上【@functools.lru_cache()】，运行时间为：0.087

3.加上【@functools.lru_cache()】，运行时间为：0.273



类装饰器（类当中的函数的装饰器）

```python
from functools import wraps

class Myclass(object):
    def __init__(self, var = 'init_var', *args, **kwargs):
        self._v = var
        super(Myclass, self).__init__(*args, **kwargs)
    
    def __call__(self, func):
        # 类装饰器：类的函数装饰器
        @wraps(func)
        def wrapped_function(*args, **kwargs):
            func_name = func.__name__
            print(func_name,' been called')
            return func(*args, **kwargs)
        return wrapped_function
    

def my_fun():
    pass


Myclass(100)(my_fun)()
```



装饰类

```python
# 装饰类
def decorator_class(aClass):
    class newClass(object):
        def __init__(self, args):
            self.times = 0
            self.wrapped = aClass(args)
        
        def display(self):
            self.times += 1
            print('run times:',self.times)
            self.wrapped.display()
    return newClass

@decorator_class
class MyClass(object):
    def __init__(self, number):
        self.number = number
    
    def display(self):
        print('number is ', self.number)


num = MyClass(6)
for i in range(5):
    num.display()
```





应用1:为函数添加属性

```python
def attr(**kwargs):
    def decorate(f):
        for k in kwargs:
            setattr(f, k, kwargs[k])
        return f
    return decorate


@attr(version='1.0', authod='bule')
def mymethod():
    pass

print(getattr(mymethod,'__name__'))
print(getattr(mymethod,'version'))
print(getattr(mymethod,'authod'))
```



应用2:函数参数观察器

```python
def trace(f):
    def decorate_funciton(*args, **kwargs):
        print(f, args, kwargs)
        res = f(*args, **kwargs)
        print(res)
    return decorate_funciton

@trace
def myfun(greet, name):
    return f'{greet},{name}'

myfun('hello','Tom')
# 打印结果为：
# <function myfun at 0x7f917526d670> ('hello', 'Tom') {}
# hello,Tom
```



应用3:单例



python自带的装饰器:

```python
@classmethod
@statcimethod
@property
@*.setter
@*.deleter
```



举例：

@dataclass：判断对象是否相同（python3.7）

```python
from dataclasses import dataclass

@dataclass
class Myclass(object):
    var_a : str
    var_b : str


obj1 = Myclass('x','y')
obj2 = Myclass('x','y')
print(obj1 == obj2)
```

如果不用dataclass装饰器的话，实现代码为：

这里可以体会到装饰器的用处：把判断、封装、验证、数据库操作 等可以丢到装饰器里



```python
class Myclass(object):
    def __init__(self, var_a, var_b):
        self.var_a = var_a
        self.var_b = var_b
    
    def __eq__(self, other):
        if self.__class__ != other.__class__:
            return False
        return (self.var_a, self.var_b) == (other.var_a, other.var_b)


obj1 = Myclass('x','y')
obj2 = Myclass('x','y')
print(obj1 == obj2)
```





## 魔术方法：

```python
# __sub__:就是减号
# 下面例子是用来求欧氏距离
from collections import namedtuple
from math import sqrt

Point = namedtuple('Point',['x','y','z'])


class Vector(Point):
    def __init__(self, p1, p2, p3):
        super(Vector).__init__()
        self.p1 = p1
        self.p2 = p2
        self.p3 = p3

    def __sub__(self, other):
        tmp = (self.p1 - other.p1) ** 2 + (self.p2 - other.p2) ** 2 + (self.p3 - other.p3) ** 2
        return sqrt(tmp)

p1 = Vector(1,2,3)
p2 = Vector(4,5,6)
print(p1-p2)
```







```python
__getAttr__
__get__
__getattr...
__eq__
__init__
__setattr__

```





## 常用的库：

pytest、requests、selenium、appium、jsonpath、jsonschema、allure、pymysql、pymongo、pyyaml、hamcrast、threading、pytest-sdist、logging、os、subprocess

特殊的数据类型：链表

## 常见的几个算法

1.冒泡排序：

```python
    # 遍历所有数组元素
    for i in range(n):
 
        # Last i elements are already in place
        for j in range(0, n-i-1):
 
            if arr[j] > arr[j+1] :
                arr[j], arr[j+1] = arr[j+1], arr[j]
```



2.快速排序





## 类

类定义建议写法，最好明确写出继承Object的新式类，这样兼容性比较高：

```python
class Myclass(Object):
  pass
```



## 面试题：

*args、**kwargs有什么区别：

他俩都是可变参数，*args参数需要写在**kwargs之前，不然程序报错

*args：其他非关键字参数，接受0-多个参数，args是一个元组

**kwargs：关键字参数，接受多个参数，但是参数时键值对出现

```python
def test_arg(first,*args,**kwargs):
  pass
test_arg(1,2,3,4,a=5,b=6)
# *args:接收:(2,3,4)
# **kwargs:{'a':5,'b'=6}
```









迭代器、生成器、装饰器

反射

原语





原理：

1.python垃圾回收机制

2.python内存管理机制





bs4和lxml功能类似

bs4是基于DOM解析的，所以比较慢

lxml基于C写的，性能好

xpath：



# 补充知识点：

## LEGB

LEGB（先查L函数内部，L没有就查E外部函数，如果E没有就查G模块内的全局变量，如果E没有就查B python内置的）

L（local function）：函数内的名字空间

E（Enclosing function locals）：外部嵌套函数的名字空间

G（Global module）：函数定义所在模块（文件）的名字空间

B（Builtin Python）：Python内置模块的名字空间

```bash
# 查看B有哪些
>>> print(dir(__builtins__))
['ArithmeticError', 'AssertionError', 'AttributeError', 'BaseException', 'BlockingIOError', 'BrokenPipeError', 'BufferError', 'BytesWarning', 'ChildProcessError', 'ConnectionAbortedError', 'ConnectionError', 'ConnectionRefusedError', 'ConnectionResetError', 'DeprecationWarning', 'EOFError', 'Ellipsis', 'EnvironmentError', 'Exception', 'False', 'FileExistsError', 'FileNotFoundError', 'FloatingPointError', 'FutureWarning', 'GeneratorExit', 'IOError', 'ImportError', 'ImportWarning', 'IndentationError', 'IndexError', 'InterruptedError', 'IsADirectoryError', 'KeyError', 'KeyboardInterrupt', 'LookupError', 'MemoryError', 'ModuleNotFoundError', 'NameError', 'None', 'NotADirectoryError', 'NotImplemented', 'NotImplementedError', 'OSError', 'OverflowError', 'PendingDeprecationWarning', 'PermissionError', 'ProcessLookupError', 'RecursionError', 'ReferenceError', 'ResourceWarning', 'RuntimeError', 'RuntimeWarning', 'StopAsyncIteration', 'StopIteration', 'SyntaxError', 'SyntaxWarning', 'SystemError', 'SystemExit', 'TabError', 'TimeoutError', 'True', 'TypeError', 'UnboundLocalError', 'UnicodeDecodeError', 'UnicodeEncodeError', 'UnicodeError', 'UnicodeTranslateError', 'UnicodeWarning', 'UserWarning', 'ValueError', 'Warning', 'ZeroDivisionError', '_', '__build_class__', '__debug__', '__doc__', '__import__', '__loader__', '__name__', '__package__', '__spec__', 'abs', 'all', 'any', 'ascii', 'bin', 'bool', 'breakpoint', 'bytearray', 'bytes', 'callable', 'chr', 'classmethod', 'compile', 'complex', 'copyright', 'credits', 'delattr', 'dict', 'dir', 'divmod', 'enumerate', 'eval', 'exec', 'exit', 'filter', 'float', 'format', 'frozenset', 'getattr', 'globals', 'hasattr', 'hash', 'help', 'hex', 'id', 'input', 'int', 'isinstance', 'issubclass', 'iter', 'len', 'license', 'list', 'locals', 'map', 'max', 'memoryview', 'min', 'next', 'object', 'oct', 'open', 'ord', 'pow', 'print', 'property', 'quit', 'range', 'repr', 'reversed', 'round', 'set', 'setattr', 'slice', 'sorted', 'staticmethod', 'str', 'sum', 'super', 'tuple', 'type', 'vars', 'zip']
```



```python
# L E G
x = 'global'

def fun1():
    x = 'enclosing'

    def fun2():
        x = 'local'
        print(x)

    fun2()

fun1()
print(x)
# 结果：
# local
# global
```



为什么要知道LEGB：

1.同名不同作用域

2.变量查找顺序

有层次了之后就有了域

这里也引申到了另一个概念【闭包】，闭包中的作用域也是要注意的

## 闭包：

python的【一切皆对象】理念在【闭包】这个概念中体现的淋漓尽致





# 什么时候才是学会python了？

1.有了自己的学习模型，开始不断完善、升级、甚至是替换成第三方的库

2.清晰的知道python的长处，哪些事情适合python去做，哪些事情不适合python去做？

3.深刻理解python的特效

4.官网文档必须玩的6

5.常阅读Git上好的项目

6.好的问题是成功的一半

7.好的风格（PEPE9）



# Tips

-O：这个选项会让所有的assert失效









