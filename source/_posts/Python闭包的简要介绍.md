---
title: 闭包的简要介绍
date: 2020-06-01 13:23:17
tags:
---
## 闭包
   - 是内层函数对外层函数非全局变量的应用
   - 闭包会一直存在计算机内存中，不会因为函数执行的结束而释放

## 举例说明
&emsp;&emsp;这里我们做一个求和函数，但是我们需要做到每次求和后也返回一个平均值及对应个数
```
def make_averager():
    count = 0
    total = 0

    def fun(var):
        # nonlocal count, total # 这里不加会报错
        count += 1
        total += var
        print(total)
        print(str(total/count) + ' , ' + str(count))
    return fun


if __name__ == "__main__":
    # 这里用到了高阶函数的定义，简单说就是可以将函数的返回值存储在变量中使用
    average = make_averager()
    average(5)
    average(6)
    average(7)
```
&emsp;&emsp;很糟糕，这里会有一个报错UnboundLocalError: local variable 'count' referenced before assignment，关于这里其实和变量的使用范围有关，在fun函数内部使用关键字nonlocal添加nonlocal count, total即可解决。   
&emsp;&emsp;nonlocal声明的变量不是局部变量，也不是全局变量，而是外部嵌套函数内的变量。   
&emsp;&emsp;简单来说就是在修改函数外部的变量，需要使用global；但是在闭包中，修改外层函数中的变量则需要时使用到nonlocal。