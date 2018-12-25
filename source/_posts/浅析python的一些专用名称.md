---
layout: _post
title: 浅析python的一些专用名称
date: 2018-12-25 11:27:45
tags:
categories: python 学习

---
## 前言
多少时候，被那些专有名词搞得很伤，但是又不得不去遵循那套规则。今天我们来聊一聊 可迭代对象，迭代器，生成器，闭包，装饰器这个几个专有名词的意思。

### 可迭代对象
这个名称挺有意思的，从我们中文博大精深的字面意思可知，他首先是个对象，然后是可以迭代的,在聊这个可迭代对象这儿专有名词之前，我们可以先来认识认识什么是容器，容器我们可以理解为可以装东西的物件，平常我们经常吃饭，碗就成了容器，因为他可以装饭，在程序中，我们定义将多个元素组织在一起的数据结构称之为容器，常见的容器对象有:
>* list(列表), deque(队列)
>* set(可变集合), frozensets(不可变集合)
>* dict(字典)
>* tuple(元组)
>* str(字符串)

我们说容器，称之为容器对象，但是大部分容器本身能获取其中的元素吗？答案是不能的，就好像碗他会吃饭吗，因为他没有这个动作或者这个能力，但是可迭代对象出来了，他是容器对象的一种，或者说少部分，比如我们上面中的list,set,str等都可以说是可迭代对象
```
x=[1,2,3]
next(x)
1
```
这里可以说x就是可迭代对象，next()是这个对象本身的方法，他有这个能力。他就是可迭代对象


### 迭代器
迭代器在我看来他是一个过程的工具，你也可以说他是可以访问容器中元素的一种方式。
```
x=[1,2,3]
for x in x:
    print x
1
2
3
```
这就是个简单的迭代器，他实现了对列表中每一个元素的访问,那么我们可以说他就是个迭代器

### 生成器
生成器就是迭代器的一种特殊形式，生成器可以节省内存和cpu,我们可以参考下迭代器和生成器代码的比较
```
迭代器
def something():
    result = []
    for x in range(5):
        result.append(x)
    return result
```


```
生成器
def something():
    result = []
    for x in range(5):
        yield x

yield是生成器的关键字

```
>>* 生成器表达式

他返回的是一个生成器对象，而不是列表对象
```
a = (x*x for x in range(10))
print a
<generator object <genexpr> at 0x0000000002724D38>
他输出的是个对象
print sum(a)
285
```
在我感觉他就是将迭代的元素和方法封装起来了，当你需要他的时候，你调用他，他在去实行它内部的方法，这样就可以节省好多内存，你不调用的时候，我也存储什么数据，我只是个对象，那有一天你要用我的时候，来吧，调用我吧，我就直接执行内部方法。

有点绕口是吧，关于对象这个坑，我们下面还会聊，万物皆虚，万事皆允，python里面的所有东西都可以看做是对象，但是对象在我们看来又是很具体又很虚的东西。

我们先来总结下上面的一些东西，可迭代对象是指一些特殊的容器，他可以被迭代，比如(用在for,while等语句中)，迭代器他是一种访问内部元素的方式，生成器是迭代器的特殊变种，他的关键字是yield，而不是return。



### 闭包
如果理解了作用域和对象这个问题的话，关于闭包和装饰器的理解起来就会非常容易。

先将闭包之前我们讲一下作用域吧,函数的作用域他是分为LEGB:L>E>G>B
>* L:local 函数内部作用域
>* E:enclosing 函数内部与嵌套函数之间
>* G:global 全局作用域，我们所定义的全局变量
>* B:build-in 内置作用域，python解释器默认导入的一些变量

build-in比如list,元组等
访问作用域的的原则就是从里到外
```
x = 60              # 全局变量
def func(y):  
    if y >= x:
        print "pass"
    else:
        print "failed"
func(80)

pass

```


```
x = 60              # 全局变量
def func(y): 
    x =90
    if y >= x:
        print "pass"
    else:
        print "failed"
func(80)

faild

```

