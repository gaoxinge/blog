下面我们研究python中的元类。

参考资料如下：

- [Python进阶：一步步理解Python中的元类metaclass](https://zhuanlan.zhihu.com/p/23887627)
- [Python·元类（Meta Class）及其应用](https://zhuanlan.zhihu.com/p/24633374)
- [Python·元类（Meta Class）及其应用（二）](https://zhuanlan.zhihu.com/p/25184903)
- [Python元编程：控制你想控制的一切](https://zhuanlan.zhihu.com/p/29849145)
- [Python元类中的__call__和__new__的区别？](https://segmentfault.com/q/1010000007818814/a-1020000007829102)
- [通过 python的__call__函数与元类实现单例模式](http://www.cnblogs.com/blackmatrix/p/6906023.html)
- [深刻理解Python中的元类(metaclass)以及元类实现单例模式](http://www.cnblogs.com/tkqasn/p/6524879.html)
- [Python中的元类](http://www.cnblogs.com/wilber2013/p/4695836.html)

## 函数式创建

```python
def upper_attr(future_class_name, future_class_parents, future_class_attr):

    uppercase_attr = {}
    
    for name, val in future_class_attr.items():
        if not name.startswith('__'):
            uppercase_attr[name.upper()] = val
        else:
            uppercase_attr[name] = val

    return type(future_class_name, future_class_parents, uppercase_attr)
```
```python
class Foo(object):

    __metaclass__ = upper_attr
    
    bar = 'bip'

Foo = upper_attr('Foo', (object, ), {'bar': 'bip'})
```

## 类式创建

```python
class UpperAttrMetaClass(type):

    def __new__(cls, name, bases, attr):
        uppercase_attr = {}
        
        for name, val in attr.items():
            if not name.startswith('__'):
                uppercase_attr[name.upper()] = val
            else:
                uppercase_attr[name] = val

        return super(UpperAttrMetaClass, cls).__new__(cls, name, bases, uppercase_attr)
```

```python
class Foo(object):

    __metaclass__ = UpperAttrMetaClass

    bar = 'bip'

Foo = UpperAttrMetaClass('Foo', (object, ), {'bar': 'bip'})
```

## \_\_new\_\_, \_\_init\_\_, \_\_call\_\_

```python
class A(object):

    def __init__(x, a):
        x.a = a

    def fn():
        print 1

    def gn(x):
        print(x.a)

def fw():
    print 2

def gw(self):
    print(self.a)

a = A(1)
a.fn()     # TypeError: fn() takes exacyly no arguments (1 given)
a.gn()     # 1
a.fw = fw
a.gw = gw
a.fw()     # 2
a.gw()     # TypeError: gw() takes exactly 1 argument (0 given)
```

```python
class A(object):

    def __new__(x, b):
        x.b = b
        return x

a = A(1)
print(a is A)
print(a.b)
print(A.b)
```

## 装饰器

```python
from functools import wraps

def print_result(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        result = func(*args, **kwargs)
        print(result)
        return result
    return wrapper
    
@print_result
def add(x, y):
    return x + y
    
a = add(1, 3)

def attribute_upper(cls):
    for k, v in cls.__dict__.items():
        if isinstance(v, str):
            setattr(cls, k, v.upper())
    return cls
            
@attribute_upper
class Person(object):
    sex = 'man'
    
print(Person.sex)
```