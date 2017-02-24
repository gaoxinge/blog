下面我们研究python中的装饰器。

参考资料如下：

- [Python进阶: 通过实例详解装饰器（附代码）](https://zhuanlan.zhihu.com/p/23510985)

## 函数式创建

```python
def Decorator(f):
    def wrapper(*args, **kwargs):
        print '%s called' % f.__name__
        result = f(*args, **kwargs)
        print '%s end' % f.__name__
        return result
    return wrapper

@Decorator
def test06(a, b, c):
    print 'in function test06, a=%s, b=%s, c=%s' % (a, b, c)
    return 1

print test06(1,2,3)
```

## 类式创建

```python
class Decorator(object):

    def __init__(self, f):
        self.f = f

    def __call__(self, *args, **kwargs):
        print '%s called' % self.f.__name__
        result = self.f(*args, **kwargs)
        print '%s end' % self.f.__name__
        return result
    
@Decorator
def test06(a, b, c):
    print 'in function test06, a=%s, b=%s, c=%s' % (a, b, c)
    return 1

print test06(1,2,3)
```

