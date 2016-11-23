整理了董伟明的知乎回答。

- [Python开发中有哪些高级技巧](https://www.zhihu.com/question/23760468/answer/125478261)
- [Python有哪些优雅的代码实现](https://www.zhihu.com/question/37751951/answer/125640796)

## 上下文管理器

Bad:

```python
f = open('tmp/a', 'a')
f.write('hello world')
f.close()
```

Good:

```python
with open('/tmp/a', 'a') as f:
    f.write('hello world')
```

Bad:

```python
class OpenContext(object):
    def __init__(self, filename, mode):
        self.fp = open(filename, mode)

    def __enter__(self):
        return self.fp

    def __exit__(self, exc_type, exc_val, exc_tb):
        self.fp.close()

with OpenContext('/tmp/a', 'a') as f:
    f.write('hello world')
```

Good:

```python
from contextlib import contextmanager

@contextmanager
def make_open_context(filename, mode):
    fp = open(filename, mode)
    try:
        yield fp
    finally:
        fp.close()

with make_open_context('/tmp/a', 'a') as f:
    f.write('hello world')
```

## total_ordering

```python
from functools import total_ordering

@total_ordering
class Size(object):
    def __init__(self, value):
        self.value = value

    def __lt__(self, other):
        return self.value < other.value

    def __eq__(self, other):
        return self.value == other.value
```

## inspect

```python
import inspect

def add(a, b=1):
    return a + b

print inspect.getsourcelines(add)
print inspect.getargspec(add)
print inspect.getcallargs(add, 10, 2)
print inspect.isclass(add)
print inspect.isfunction(add)
```

## mixin

具体可参见[Python mixin模式](http://python.jobbole.com/84052)。

Bad:

```python
class SimpleItemContainer(object):
    def __init__(self, id, item_containers):
        self.id = id
        self.data = {}
        for item in item_containers:
            self.data[item.id] = item
```

Good:

```python
from UserDict import DictMixin

class MyDict(DictMixin):
    def __init__(self, dict=None, **kwargs):
        self.data = {}
        if dict is not None:
            self.update(dict)
        if len(kwargs):
            self.update(kwargs)
            
    def __getitem__(self, id):
        return self.data[id]
        
    def __setitem__(self, id, value):
        self.data[id] = value
        
    def __delitem__(self, id):
        del self.data[id]
        
    def keys(self):
        return self.data.keys()
```

```python
from UserDict import DictMixin

class BetterSimpleItemContainer(object, DictMixin):
    def __getitem__(self, id):
        return self.data[id]
        
    def __setitem__(self, id, value):
        self.data[id] = value
        
    def __del__(self, id):
        del self.data[id]
        
    def keys(self):
        return self.data.keys()
```

```python
class CommonEqualityMixin(object):
    def __eq__(self, other):
        return isinstance(other, self.__class__)
                and self.__dict__ == other.__dict__
                
    def __ne__(self, other):
        return not self.__eq__(other)
        
class Foo(commonEqualityMixin):
    def __init__(self, item):
        self.item = item
```



