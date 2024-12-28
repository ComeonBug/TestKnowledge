# 基础面试题：

## python是什么类型的语言

解释型 通用编程语言

## python是否区分大小笑

区分

python对大小写敏感、对缩进、格式敏感，最好使用4个空格，不建议使用tab键

## python数据类型有哪些，哪些可变哪些不可变

python一共有8大数据类型：int、float、bool、string、list、set、tuple、dict

可变：list、dict、set

```python
# list
l = [1,1,'a']
# dict
d = {'a':1,'b':1}
# set,元素不可重复
s = {'a','b',1}
```

不可变：bool、number、string、tuple

```python
# bool
b = False
b1 = 0
# int
i = 1
# float
f = -1.1
# string
s = '1'
# tuple，元素可重复，一旦创建就不能变了
t = (1,'a','1')
```

## 列表和元组的区别

列表可变，元组不可变

但是其实元组也是可以做成可变的

只要元组的元素是可变的就行，比如一个元组里的元素是列表，这个列表变，这个元组也就会变了



## 数组和列表有什么区别

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



## %和/的区别

% 取余 

/ 除法

```  python
a = 1/2  #a=0.5
b = 1%2  #b=1
```



## equal和==的区别

python中==是调用对象的__ eq __ () 方法，equal是对象定义的一个方法，他们对于比较的内容视各自定义的情况而定，如果没有显式定义__ eq __ ()方法比较的是内存地址，即==比较的是内存地址





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
# print_func.py
def print_func(x,*args,**kwargs):
    print(x)
    print(args)
    print(kwargs)
    print(kwargs['y'])
print_func(1,2,3,'a',name='zhangsan',age='10',y='yy',z='zz')
```

```shell
$ python print_func.py
1
(2, 3, 'a')
{'name': 'zhangsan', 'age': '10', 'y': 'yy', 'z': 'zz'}
yy
```

补充：其实，* 有点拆包的概念

## python的拆包

顾名思义，就是把一个【包】拆开

这个包，在python里类似这样的类型是什么呢？就是：列表、元组、集合

```python
l = [1,2,3]
a,b,c = l #这就是拆包，把 l 这个包拆开，每个元素放在对应的变量里,a=1,b=2,c=3
*d,e=l #这也是拆包，把 l 这个包拆开，每个元素放在对应的变量里,而【*d】中【*】代表不确定位数，那就是除去e这个固定的，其余的就是d，所以d=[1,2],e=[3]
t = ('a','b')
k,v = t   #这就是拆包，把 t 这个包拆开，每个元素放在对应的变量里,k='a',v='b'
s = {1,'a'}
f,g = s  #这就是拆包，把 s 这个包拆开，每个元素放在对应的变量里,g='1',g='a'
```



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



## 如何反转一个列表

这个题有点意思

可以问问面试官是否修改原列表

如果是原地修改，只有一个方法：列表自身的reverse方法

```python
li = [1,2,3,4]
li.reverse()
print(l)
```

如果不需要原地修改，方法就很多了，可以切片，可以range+列表推导式

```python
# 方法1：切片
li = [1,2,3,4]
li_new = li[::-1]
# 方法2：range+列表推导式
li_new2 = [li[i] for i in range(len(li) - 1, -1, -1)]
```



## python有哪些推导式

推导式格式：

[表达式 for i in 迭代器]

注意，其中【迭代器】，只能有1个迭代器，因为只有一个迭代器生效，具体可以看下面字典推导式里的例子

### 列表推导式

通过一行代码构建一个列表，比通常的for循环更简洁

```python
x = [i*2 for i in [1,2,3,4] if i%2==0] # x=[4,8]
```

### 集合推导式

```python
x = {i*2 for i in [1,2,3,4] if i%2==0} # x={4,8}
```

### 字典推导式

```python
x = {i:i*2 for i in [1,2,3,4] if i%2==0} # {2: 4, 4: 8}
# 注意，我在编写文档的过程中，写了下面一个语句
# x = {j:i*2 for j in ['a','b'] for i in [1,2,3,4]}
# 上面的语句的结果：x={'a': 8, 'b': 8}
# 所以注意，如果我们的字典推导式中，元素个数看的是第一个变量，即再看下面的语句，以及推导式里无法嵌套多个迭代器
# x = {j:i*2 for j in ['a','b','c','d'] for i in [1,2,3,4]} x最后的结果是{'a': 8, 'b': 8, 'c': 8, 'd': 8}
```

### 生成器推导式

```python
x = (i**i for i in range(3))
print(x)   # <generator object <genexpr> at 0x7fcacca84510>
```

注解：生成器用()来表示

引申：range(2)返回一个range对象，不是一个生成器，但是在for循环里，可以当作生成器用

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



# 高级面试题（每个问题都差具体的代码示例）

## 什么是python迭代器

迭代器：可以遍历或者迭代的对象

任意对象，只要实现了__ iter __ 方法 和 __ next __()方法，就是一个迭代器

__ iter __ 方法 返回一个特殊的迭代器对象，这个迭代器对象实现了 __ next __()方法，用于返回序列里的下一个元素

```python
class RangeIterator:
    def __init__(self, start, end):
        self.current = start
        self.end = end

    def __iter__(self):
        # 返回迭代器对象本身
        return self

    def __next__(self):
        # 当前值如果小于end，则返回当前值，并更新当前值为下一个值
        if self.current < self.end:
            num = self.current
            self.current += 1
            return num
        # 如果当前值已经达到或超过end，则停止迭代并抛出StopIteration异常
        else:
            raise StopIteration

