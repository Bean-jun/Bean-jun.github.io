---
title: Python连接C实现高效编程demo
date: 2020-05-31 08:54:12
tags:
---
## 一、前言
作为一门经典的编程语言，这些年来，C有着重要的地位，在各个领域的使用都是相当重要的地位！而Python作为较新式的语言，相对而言入门更加简单，适合新手来学习！但是作为一门动态的脚本语言，其效率相对C来说还是有些捉急的。在这里，我们引入一个demo来作为Python和C混合编程的一个基础。

## 二、编写代码
在这里我会使用对比的方式来比较两种语言计算一个较大数字求和时的耗时情况，从而直观体验其中的差别。同时这里会引入混合编程的概念。   
1. 编写C的代码
    ```
    #include<stdio.h>

    void add(long VALUE){
        long sum = 0;
        for (long i = 1;i < VALUE; i ++)
            sum += i;
        printf("C求和结果为: %ld\n", sum);
    }
    ```
2. 编写Python代码
    ```
    import time

    VALUE = 9999999

    t1 = time.time()
    sum = 0
    for i in range(1, VALUE):
        sum += i
    print('for循环求和结果为：'+ str(sum))
    t2 = time.time()
    print('for循环求和开销时间为：'+ str(t2-t1))
    ```
3. 将之前的C代码编译成Python可以读取调用的文件
    - gcc -fPIC -shared C的代码 -o 后续调用文件名.so   
    例如: gcc -fPIC -shared sum.c -o csum.so
4. 将csum.so加载到Python中，进行编写
    - 导入库文件`from ctypes import cdll`
    - 调用文件
        ```
        result = cdll.LoadLibrary("./csum.so")
        result.add(VALUE)
        ```

5. 最终Python代码sum.py是酱紫

    ```
    import time
    from ctypes import cdll

    VALUE = 9999999

    # 使用Python求和
    t1 = time.time()
    sum = 0
    for i in range(1, VALUE):
        sum += i
    print('for循环求和结果为：'+ str(sum))
    t2 = time.time()
    print('for循环求和开销时间为：'+ str(t2-t1))
    print('-'*30)

    # Python连接C求和
    t1 = time.time()
    result = cdll.LoadLibrary("./csum.so")
    result.add(VALUE)
    t2 = time.time()
    print('使用C求和开销时间为：'+ str(t2-t1))
    ```
6. 最终结果
    ```
    for循环求和结果为：49999985000001
    for循环求和开销时间为：3.156290292739868
    ------------------------------
    C求和结果为: 49999985000001
    使用C求和开销时间为：0.043367624282836914
    ```
## 三、结语
由上述可以看出来，同样使用for循环，但是使用c的效率是非常非常高的，所以针对一些特殊场景，使用c编写，然后用Python调用也是不错的方法。