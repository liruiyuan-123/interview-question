# interview-question
<!-- 面试题 -->
1.Python里的拷贝

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

2.鸭子类型

“当看到一只鸟走起来像鸭子、游泳起来像鸭子、叫起来也像鸭子，那么这只鸟就可以被称为鸭子。”
我们并不关心对象是什么类型，到底是不是鸭子，只关心行为。
比如在python中，有很多file-like的东西，比如StringIO,GzipFile,socket。它们有很多相同的方法，我们把它们当作文件使用。
又比如list.extend()方法中,我们并不关心它的参数是不是list,只要它是可迭代的,所以它的参数可以是list/tuple/dict/字符串/生成器等.
鸭子类型在动态语言中经常使用，非常灵活，使得python不像java那样专门去弄一大堆的设计模式。

3.字典推导式
可能你见过列表推导时,却没有见过字典推导式,在2.7中才加入的:
d = {key: value for (key, value) in iterable}

4.单例模式

​ 单例模式是一种常用的软件设计模式。在它的核心结构中只包含一个被称为单例类的特殊类。通过单例模式可以保证系统中一个类只有一个实例而且该实例易于外界访问，从而方便对实例个数的控制并节约系统资源。如果希望在系统中某个类的对象只能存在一个，单例模式是最好的解决方案。

__new__()在__init__()之前被调用，用于生成实例对象。利用这个方法和类的属性的特点可以实现设计模式的单例模式。单例模式是指创建唯一对象，单例模式设计的类只能实例
<这个绝对常考啊.绝对要记住1~2个方法,当时面试官是让手写的>

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


5.Python中的作用域
Python 中，一个变量的作用域总是由在代码中被赋值的地方所决定的。
当 Python 遇到一个变量的话他会按照这样的顺序进行搜索：
    本地作用域（Local）→当前作用域被嵌入的本地作用域（Enclosing locals）→
    全局/模块作用域（Global）→内置作用域（Built-in）