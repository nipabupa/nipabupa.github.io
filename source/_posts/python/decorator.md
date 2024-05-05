---
title: Python装饰器
description: 特殊功能的上下文
categories:
    - Python
tags:
    - Python语法
---

## 概念

在不改变原有代码的情况下，为被装饰的对象附加新的功能、附加限制条件或改变输出

类型：
- 函数装饰器
- 类装饰器

## 原理

开发封闭原则，利用[闭包](./closure.md)的特性，完成被装饰对象的隐式替换。


举个栗子：


```python
import functools


# 定义一个辅助打印日志的装饰器
def log(func):
    @functools.wraps(func)
    def inner():
        print('log start')
        func()
        print('log end')
    return inner

@log
def hello():
    print('hello')


hello()
```

输出结果:

```
log start
hello
log end
```

执行顺序
1. 系统读取装饰器定义并生成对应的函数对象
2. 当遇到`@`时，执行后面对应标识符的装饰器函数（若对应标识符为一个函数有参数，则先执行该函数获取返回字符串，执行该字符串对应的装饰器函数）
3. 执行装饰器函数时，将被装饰对象（此处为hello函数对象，可以理解为指针）传递给装饰器函数
4. 装饰器函数执行闭包的逻辑，将原对象进行二次加工，返回加工后的对象

结果经过装饰器修饰，实际执行的hello函数已经是被装饰器替换后的函数，因此具有新的功能

由于被修饰的对象已经不是原有的对象，因此通过`hello.__name__`获取到的实际是`inner`，与我们预期不符，因此通过增加`@functools.warps(func)`，来解决这些副作用。

