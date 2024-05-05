---
title: Python闭包
description: Python函数式编程
categories:
    - Python
tags:
    - Python语法
---

## 概念

闭包是具体特殊作用域的函数，它在函数定义体中引用了不在定义体中的非全局变量。

- 函数式编程的重要的语法结构
- 一种组织代码的结构
- 提高了代码的可重复使用性

## 条件

- 必须有一个内部函数
- 内部函数必须引用外部函数的变量
- 外部函数的返回值必须是内部函数本身

举个栗子：

```python
def out():
    a = []
    def inner():
        a.append('test')
        print(a)
    return inner

out_ins = out()
out_ins()
out_ins()
out_ins()

out_ins2 = out() 
out_ins2() 
out_ins2() 
out_ins2() 
```

返回结果

```
['test']
['test', 'test']
['test', 'test', 'test']
['test']
['test', 'test']
['test', 'test', 'test']
```

## 原理

闭包是一种函数，它会保留定义函数时存在的自由变量的绑定，这样调用函数时虽然定义作用域不可用了，但仍能使用那些绑定

实质上即为每一个闭包函数实例都有一个或多个独属于自己的内部的静态变量。

## `nolocal`关键字

若闭包内部函数需要对外部函数的变量进行赋值，需要使用`nolocal`声明外部变量，然后赋值。

```python
def out():
    a = []
    def inner():
        # 若不使用nolocal，则变量a即为inner函数的内部变量，与out函数定义的变量a无任何关系
        # 若使用nolocal，则变量a即为out函数定义的变量a
        nonlocal a
        a = ['test']
        print(a)
    return inner

out_ins = out()
out_ins()

out_ins2 = out() 
out_ins2() 
```

返回结果

```
['test']
['test']
```