# 使用自定义迭代器
start, end = 0, 5
range_iterator = RangeIterator(start, end)

for num in range_iterator:
    print(num)
```



## 什么是迭代

迭代：顾名思义，中文语义中，【迭代】，组词【更新迭代】，有 逐渐、挨个的含义

所以python中迭代的意思可以理解为【以此取出｜遍历元素的过程】

当我们循环遍历一个对象时，这个过程本身就叫做迭代

## 什么是python生成器

返回可迭代项目集的函数，称为 生成器

【生成器】 是一种 【迭代器】

特别的点在于，【生成器】只能迭代一次

生成器在函数中使用yield实现，每次迭代是，生成器函数都会在yield表达式处暂停，下次迭代从该处开启

```python
def range_generator(start, end):
    """创建一个生成器，用于生成从start到end（不包括end）的整数"""
    i = start
    while i < end:
        yield i
        i += 1

# 使用生成器表达式
start, end = 0, 5
gen = range_generator(start, end)

for num in gen:
    print(num)
```



## 什么是python的装饰器

python装饰器是一个设计模型，旨在不修改原函数的情况下，更改原函数的逻辑

装饰器的实现：

1、要有一个装饰函数,假设为A，这个装饰函数接收的参数是原函数B，返回一个更改了逻辑的函数C的名字，注意不加()

2、装饰器函数里定义C函数，这个函数的入参固定为*args和**kwargs，C函数里写更改后的逻辑，当然其中包含了调用原B函数

3、在原函数的头上加上 @装饰函数名A，注意，也不加()

4、装饰器函数要写在被装饰函数的前面

举例：

A时一个自动化用例，判断接口调用是否正常，想要加一个装饰器函数，装饰器函数的功能时如果A函数返回校验失败的话，尝试3次，3次都失败则抛出异常

decorators.py文件

```python
# decorators.py
# 自定义装饰器，失败重试n次，默认n=3
def retry(func, n=3):
    def wrapper(*args, **kwargs):
        case_flag = 0
        for i in range(n):
            try:
                func(*args, **kwargs)
                break

            except AssertionError:
                case_flag += 1
                print('第' + str(case_flag) + '次重试')
                continue
        if case_flag == 3:
            assert False
        else:
            assert True
    return wrapper
```

test文件

```python
import decorators


class Test_a():
    @decorators.retry
    def test_a(self):
        res = {'code':100}
        assert res['code'] == 200
```



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

try-except-else-finally

try：执行的语句，捕获这句语句执行时是否有异常发生

except：后面跟上要捕获的异常，可以是具体异常名（如：ZeroDivisionError）也可以是所有异常（Exception），捕获异常以后执行里面的语句快

else：没有异常发生时，接着执行的语句快

finally：不论有没有异常发生，都执行的语句快



```python
try:
    # 尝试执行的代码
    result = 10 / 0
except ZeroDivisionError:
    # 如果发生了 ZeroDivisionError，则执行这里的代码
    print("不能除以零！")
else:
    # 如果没有异常发生，则执行这里的代码
    print("除法成功执行。")
finally:
    # 无论是否发生异常，都会执行这里的代码
    print("这是 finally 块，总会被执行。")
```

```python
try:
    # 可能会引发任何类型的异常的代码
    result = 1 / 0
except Exception as e:
    # 捕获所有非系统退出类的异常
    print(f"发生了一个异常：{e}")