通过这个例子证明了如果编辑器查找顺序是先找里面的变量在找外面作用域的变量，实际也是证明了编辑器的查找顺序是*L-E-G-B*
那什么是闭包呢，其实说简单点就是内部函数对E 作用域的变量的引用。

在这里我简单说下引用是什么意思，引用就是访问那个对应的对象。那么问题来了，对象是什么呢？老问题了，一个函数对象就是指包含函数里面的一切，变量，方法等。
```

def func()
    print "-----"

fun()               # 他就是个fun函数
fun                 # 他是这个函数对象，指向func()这个函数，也可说成是引用
```

js的闭包和python都是一样的

```
js 闭包
function foo(){
    var a = 2;
    function bar(){
        console.log(a);
    }
    return bar
}
var baz = foo();
baz()          //2      这就是闭包的效果

```

```
python 闭包

def prin_msg():
    msg = "xiao of python"
    def printer():
        print(msg)
    return printer

anther = print_msg()
anther()


输出xiao of python
```
他是调用了内部的printer()函数，输出E作用域的变量，这就是闭包，他使得局部变量在函数外部访问成了可能。这里anther就是个闭包，他由printer函数和变量msg。闭包使得这些变量始终保存在内存中。

#### 为什么要使用闭包

总结起来就是简化编程操作，调用起来十分方便

```
def adder(x):
    def wrapper(y):
        return x+y
    return wrapper

adder1 = adder(5)               # adder1是一个对象
adder1(10)
# 15
adder1(6)
# 11
```
是不是非常方便呢。但是外部函数的局部变量，则外部函数的局部变量没有及时释放，消耗内存。相比他的缺点，优点就是使得局部变量在函数外部访问成了可能。换句话说就是使得在函数外部在访问局部变量成为了可能。


### 装饰器
上面我们说道闭包，我们知道函数他也是一个对象。下面讲到主角了，他就是装饰器。如果你理解了上面的闭包，那么理解起装饰器来是非常的简单。装饰器本身就是个闭包，只是相对高级一些，因为相对高级一点，所以也就有对应的语法出来，@出来了，他就是语法糖，写起来相当的帅气，其实就是偷懒的一种方式

我们先来基础函数
```
def foo():
    print('foo')

foo     #表示是函数
foo()   #表示执行foo函数

#### 第二波 ####
def foo():
    print('foo')

foo = lambda x: x + 1

foo()   # 执行下面的lambda表达式，而不再是原来的foo函数，因为foo这个名字被重新指向了另外一个匿名函数
```

#### 缘起
初创公司有N个业务部门，1个基础平台部门，基础平台负责提供底层的功能，如：数据库操作、redis调用、监控API等功能。业务部门使用基础功能时，只需调用基础平台提供的功能即可。如下：
```

############### 基础平台提供的功能如下 ###############

def f1():
    print('f1')

def f2():
    print('f2')

def f3():
    print('f3')

def f4():
    print('f4')

############### 业务部门A 调用基础平台提供的功能 ###############

f1()
f2()
f3()
f4()

############### 业务部门B 调用基础平台提供的功能 ###############

f1()
f2()
f3()
f4()
```

目前公司有条不紊的进行着，但是，以前基础平台的开发人员在写代码时候没有关注验证相关的问题，即：基础平台的提供的功能可以被任何人使用。现在需要对基础平台的所有功能进行重构，为平台提供的所有功能添加验证机制，即：执行功能前，先进行验证。我们先想到的都是动原函数，但是这哪里能动，别部门写的底层一般是不能动的，现在装饰器出来了，这个问题迎面而解。

看下下面的代码
```
def w1(func):
    def inner():
        # 验证1
        # 验证2
        # 验证3
        func()
    return inner

@w1
def f1():
    print('f1')
@w1
def f2():
    print('f2')
@w1
def f3():
    print('f3')
@w1
def f4():
    print('f4')
```

我们先看这段函数，他的执行过程
```
def w1(func):
    def inner():
        # 验证1
        # 验证2
        # 验证3
        func()
    return inner

@w1
def f1():
    print('f1')
```

