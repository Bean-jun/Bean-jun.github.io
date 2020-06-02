---
title: Python迭代器及生成器的简单理解
date: 2020-06-02 08:36:31
tags:
---
## 迭代器
&emsp;&emsp;在使用列表时，直接将数据存入列表将会占据大量空间，且复用率较低，
为解决这个问题，这里了解一下迭代器，从而创建一种数据产生的方式，以此来节省空间。
## 迭代对象
&emsp;&emsp;注意，这里需要使用到内建函数__iter__，简单理解为，使用了__iter__才会是一个可迭代对象，关于这部分，我们可以对一些对象做一些判断，从而清楚是不是可迭代对象，比如L1列表：
```
from collections.abc import Iterable
# 判断L1是否为可迭代对象
L1 = [i for i in range(4)]
print(isinstance(L1, Iterable)) # 结果为True，确定为迭代对象
```
&emsp;&emsp;这里我们使用__iter__()方法和__next__()方法来将元素取出。
```
L2 = L1.__iter__()
# L2 = iter(L1) # 和上面一句等价
print(next(L2))
print(L2.__next__()) # 和上面一句等价
print(next(L2))
```
&emsp;&emsp;在这里我们实现了使用for循环来取出元素的方法，在了解这里之后，我们开始做一个自己的迭代器
```
# 构造迭代器
class NewIter():
    # 用于生成斐波那契数列
    def __init__(self, num):
        self.num = num
        self.a = 0
        self.b = 1

    def __iter__(self):
        # 创建可迭代对象
        return self
    
    def __next__(self):
        while self.num > 0:
            result = self.a
            self.a, self.b = self.b, self.a + self.b
            self.num -= 1
            return result
        # 循环结束后跳出
        raise StopIteration
        
new_iter = NewIter(5)
print(next(new_iter))
print(next(new_iter))
```
## 生成器
&emsp;&emsp;这里引入生成器个概念，生成器能做到迭代器能做的所有事，由于自动创建了iter()和next()方法，生成器显得特别简洁。除了创建和保存程序状态的自动方法,当
发生器终结时,还会自动抛出 StopIteration 异常。简单理解他就是迭代器的一种！！使用yield即可实现。
```
# 生成器实现
def new_create(num):
    # 用于生成斐波那契数列
    a, b = 0, 1
    while num > 0:
        yield a
        a, b = b, a+b
        num -= 1
        
new_iter = new_create(5)
print(next(new_iter))
print(next(new_iter))
```
## 结语
&emsp;&emsp;目的就是节省空间，提高复用率