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
### 面向对象