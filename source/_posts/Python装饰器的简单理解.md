---
title: Python装饰器的简单理解
date: 2020-06-01 15:46:14
tags:
---
## 装饰器
&emsp;&emsp;通俗点理解就是锦上添花，关于这块需要参考一些软件开发的设计模式，在这里引入一个小的故事。   
&emsp;&emsp;公司招了一个人A，某一天，项目组想要给一个接口添加一些时间计算，具体时间计算没有说，但是这个功能要加，A呢就去查看接口中的代码，在代码中添加了一个时间计算，过了两天，项目组要求所有接口全部添加时间计算，此时A则一个个的去每个接口中添加时间计算，以至于天天晚上加班，结果没过多久就被炒鱿鱼了。此时来了B，他先写了个时间计算函数并在每个接口头部添加这个函数名，实现了对应的加时间计算功能。没多久就升职加薪了。   
&emsp;&emsp;看到这里，不禁会想，为什么呢？？？其实这里涉及到一个软件开发的设计模式-开闭原则（简单说就是功能可以加，代码修改是不允许的），A就明显犯了这个错误，
而B则是使用到了我们接下来要说到的装饰器。
## 引入
&emsp;&emsp;在了解装饰器之前，请先了解闭包的相关概念！！以及高阶函数的概念！！
这里我们有一个阶乘功能的函数
```
def product(value):
    # 实现阶乘功能
    result = 1
    while value > 0:
        result *= value
        value -= 1
    print(result)
```
1. 我们需要添加一个计算运行时间的功能，请记住，不要改动product函数，直接做功能添加，这里使用装饰器处理。首先我们创建一个装饰器：
    ```
    import time

    def count_time(func):
        def call_func(var):
            t1 = time.time()
            func(var)
            t2 = time.time()
            print('消耗时间为：'+ str(t2-t1))
        return call_func
    ```
2. 对原有函数进行装饰改造
    ```
    @count_time
    def product(value):
        # 实现阶乘功能
        result = 1
        while value > 0:
            result *= value
            value -= 1
        print(result)
    ```
3. 细节理解   
&emsp;&emsp;在第二步中的@count_time其实就是装饰器的基本使用，我们在使用product(100)时，解释器不会直接去调用product函数，而是实现调用count_time功能，并将返回的call_func返回给product，也就是说此时的product其实是指向call_func的，当执行product（100）时，其实是在向call_func传递参数，实现功能，简单理解可以使用以下两行代码来表示：
    ```
    product = count_time(product) # 此时的product是指向call_func函数的
    product(100) # 相当于执行call_func(100)
    ```
## 更高一级的操作
1. 原函数需要传递多个参数
&emsp;&emsp;这里很好处理，很正常函数传递参数相同，我们在装饰器中最内层函数中需要传递的变量名改为`*args, **kwargs`，在执行时，将其直接传入即可。
2. 原函数有返回值
&emsp;&emsp;这里我们可以直接使用return将装饰器中最内层的执行函数部分return出去即可，比如：
    ```
    def count_time(func):
        def call_func(var):
            return func(var)
        return call_func
    ```
3. 两个装饰器同时装饰同一个函数
&emsp;&emsp;先看一下伪代码
    ```
    @装饰器1
    @装饰器2
    def fun():
        pass
    ```
&emsp;&emsp;意思很简单，就是在原来函数基础上加两个新功能，但是关于执行顺序是怎么样的呢？
&emsp;&emsp;就不买关子了，装饰器1先执行，但装饰器1最后装饰，其实这里和堆栈很相似，若是方便，可以自己编代码，打断点调试一下就知道了。
## 结语
未完待续...