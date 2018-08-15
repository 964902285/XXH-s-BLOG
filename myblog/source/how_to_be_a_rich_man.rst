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
    
执行结果：
	<Person: Piglei(24)>
这样便是__init__最普通的用法了。但__init__其实不是实例化一个类的时候第一个被调用 的方法。当使用 Persion(name, age) 这样的表达式来实例化一个类时，最先被调用的方法 其实是 __new__ 方法。


1.2 __new__ 方法是什么？
---------------------

__new__方法接受的参数虽然也是和__init__一样，但__init__是在类实例创建之后调用，而 __new__方法正是创建这个类实例的方法。

class Person(object):
    def __new__(cls, name, age):
        print('这是__new__')
        return super(Person, cls).__new__(cls)

    def __init__(self, name, age):
        print('这是__init__')
        self.name = name
        self.age  = age

    def __str__(self):
        return '<Person: %s(%s)>' % (self.name, self.age)

if __name__ == '__main__':
    Piglei = Person('Piglei', 24)
    print(Piglei)

执行结果：
	这是__new__
	这是__init__
	<Person: Piglei(24)>
	
通过运行这段代码，我们可以看到，__new__方法的调用是发生在__init__之前的。其实当 你实例化一个类的时候，具体的执行逻辑是这样的：

1. p = Person(name, age)
2. 首先执行使用name和age参数来执行Person类的__new__方法，这个__new__方法会 返回Person类的一个实例（通常情况下是使用 super(Persion, cls).__new__(cls) 这样的方式），
3. 然后利用这个实例来调用类的__init__方法，上一步里面__new__产生的实例也就是 __init__里面的的 self


所以，__init__ 和 __new__ 最主要的区别在于：
1.__init__ 通常用于初始化一个新实例，控制这个初始化的过程，比如添加一些属性， 做一些额外的操作，发生在类实例被创建完以后。它是实例级别的方法。
2.__new__ 通常用于控制生成一个新实例的过程。它是类级别的方法。
但是说了这么多，__new__最通常的用法是什么呢，我们什么时候需要__new__？



