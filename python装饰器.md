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
```
我们既然用到了python这种高级语言，就一定要使用到高级的特性，不能使用静态语言的那一套。毕竟，
> life is short, we need python.

好了。在python里面是有一种叫做装饰器（decrator）的东西。这个东西呢，接受一个函数，不如说A，然后返回一个函数（可以实现新的功能），但是呢，他的函数名字还是就做A。
有点绕，没关系。我们看看它的具体使用。
```python
# decrator, just print out the name of function when calling the decrated function.
def 
```