```
@w1
def f1():
    print('f1')
```
这段代码我们可以了解下python解释器的过程
>* def w1(func): ==>将w1函数加载到内存
>* @w1

上例@w1内部会执行一下操作：
```
def w1(f1):
    def inner():
        # 验证1
        # 验证2
        # 验证3
        f1()
    return inner
```
这个过程其实就是执行inner()的过程
但是还没完，然后执行
```
新f1 = def inner(): 
            #验证 1
            #验证 2
            #验证 3
            原来f1()
        return inner
```
现在的f1就是下面这段代码
```
@w1
def f1():
    print('f1')
```
以后调用起来那是相当方便，这样即达到了添加验证的的功能，又使得没有动用底层代码

#### 装饰器代码例子
```
#定义函数：完成包裹数据
def makeBold(fn):
    def wrapped():
        return "<b>" + fn() + "</b>"
    return wrapped

#定义函数：完成包裹数据
def makeItalic(fn):
    def wrapped():
        return "<i>" + fn() + "</i>"
    return wrapped

@makeBold
def test1():
    return "hello world-1"

@makeItalic
def test2():
    return "hello world-2"

@makeBold
@makeItalic
def test3():
    return "hello world-3"

print(test1()))
print(test2()))
print(test3()))
```
结果是这样，多看看就懂了
```
<b>hello world-1</b>
<i>hello world-2</i>
<b><i>hello world-3</i></b>
```
#### 装饰器----被装饰的函数有参数

之前我们看到的被装饰的函数无参数，那么有参数该怎么办呢，先上示例
```

from time import ctime, sleep

def timefun(func):
    def wrappedfunc(a, b):
        print("%s called at %s"%(func.__name__, ctime()))
        print(a, b)
        func(a, b)
    return wrappedfunc

@timefun
def foo(a, b):
    print(a+b)

foo(3,5)
sleep(2)
foo(2,4)
```
我们可以引入*args,**kwargs,先解释下这两个是什么东西，*args是指接收任意多个参数，**kwargs是指接收任意多个关键字参数，比如那种 bar="xiaoming",这种字典型参数。
```
from time import ctime, sleep

def timefun(func):
    def wrappedfunc(*args, **kwargs):
        print("%s called at %s"%(func.__name__, ctime()))
        func(*args, **kwargs)
    return wrappedfunc

@timefun
def foo(a, b, c):
    print(a+b+c)

foo(3,5,7)
sleep(2)
foo(2,4,9)
```



#### 装饰器----装饰器中的return
```

from time import ctime, sleep

def timefun(func):
    def wrappedfunc():
        print("%s called at %s"%(func.__name__, ctime()))
        func()
    return wrappedfunc

@timefun
def foo():
    print("I am foo")

@timefun
def getInfo():
    return '----hahah---'

foo()
sleep(2)
foo()


print(getInfo())
```
结果是
```
foo called at Tue Dec 25 16:41:33 2018
I am foo
foo called at Tue Dec 25 16:41:35 2018
I am foo
getInfo called at Tue Dec 25 16:41:35 2018
None
```
如果修改装饰器为return func()，则运行结果：
```
foo called at Fri Nov  4 21:55:57 2016
I am foo
foo called at Fri Nov  4 21:55:59 2016
I am foo
getInfo called at Fri Nov  4 21:55:59 2016
----hahah---
```
一般情况下为了让装饰器更通用，可以有return

#### 装饰器----装饰器带参数
```
from time import ctime, sleep

def timefun_arg(pre="hello"):
    def timefun(func):
        def wrappedfunc():
            print("%s called at %s %s"%(func.__name__, ctime(), pre))
            return func()
        return wrappedfunc
    return timefun

@timefun_arg("itcast")
def foo():
    print("I am foo")

@timefun_arg("python")
def too():
    print("I am too")

foo()
sleep(2)
foo()

too()
sleep(2)
too()
```
可以理解为
```
foo()==timefun_arg("itcast")(foo)()
```


