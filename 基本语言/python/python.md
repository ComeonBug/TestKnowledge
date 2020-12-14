# 基础知识

我为什么选择python？

1.入门简单，快速上手，易于测试组内提升

2.丰富的第三方库，快速上手

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

集合（无序、不可重复）常用方法：add、union、clear、pop、difference、intersection、remove

元组（不可变更）常用方法：index、count

文件：file1=open('test.txt','r')

## 逻辑控制：

if、while、for、break、contine



## 高级表达式

列表推倒式：

```python
l=[i*2 for i in range(1,101,2)] 
```

lambda表达式：



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



面试题：

*args、**kwargs：



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



迭代器、生成器、装饰器

反射

原语









