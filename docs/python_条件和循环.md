下面我们研究python中的条件和循环。

参考资料如下：

- [自定义序列](http://www.cnblogs.com/scolia/p/5690210.html)
- [Iterables vs. Iterators vs. Generators](http://nvie.com/posts/iterators-vs-generators)

## for

使用的是可迭代对象。

## 容器

实现了__getitem__方法。

- 列表推导式

```python
class Foo(object):
    def __init__(self):
        self.x = {}
        self.__index = -1

    def __setitem__(self, key, value):
        self.x[key] = value 

    def __getitem__(self, key):
        self.__index += 1
        if self.__index == len(self.x):
            self.__index = -1
            raise IndexError # raise StopIteration or raise StopIteration()
        return self.x[self.__index]
    
foo = Foo()
foo[0] = 1
foo[1] = 2
foo[2] = 3
for i in foo:
    print i
```

## 可迭代对象

实现了__iter__方法。

## 迭代器

实现了next方法。

## 生成器

不通过__iter__和next实现，只可以迭代一次。

- 生成器表达式

- 生成器函数

