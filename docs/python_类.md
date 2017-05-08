下面我们研究python中的类。

参考资料如下：

- [Python进阶：实例讲解Python中的魔法函数（Magic Methods）](https://zhuanlan.zhihu.com/p/24567545)
- [Python的Magic Methods指南](http://python.jobbole.com/84084)
- [Python魔术方法指南](http://pycoders-weekly-chinese.readthedocs.io/en/latest/issue6/a-guide-to-pythons-magic-methods.html)
- [Python __getattribute__ vs __getattr__ 浅谈](http://python.jobbole.com/84095)
- [如何理解Python的Descriptor](https://www.zhihu.com/question/25391709/answer/30634637)
- [如何理解Python的Descriptor](https://www.zhihu.com/question/25391709/answer/30715222)
- [Python Unbound/Bound method object](http://www.jianshu.com/p/4b871019ef96)
- [Python中的classmethod和staticmethod有什么具体用途](https://www.zhihu.com/question/20021164/answer/18224953)
- [Python中的method](http://blog.jobbole.com/53989)

## 实例的属性访问

```python
def __getattribute__(self, item):
    return super(A, self).__getattribute__(item)

def __getattribute__(self, item):

    def class_lookup(cls, item):
        v = cls.__dict__.get(item)
        if v is not None:
            return v, cls
        for x in cls.__bases__:
            v, c = class_lookup(x, item)
            if v is not None:
                return v, c
        return None, None

    v, cls = class_lookup(type(self), item)
    if (v is not None) and hasattr(v, '__get__') and hasattr(v, '__set__'):
        return v.__get__(self, cls)
    w = self.__dict__.get(item)
    if w is not None:
        return w
    if v is not None:
        if hasattr(v, '__get__'):
            return v.__get__(self, cls)
        else:
            return v
    raise AttributeError(type(self), item)
```

函数是代码对象的包装，实例方法，类方法，静态方法是函数的包装。这影响类调用方法的方式。

```python
class ClassMethod(object):
    
    def __init__(self, f):
        self.f = f

    def __get__(self, obj, typ=None):
        return self.f.__get__(typ, type)

class StaticMethod(object):

    def __init__(self, f):
        self.f = f

    def __get__(self, obj, typ=None):
        return self.f

class Property(object):

    def __init__(self, fget=None, fset=None, fdel=None, doc=None):
        self.fget = fget
        self.fset = fset
        self.fdel = fdel
        
        if not doc and not fget:
            doc = fget.__doc__
        
        self.__doc__ = doc
        
    def __get__(self, obj, typ=None):
        if not obj:
            return self
        if not self.fget:
            raise AttributeError('unreadable attribute')
        return self.fget(obj)
        
    def __set__(self, obj, value):
        if not self.fset:
            raise AttributeError('can\'t set attribute')
        self.fset(obj, value)
        
    def __delete__(self, obj):
        if not self.fdel:
            raise AttributeError('can\'t delete attribute')
        self.fdel(obj)
        
    def getter(self, fget):
        return self.__class__(fget, self.fset, self.fdel, self.__doc__)
    
    def setter(self, fset):
        return self.__class__(self.fget, fset, self.fdel, self.__doc__)
        
    def deleter(self, fdel):
        return self.__class__(self.fget, self.fset, fdel, self.__doc__)
```

```python
def F(self):
    print 1
f = F

def G(cls):
    print 1
g = classmethod(G)

def H():
    print 1
h = staticmethod(H)

class A(object):
    pass

A.f = f # 实例方法
A.g = g # 类方法
A.h = h # 静态方法

a = A()
# a.f() = a.__class__.__dict__['f'].__get__(a, A)() = f.__get__(a, A)() = f(a)
# a.g() = a.__class__.__dict__['g'].__get__(a, A)() = h.__get__(a, A)() = g(A)
# a.h() = a.__class__.__dict__['h'].__get__(a, A)() = g.__get__(a, A)() = h()
```

## __init__和__new__

- [Python中__init__和__new__的区别详解](http://www.ynpxrz.com/n685992c2025.aspx)

```python
'''
basic
'''
class Person(object):
    
    def __init__(self, name, age):
        self.name = name
        self.age = age
        
    def __str__(self):
        return '<Person: %s(%s)>' % (self.name, self.age)

class Person1(object):

    def __new__(cls, name, age):
        print '__new__ called'
        return super(Person1, cls).__new__(cls, name, age)
    
    def __init__(self, name, age):
        print '__init__ called'
        self.name = name
        self.age = age
        
    def __str__(self):
        return '<Person: %s(%s)>' % (self.name, self.age)
        
piglei = Person('piglei', 24)
print piglei
piglei = Person1('piglei', 24)
print piglei        

'''
immutable object
'''
class PositiveInteger(int):
    
    def __init__(self, value):
        super(PositiveInteger, self).__init__(self, abs(value))
        
class PositiveInteger1(int):
    
    def __new__(cls, value):
        return super(PositiveInteger1, cls).__new__(cls, abs(value))
        
i = PositiveInteger(-3)
print i
i = PositiveInteger1(-3)
print i

'''
metaclass
'''
class A(type):
    
    def __init__(self, a):
        self.a = a

a = A(1)
# print a
    
'''
singleton
''' 
class Singleton(object):
    
    def __new__(cls):
        if not hasattr(cls, 'instance'):
            cls.instance = super(Singleton, cls).__new__(cls)
        return cls.instance
        
print hasattr(Singleton, 'instance')
obj1 = Singleton()
print hasattr(Singleton, 'instance')
obj2 = Singleton()
obj1.attr1 = 'value1'
print obj1.attr1, obj2.attr1
print obj1 is obj2
```