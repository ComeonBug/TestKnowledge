# 基础面试题：

## python是什么类型的语言

解释型 通用编程语言



## python数据类型有哪些，哪些可变哪些不可变

number、bool、string、list、set、tuple、dict

可变：list、dict、set

不可变：bool、number、string、tuple



## 列表和元组的区别

列表可变，元组不可变



# 高级面试题

## PYTHONPATH是什么

pythonpath：一个环境变量

当我们导入python模块的时候，会查找pythonpath

通过pythonpath来检查目录是是否有要导入的模块

python解释器 通过 pythonpath 来决定 导入哪个模块



## 什么是python模块，列举几个常用模块





## python内存管理机制



## python中如何管理内存

由python专用堆空间管理，所有的对象和数据结构都存在 【python堆空间】里

【python内存管理器】 来分配 【python堆空间】

【垃圾收集器】 回收未使用的内存， 回收完后的内存给 【python堆空间】使用







## python垃圾回收机制



