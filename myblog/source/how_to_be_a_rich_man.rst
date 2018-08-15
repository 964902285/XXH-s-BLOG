01 详解Python中的__init__和__new__的区别
======================

1.1 __init__ 方法是什么？
---------------------

使用Python写过面向对象的代码的同学，可能对 __init__ 方法已经非常熟悉了，__init__ 方法通常用在初始化一个类实例的时候。例如：

class Person(object):
    def __init__(self, name, age):
        self.name = name
        self.age  = age

    def __str__(self):
        return '<Person: %s(%s)>' % (self.name, self.age)

if __name__ == '__main__':
    Piglei = Person('Piglei', 24)
    print(Piglei)

这样便是__init__最普通的用法了。但__init__其实不是实例化一个类的时候第一个被调用 的方法。当使用 Persion(name, age) 这样的表达式来实例化一个类时，最先被调用的方法 其实是 __new__ 方法。


1.2 财富共享法
---------------------

有个有钱的老婆。
