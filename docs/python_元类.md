下面我们研究python中的元类。

参考资料如下：

- [Python进阶：一步步理解Python中的元类metaclass](https://zhuanlan.zhihu.com/p/23887627)
- [Python·元类（Meta Class）及其应用](https://zhuanlan.zhihu.com/p/24633374)
- [Python·元类（Meta Class）及其应用（二）](https://zhuanlan.zhihu.com/p/25184903)

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
