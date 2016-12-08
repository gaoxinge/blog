整理了董伟明的知乎回答。

- [Python开发中有哪些高级技巧](https://www.zhihu.com/question/23760468/answer/125478261)
- [Python有哪些优雅的代码实现](https://www.zhihu.com/question/37751951/answer/125640796)

## 直到找到符合的结果

Bad:

```python
a = -1
for i in range(1, 10):
    if not i % 4:
        a = i
        break
```

Good:

```python
a = next((i for i in range(1, 10) if not i % 4), -1)
```

## 直到出现不符合的结果

Bad:

```python
blocks = []
while True:
    block = f.read(32)
    if block == '':
        break
    blocks.append(block)
```

Good:

```python
from functools import partial

blocks = []
for block in iter(partial(f.read, 32), ''):
    blocks.append(block)
```

## 标记区分

Bad:

```python
def prime(n):
    for i in range(2, n-1):
        if n % i == 0:
            return i
    return -1
```

Good:

```python
def prime(n):
    for i in range(2, n-1):
        if n % i == 0:
            break
    else:
        return -1
    return i
```

## 字典

Bad:

```python
t = {}
t['a'] = {}
t['a']['b'] = 1 
t['c'] = 1
t['d'] = {}
t['d']['e'] = {}
t['d']['e']['f'] = 10
```

Good:

```python
from collections import defaultdict

tree = lambda: defaultdict(tree)
t = tree()
t['a']['b'] = 1
t['c'] = 1
t['d']['e']['f'] = 10
```

## 上下文管理器

### 文件

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

### 锁

Bad:

```python
import threading
import time

def print_time(threadName, delay, counter):
    lock.acquire()
    print 'Staring', threadName
    lock.release()
    while counter:
        time.sleep(delay)
        lock.acquire()
        print '%s: %s' % (threadName, time.ctime(time.time()))
        lock.release()
        counter -= 1
    lock.acquire()
    print 'Exiting', threadName
    lock.release()

lock = threading.Lock()

t1 = threading.Thread(target=print_time, args=('Thread-1', 1, 5,))
t2 = threading.Thread(target=print_time, args=('Thread-2', 2, 5,))

t1.start()
t2.start()

t1.join()
t2.join()

print 'Exiting Main Thread'
```

Good:

```python
import threading
import time

def print_time(threadName, delay, counter):
    with lock:
        print 'Staring', threadName
    while counter:
        time.sleep(delay)
        with lock:
            print '%s: %s' % (threadName, time.ctime(time.time()))
        counter -= 1
    with lock:
        print 'Exiting', threadName

lock = threading.Lock()

t1 = threading.Thread(target=print_time, args=('Thread-1', 1, 5,))
t2 = threading.Thread(target=print_time, args=('Thread-2', 2, 5,))

t1.start()
t2.start()

t1.join()
t2.join()

print 'Exiting Main Thread'
```

### 异常

Bad:

```python
import os

try:
    os.remove('somefile.tmp')
except OSError:
    print 'no file'
```

Good:

```python
import os
from contextlib import contextmanager

@contextmanager
def ignored(*exceptions):
    try:
        yield
    except exceptions:
        print 'no file'

with ignored(OSError):
    os.remove('somefile.tmp')
```

## 缓存

Bad:

```python
import requests

def web_lookup(url, saved={}):
    if url in saved:
        return saved[url]
    page = requests.get(url)
    saved[url] = page
    return page
```

Better:

```python
import requests

def cache(func):
    saved = {}
    @wraps(func)
    def newfunc(*args):
        if args in saved:
            return saved[args]
        result = func(*args)
        saved[args] = result
        return result
    return newfunc

@cache
def web_lookup(url):
    return requests.get(url)
```

Good:

```
class cached_property(property):
    def __init__(self, func, name=None, doc=None):
        self.__name__ = name or func.__name__
        self.__module__ = func.__module__
        self.__doc__ = doc or func.__doc__
        self.func = func

    def __set__(self, obj, value):
        obj.__dict__[self.__name__] = value

    def __get__(self, obj, type=None):
        if obj is None:
            return self
        value = obj.__dict__.get(self.__name__, _missing)
        if value is _missing:
            value = self.func(obj)
            obj.__dict__[self.__name__] = value
        return value
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

class Item(DictMixin):
    def __init__(self):
        self.data = {}
        
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
class CommonEqualityMixin(object):
    def __eq__(self, other):
        return isinstance(other, self.__class__) and self.__dict__ == other.__dict__
                
    def __ne__(self, other):
        return not self.__eq__(other)
        
class Foo(CommonEqualityMixin):
    def __init__(self, item):
        self.item = item
```