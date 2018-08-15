---
title: python-闭包
tags: python,闭包,装饰器
grammar_cjkRuby: true
---


在上一章装饰器的时候，我们提到了装饰器的特征（函数头部）。
> 装饰器是一个函数，这个函数接受一个函数作为参数，返回一个函数。

而通常，装饰器返回的函数一般是定义在装饰器内部的，也就是说我们在函数的内部定义了一个函数。这也是非常好的一个特性。优点是，函数是会产生命名空间的，这样，我们在函数外部不能直接引用到了这个内部定义的函数，也就不会污染全局的变量名。当然，还有其他的优点，比如，书写方便啊，逻辑紧密结合等。

> 这种定义在函数内部的函数，其实就是闭包。

如果我们只是在函数内部里面定义了一个函数，没有将其返回（或者通过其他手段交给其他对象），那么我们是无法访问这个内部定义的函数的。
```python
def outter():
    def inner():
        print("inner")
    print("outter")

outter.inner()
# output:
# AttributeError                            Traceback (most recent call last)
# <ipython-input-29-18d9236e9c7d> in <module>()
# ----> 1 outter.inner()
# 
# AttributeError: 'function' object has no attribute 'inner

inner()
# output:
# NameError                                 Traceback (most recent call last)
# <ipython-input-33-bc10f1654870> in <module>()
# ----> 1 inner()
# 
# NameError: name 'inner' is not defined

```
所以，一般我们会返回这个内部的对象，以供使用，这就是装饰器常用的实现。
闭包是什么？定义在函数内部的函数。对的，这个是闭包的表现形式。但是，闭包的真正做了什么东西，在python里面是怎么实现的？
> 闭包在python中的实现其实跟类的可调用对象差不多。

我们都知道知道类实现了`__call__`，那么这个类产生的对象就可以像函数一样被调用。
```python
class A:
    def __call__(self):
        print("__call__")
		
a = A()
a()

# output
# __call__
```
用对象当作函数来调用，可以带来很多好处，比如说，可以用对象来保存函数的状态。下面这个例子，可以每次调用这个对象函数的时候，统计对象函数被调用的次数。
```python
class Count:
    def __init__(self):
        self.count = 0
        
    def __call__(self):
        print("__call__, count: {}".format(self.count))
        self.count += 1

c = Count()
c()
c()

# output:
# __call__, count: 0
# __call__, count: 1
```
那么闭包是怎么跟可调用对象联系起来的呢？
```python
def outter():
    def inner():
        print("inner")
    return inner  # 这里为了可以在函数outter之外访问到函数inner， 这里将函数inner返回了

下面定义的可调用对象和上面的是一样的

class Outter(object):
	def __call_
```