```



## PYTHONPATH是什么

pythonpath：一个环境变量

当我们导入python模块的时候，会查找pythonpath

通过pythonpath来检查目录是是否有要导入的模块

python解释器 通过 pythonpath 来决定 导入哪个模块，是导入默认的还是我们自己写的，可以自己写包，然后把路径添加到pythonpath中，导入的就是我们自己写的包了

在大型项目开发时，pythonpath也可以定义项目依赖的路径，使得项目可以在不同的机器下运行而不需要重新配置环境变量



## 什么是python模块，列举几个常用内置模块

python模块就是包含了python代码的文件，可以是函数、也可以是类，必须含.py文件

常见的几个内置模块：

操作系统os、数学操作math、系统sys、随机random、re、time、pytest、selenium、request、allure、yaml、json、logging、SSHLibrary





## python内存管理机制

内存管理主要是在这3部分：内存的分配、使用、回收



内存的分配：python解释器的【python内存管理器】会自动进行内存的分配

​	**pymalloc**：python私有内存分配器，适用于小型对象的分配

​	malloc：系统分配器，适用于大型对象

​	pool：内存池，一个预分配的内存区域，用于存储特定大小的对象，可以减少内存碎片，提升内存分配效率，python为每种数据类型维护了内存池，这些内存池根据对象的大小进行划分



内存的使用：注意内存泄漏，即对象的引用计数没有正确减少，导致对象无法被垃圾回收

​	内存的优化：使用生成器而不是列表、使用collection模块中的优化数据类型、weakref模块创建弱引用，避免循环引用



## python中如何管理内存

由python专用堆空间管理，所有的对象和数据结构都存在 【python堆空间】里

【python内存管理器】 来分配 【python堆空间】

【垃圾收集器】 回收未使用的内存， 回收完后的内存给 【python堆空间】使用





## python垃圾回收机制

2种机制

第一种：引用计数机制

当一个对象被创建后，其引用计数计为1，当对象被引用后，计数+1，当引用被删除后，计数-1，当计数为0后，python解释器调用该对象的__ del __方法，释放该对象所占有的内存，该内存被回收

但是如果遇到循环引用的情况，计数机制无法做到垃圾回收，需要gc垃圾回收器来清理

以及，像整数这样的不可变内置类型，是特殊的，通过【整数对象缓存】技术来实现

python会选择一定范围的整数，如-5～256，python会事先缓存这个范围内的整数在内存中，如果新建这个范围内的整数对象时，不会新开内存，会重用已建好的整数对象

字符串、元组类似



第二种：分代收集机制

这个机制是假定新一代比老一代更快被回收，这里根据创建对象的时间分为不同的【代】

新创建的对象为第0代，第一次垃圾回收后仍存活的对象，为1代，以此类推

先回收新一代的对象内存，然后再回收老一代的



## collections模块中的优化数据结构



## python中如何实现多线程



## python进程、线程、协程的区别



## python进程之间如何通信



## python套接字



## python GIL锁



## 反射



## 元编程



# pytest

## 常用的一些装饰器

Pytest 是一个流行的 Python 测试框架，它提供了多种装饰器来增强测试用例的功能。根据提供的搜索结果，以下是一些常用的 pytest 装饰器及其功能：

1. **`@pytest.fixture()`**
   - 功能：用于创建测试夹具（fixture），可以提供测试用例所需的资源或环境设置。
   - 备注：fixture 可以参数化，可以设定作用域（如 session, module, class, function），并且可以自动运行。

2. **`@pytest.mark.usefixtures()`**
   - 功能：允许测试用例使用 fixture 中的功能，但与直接传递给测试用例的 fixture 不同，它不能有返回值。
   - 备注：常用于给测试用例添加日志打印或其他无需返回值的设置。

3. **`@pytest.mark.parametrize()`**
   
   - 功能：用于参数化测试用例，可以为单个测试用例或一组测试用例提供参数。
- 备注：非常适合于需要多种不同输入值进行测试的场景。
   
4. **`@pytest.mark.skip()`**
   
   - 功能：用于跳过测试用例的执行。
- 备注：通常在调试期间或当测试用例暂时无法运行时使用。
   
5. **`@pytest.mark.skipif()`**
   - 功能：根据条件选择性地跳过测试用例。
   - 备注：当给定的条件为真时，对应的测试用例将被跳过。

6. **`@pytest.mark.xfail()`**
   
   - 功能：预期测试用例执行失败。
- 备注：用于标记那些已知失败的测试用例，这样它们就不会在测试结果中被当作意外失败。
   
7. **`@pytest.mark.run(order=1)`**
   
   - 功能：控制测试用例的执行顺序。
- 备注：需要安装 pytest-ordering 插件，通过设置不同的顺序值来控制测试用例的执行顺序。
   
8. **`@pytest.mark.rerun_failures()`**
   
   - 功能：失败重跑机制，允许失败的测试用例重新运行多次。
   
   - 备注：可以配置重跑次数，有助于确保测试失败不是由于瞬时的基础设施问题导致的。
   
   - pip install pytest-rerunfailures
   
   - 使用：
   
     ```python
     import pytest
     
     @pytest.mark.rerun_failures(reruns=3, reruns_delay=1)
     def test_example():
         # 测试代码
     ```

这些装饰器可以单独使用，也可以组合使用，以满足不同的测试需求。通过合理使用这些装饰器，可以提高测试的效率和可维护性。

请注意，某些装饰器如 `pytest.mark.run()` 需要安装额外的插件才能使用。此外，具体的使用方式和参数可能会根据 Pytest 的版本有所不同。