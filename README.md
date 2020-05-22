# interview-question
<!-- 面试题 -->

## 1.Python里的拷贝

引用和copy(),deepcopy()的区别

import copy

a = [1, 2, 3, 4, ['a', 'b']]  #原始对象
b = a  #赋值，传对象的引用
c = copy.copy(a)  #对象拷贝，浅拷贝
d = copy.deepcopy(a)  #对象拷贝，深拷贝

a.append(5)  #修改对象a

a[4].append('c')  #修改对象a中的['a', 'b']数组对象

print 'a = ', a
print 'b = ', b
print 'c = ', c
print 'd = ', d

输出结果：

a =  [1, 2, 3, 4, ['a', 'b', 'c'], 5]
b =  [1, 2, 3, 4, ['a', 'b', 'c'], 5]
c =  [1, 2, 3, 4, ['a', 'b', 'c']]
d =  [1, 2, 3, 4, ['a', 'b']]

## 2.鸭子类型

“当看到一只鸟走起来像鸭子、游泳起来像鸭子、叫起来也像鸭子，那么这只鸟就可以被称为鸭子。”
我们并不关心对象是什么类型，到底是不是鸭子，只关心行为。
比如在python中，有很多file-like的东西，比如StringIO,GzipFile,socket。它们有很多相同的方法，我们把它们当作文件使用。
又比如list.extend()方法中,我们并不关心它的参数是不是list,只要它是可迭代的,所以它的参数可以是list/tuple/dict/字符串/生成器等.
鸭子类型在动态语言中经常使用，非常灵活，使得python不像java那样专门去弄一大堆的设计模式。

## 3.字典推导式

可能你见过列表推导时,却没有见过字典推导式,在2.7中才加入的:
d = {key: value for (key, value) in iterable}

## 4.单例模式

​ 单例模式是一种常用的软件设计模式。在它的核心结构中只包含一个被称为单例类的特殊类。通过单例模式可以保证系统中一个类只有一个实例而且该实例易于外界访问，从而方便对实例个数的控制并节约系统资源。如果希望在系统中某个类的对象只能存在一个，单例模式是最好的解决方案。

__new__()在__init__()之前被调用，用于生成实例对象。利用这个方法和类的属性的特点可以实现设计模式的单例模式。单例模式是指创建唯一对象，单例模式设计的类只能实例

##### 这个绝对常考啊.绝对要记住1~2个方法,当时面试官是让手写的

```python
1. 使用__new__方法
class Singleton(object):

    def __new__(cls, *args, **kw):

        if not hasattr(cls, '_instance'):

            orig = super(Singleton, cls)

            cls._instance = orig.__new__(cls, *args, **kw)

        return cls._instance
class MyClass(Singleton):

    a = 1

2. 共享属性
创建实例时把所有实例的__dict__指向同一个字典,这样它们具有相同的属性和方法.
class Borg(object):

    _state = {}

    def __new__(cls, *args, **kw):

        ob = super(Borg, cls).__new__(cls, *args, **kw)

        ob.__dict__ = cls._state

        return ob
class MyClass2(Borg):

    a = 1

3. 装饰器版本
def singleton(cls):

    instances = {}

    def getinstance(*args, **kw):

        if cls not in instances:

            instances[cls] = cls(*args, **kw)

        return instances[cls]

    return getinstance
@singleton

class MyClass:

...

4. import方法
作为python的模块是天然的单例模式
# mysingleton.py
class My_Singleton(object):

    def foo(self):

        pass
my_singleton = My_Singleton()
# to use

from mysingleton import my_singleton
    my_singleton.foo()
```

## 5.Python中的作用域

Python 中，一个变量的作用域总是由在代码中被赋值的地方所决定的。
当 Python 遇到一个变量的话他会按照这样的顺序进行搜索：
    本地作用域（Local）→当前作用域被嵌入的本地作用域（Enclosing locals）→
    全局/模块作用域（Global）→内置作用域（Built-in）



## 6. **Python****自省**

这个也是python彪悍的特性.

自省就是面向对象的语言所写的程序在运行时,所能知道对象的类型.简单一句就是运行时能够获得对象的类型.比如type(),dir(),getattr(),hasattr(),isinstance().

a = [1,2,3]

b = {'a':1,'b':2,'c':3}

c = True

print type(a),type(b),type(c) # <type 'list'> <type 'dict'> <type 'bool'>

print isinstance(a,list)  # True



## 7.**__new__****和****__init__****的区别**

这个__new__确实很少见到,先做了解吧.

1. __new__是一个静态方法,而__init__是一个实例方法.
2. __new__方法会返回一个创建的实例,而__init__什么都不返回.
3. 只有在__new__返回一个cls的实例时后面的__init__才能被调用.
4. 当创建一个新实例时调用__new__,初始化一个实例时用__init__.

[stackoverflow](http://stackoverflow.com/questions/674304/pythons-use-of-new-and-init)

ps: __metaclass__是创建类时起作用.所以我们可以分别使用__metaclass__,__new__和__init__来分别在类创建,实例创建和实例初始化的时候做一些小手脚.



## 8.**协程**

知乎被问到了,呵呵哒,跪了

简单点说协程是进程和线程的升级版,进程和线程都面临着内核态和用户态的切换问题而耗费许多切换时间,而协程就是用户自己控制切换的时机,不再需要陷入系统的内核态.

Python里最常见的yield就是协程的思想!可以查看第九个问题.



## 9.**Python垃圾回收机制**

Python GC主要使用引用计数（reference counting）来跟踪和回收垃圾。在引用计数的基础上，通过“标记-清除”（mark and sweep）解决容器对象可能产生的循环引用问题，通过“分代回收”（generation collection）以空间换时间的方法提高垃圾回收效率。

**1** **引用计数**

PyObject是每个对象必有的内容，其中ob_refcnt就是做为引用计数。当一个对象有新的引用时，它的ob_refcnt就会增加，当引用它的对象被删除，它的ob_refcnt就会减少.引用计数为0时，该对象生命就结束了。

优点:

1. 简单
2. 实时性

缺点:

1. 维护引用计数消耗资源
2. 循环引用

**2** **标记****-****清除机制**

基本思路是先按需分配，等到没有空闲内存的时候从寄存器和程序栈上的引用出发，遍历以对象为节点、以引用为边构成的图，把所有可以访问到的对象打上标记，然后清扫一遍内存空间，把所有没标记的对象释放。

**3** **分代技术**

分代回收的整体思想是：将系统中的所有内存块根据其存活时间划分为不同的集合，每个集合就成为一个“代”，垃圾收集频率随着“代”的存活时间的增大而减小，存活时间通常利用经过几次垃圾回收来度量。

Python默认定义了三代对象集合，索引数越大，对象存活时间越长。

举例：
当某些内存块M经过了3次垃圾收集的清洗之后还存活时，我们就将内存块M划到一个集合A中去，而新分配的内存都划分到集合B中去。当垃圾收集开始工作时，大多数情况都只对集合B进行垃圾回收，而对集合A进行垃圾回收要隔相当长一段时间后才进行，这就使得垃圾收集机制需要处理的内存少了，效率自然就提高了。在这个过程中，集合B中的某些内存块由于存活时间长而会被转移到集合A中，当然，集合A中实际上也存在一些垃圾，这些垃圾的回收会因为这种分代的机制而被延迟。



## 10.**read,readline**和**readlines**

- read 读取整个文件
- readline 读取下一行,使用生成器方法
- readlines 读取整个文件到一个迭代器以供我们遍历