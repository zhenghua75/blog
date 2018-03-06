---
title: python 入门
date: 2018-02-21 17:01:02
tags:
	- python
	- 入门
---

### 环境

##### 前提

笔记本：macbook pro 

操作系统：macOS Sierra 版本10.12.6

python：python3.6
<!-- more -->
##### 安装

从python.org下载python3.6安装文件安装

```shell
MacBook-Pro:~ yourname$ python3 -V
Python 3.6.4
MacBook-Pro:~ yourname$ python3
>>> print('Hello, World!')
Hello, World!
>>> name = input()
yourname
>>> print('Hello, %s' % name)
Hello, yourname
```

### 语言

##### 数据类型

整数、浮点数、布尔值、None、字符串、bytes

```python
i = 3 # 整数
I = 4 # 大小写敏感
f = 3.3 # 浮点数
F = 4.4
b = True # 布尔值
B = False
n = None # 空值，命令行不显示
s = '字符串'
S = "字符串"
s = '字\n符\n串' # 转义字符 \n换行 \t制表符
s = r'字\n符\n串' # 不转义
PI = 3.14 # 常量大写
b = b'b' # bytes 类型
```

##### 编码

ascii编码，英文一个字节，无中文，utf-8可变长度编码，英文一个字节，中文3个字节。

```python
len('english'.encode('utf-8')) # 字节长度7
len(b'english'.decode('ascii')) # 字符长度7
len('中文'.encode('utf-8'))  # 字节长度6
len(b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')) # 字符长度2
```

##### 布尔操作

```python
b and B
b or B
not b

```

##### 数据类型转换

```python
int('1') # str->int
str(1) # int->str
float('1.1') # str->float
str(1.1) # float->str
bool(0) # int->bool
b.decode('ascii') # bytes->str
'b'.encode('ascii') # str->bytes
```

##### 规范

使用4个空格缩进，文件使用utf-8 without BOM编码方式编写

private使用\_前缀，特殊变量\_\_xxx\_\_

##### list与tuple

```python
l = [1, 2, 3] # 列表可变
l.append(4)
l.insert(4,5)
t = (1, 2, 3) # 元组不能修改

l[0:5] # 切片
l[:-1]
for x in l: # 迭代，通过for循环来遍历这个list
	print(x)

l = list(range(10)) # list生成
l = [x * x for x in range(10)]
g = (x * x for x in range(10)) # 生成器generator,一边循环一边计算的机制
list(g)
g = (x * x for x in range(10)) # 迭代器Iterator，生成器都是迭代器，可以被next()函数调用并不断返回下一个值的对象
for x in g:
	print(x)
def fib(max): # 生成器函数generator function，有了yield就不说普通函数
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
    return 'done'

f = fib(6)
list(f)
```

### 函数

##### 定义

```python
def 函数名(参数):
	函数体
	return 返回值
```

##### 返回值

```python
def add(a, b):
	return a+b # 一个返回值
def add_sub(a, b):
	return a+b, a-b # 多个返回值
a = add(2, 1)
b, c = add_sub(2, 1)
```

##### 参数

参数定义顺序：必选参数、默认参数、可变参数、命名关键字参数和关键字参数。

```python
def f1(a, b, c=0, *args, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw)

def f2(a, b, c=0, *, d, **kw): # 命名关键字参数需要一个特殊分隔符*，*后面的参数被视为命名关键字参数。
    print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'kw =', kw)

f1(1, 2, 3, 'a', 'b', x=99)
f2(1, 2, 3, d='a', x=99) # 命名关键字参数必须传入参数名
```

##### 函数式编程

高阶函数，参数是函数，map/reduce、filter、sorted

```python
def f(x):
	return x * x

l = list(range(1, 11))
m = map(f, l)
l2 = list(m)
```

```python
from functools import reduce
def add(x, y):
	return x + y
l = list(range(1, 11))
r = reduce(add, l)
```

```python
def is_odd(x):
	return x % 2 == 1
l = list(range(1, 11))
f = filter(is_odd, l)
list(f)
```

```python
sorted([36, 5, -12, 9, -21], key=abs)
```

闭包Closure，函数作为返回值

```python
def lazy_sum(*args):
    def sum():
        ax = 0
        for n in args:
            ax = ax + n
        return ax
    return sum

f = lazy_sum(1, 3, 5, 7, 9)
f()
```

匿名函数

```python
lambda x: x * x
# 等价
def f(x):
	return x * x
```

装饰器decorator，一个返回函数的高阶函数，借助@语法，置于函数的定义处。代码运行期间动态增加功能的方式。

```python
def log(func):
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper

@log
def now():
    print('2018-2-21')
```

偏函数，设定参数的默认值，降低函数调用的难度

```python
import functools
int2 = functools.partial(int, base=2)
int2('1000000')
```

##### module与package

module按文件组织代码，package按目录组织代码，目录有\_\_init\_\_.py文件

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

' a test module ' # 文档注释，任何模块代码的第一个字符串

__author__ = 'zheng hua' # 作者

import sys

def test():
    pass

if __name__=='__main__': #判断命令行运行
    test()
```

### OOP

##### class

数据封装、继承和多态

```python
from types import MethodType

Class Oneclass(object): # 类名大写开头，继承自object
	
	__slots__ = ('field1', 'field2', 'field3', 'field4') # 用tuple定义允许绑定的属性名称

	field3 = 'field3' # 类属性
	
	@property
	def field5(self):
		return self.__field5

	def __init__(self, field1, field2): # self代表实例自身，类中函数第一个参数永远是self
		self.field1 = field1 # 属性
		self.__field2 = field2 # 私有

	def method1(self): # 方法
		print('%s %s' % (self.field1, self.__field2))

	def get_field2(self):
		return self.__field2

	def set_field2(self,field2):
		self.__field2 = field2

	def __str__(self): #print时用到
		return 'Oneclass'

	__repr__ = __str__ # 调试时用到

	def __iter__(self): # 迭代
		pass

	def __next__(self):
		pass

	def __getitem__(self, n): # 获取onclass[1]
		pass

	def __getattr__(self, attr):# 没有属性时
		pass

	def __call__(self): # 实例本身上调用oneclass()
		pass

oneclass = Oneclass(1, 2) # 实例instance
oneclass.method1() # 方法调用
oneclass.field1 = 3 # 设置属性
oneclass.set_field(4)

Class Subclass(Oneclass):
	def method1(self): # 多态，覆盖父类方法
		print('hello %s %s' % (self.field1, self.__field2))

subclass = Subclass(1, 2)
subclass.method1() 

onclass.field4 = 4 # 动态给实例绑定一个属性

def method2(self): # 定义一个函数作为实例方法
	pass

one.method2 = MethodType(method2, oneclass) # 给实例绑定一个方法

def method3(self):
	pass

Oneclass.method3 = method3 # 给class绑定方法

Class Twoclass(object):
	pass

Class Threeclass(Oneclass,Twoclass):# 多重继承，MixIn设计
	pass
```

##### Enum

```python
from enum import Enum

Month = Enum('Month', ('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))

Month.Jan # <Month.Jan: 1>
Month.Jan.name # Jan
Month.Jan.value # 1 默认从1开始计数
for name, member in Month.__members__.items():
	print(name, '=>', member, ',', member.value)#Jan => Month.Jan , 1
```

```python
from enum import Enum, unique

@unique
class Weekday(Enum):
    Sun = 0 # Sun的value被设定为0
    Mon = 1
    Tue = 2
    Wed = 3
    Thu = 4
    Fri = 5
    Sat = 6
```