---
title: python装饰器
tags: python, decrator, metaprogram
grammar_cjkRuby: true
---
如果我们已经实现了一个函数，之后，由于需求的变更或者性能的需求，我们需要稍微改变这个函数。让你来改写这个函数，你是怎么实现呢？在静态语言（c或者c++等)中，我们没有其他选择，一是可以直接copy，修改，这样会混淆原来的代码（如果新加的逻辑和原来的逻辑不是那么紧密得结合的话），二是重新定义一个新的函数，这样的话，所有原来的调用都需要修正，当然，你可能觉得就是个全局搜索，然后替换的操作，但是不可否认，这给原来的代码增加了风险，三是定义一个指针，新的调用都是调用指针，这样更加丑陋了。这个在静态语言里面并没有很好的解决方法，除非我们的编译器增加了额外的特性。
好了，我们来描述一个简单的需求场景：
> 现在有个hello_world的函数，之前它一直工作的很好，但是呢，之前的设计并没有增加逻辑去统计这个函数的调用时间。现在想加上耗时统计，我们想看看，性能瓶颈是不是hello_world这个函数。
> 简单起见， hello_world函数定义如下:
```python
def hello_world():
	print("hello world!")

hello_world()
# output
# hello world!
```
我们既然用到了python这种高级语言，就一定要使用到高级的特性，不能使用静态语言的那一套。毕竟，
> life is short, we need python.

好了。在python里面是有一种叫做装饰器（decrator）的东西。这个东西呢，接受一个函数，不如说A，然后返回一个函数（可以实现新的功能），但是呢，他的函数名字还是叫做A。
有点绕，没关系。我们看看它的具体使用。

我们写一个简单的装饰器，只是在调用函数的时候顺便打印一下函数的名字。
```python
# decrator, just print out the name of function when calling the decrated function.
def print_out(func):
    def _wrapper(*args, **kwargs):
        print("function name: ", func.__name__)
        func(*args, **kwargs)
    return _wrapper

@print_out
def hello_world():
    print("hello world")

hello_world()
# output
# function name:  hello_world
# hello world
```
在上面我们可以看到，当我们调用*hello_world*的时候，函数除了打印函数本身的内容之外，同时还打印了函数的名字（*“function name:  hello_world”*）。这是怎么做到的呢？我们看到，在原来函数*hello_world*声明上面多了一行古怪的东西（*”@print_out“*）。这就是应用装饰器的声明。其实，装饰器只是语法糖。
我们写一个简单的装饰器，只是在调用函数的时候顺便打印一下函数的名字。
```python
@print_out
def hello_world():
    print("hello world")

# @print_out
# 等价于下面的语句
# hello_world = print_out(hello_world)
```
## 装饰器只是语法糖 
没错，每次我们在函数声明上面加上一行*@decrator_function*就是相当于做了一次赋值语句：将函数当作参数传给装饰器，将装饰器的返回值赋值给原来的函数名。当我们觉得对装饰器有点疑惑的时候，可以随时想起
**装饰器只是语法糖**。

好了，装饰器是一个函数，这个函数是接受一个函数作为参数，然后返回一个函数的。我们来看看，
```python
# decrator, just print out the name of function when calling the decrated function.
def print_out(func):
	# 返回函数_wrapper, _wrapper做的事情：打印函数的名字，调用函数。
    def _wrapper(*args, **kwargs):
        print("function name: ", func.__name__)
        func(*args, **kwargs)
    return _wrapper
```
我们知道装饰器是什么了。是接受一个函数，返回一个函数的函数。那么，我们来实现以下我们上面最初的需求，在每次调用一个函数的时候，打印一下函数的调用时间。
具体代码如下:
```python
import time
def print_function_execute_time(func):
    def _wrapper(*args, **kwargs):
        # 先记录函数开始的时间
        begin = time.time()
        func(*args, **kwargs)
        # 记录函数结束的时间
        end = time.time()
        print("function: ", func.__name__, ", cost time: ", end - begin)
    return _wrapper

@print_function_execute_time
def time_cout_func():
    time.sleep(1)

time_cout_func()
# output
# function:  time_cout_func , cost time:  1.0009493827819824
```
好了，我们成功得实现了我们的需求，现在来看，怎么来实现装饰器更加好用。
我们先来打一下装饰之后的函数的函数名字，这会有点奇怪：
```
print("name: ", hello_world.__name__)
# output
# name:  _wrapper
```
我们看到名字被改成了__wrapper , 这就有点奇怪了，我们调用的名字是hello_world，打印的名字确实是_wrapper。这里python没有出错，毕竟我们在装饰器里面返回的是_wrapper，这里它只是忠实得展示函数的名字。但是，很多时候，我们想保留原来的名字，跟调用的函数名保持一致。这也是可以做到的。只要我们使用一个装饰器就好了。看代码：
```
import functools
@functools.wraps
def print_out(func):
    # @functools.wraps
    def _wrapper(*args, **kwargs):
        print("function name: ", func.__name__)
        func(*args, **kwargs)
    return _wrapper

@print_out
def hello_world():
    print("hello world")

print("name: ", hello_world.__name__)
# output
# name:  print_out
```
一切正常，这里不展开*functools.wraps*的实现，可以看源码去。大致的功能嘛，就是替换函数的名字等，毕竟python的对象的很多属性都是保存在对象的某个成员而已，只要修改就可以了。
好了，如果我们已经有了两个装饰器，它们两实现的功能，我都想给函数加上。这里就引出了一个问题，装饰器能不能嵌套？答案是当然可以。我们看代码：
```
@print_out
@print_function_execute_time
def time_cout_func():
    time.sleep(1)

time_cout_func()

# output
# function name:   print_out
# function:  time_cout_func , cost time:  0.9999902248382568
```
我们可以看到，确实可以嵌套，但是跟我们想象不太一样。为什么？这是因为：
```
@print_out
@print_function_execute_time
def time_cout_func():

# 等价于
# time_cout_func = print_out(print_function_execute_time(time_cout_func))
```
最后，为什么函数可以当作返回值？这是在python里面，**一切皆对象**。但是这种在函数内部定义的函数还是很特别的，叫做闭包。闭包有些特性，我们还是需要了解的。好了，留待下一章吧。