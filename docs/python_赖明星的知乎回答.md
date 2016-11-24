整理了赖明星的知乎回答。

- [怎么样才算是精通Python](https://www.zhihu.com/question/19794855/answer/129270643)
- [怎样才能写出pythonic的代码](https://www.zhihu.com/question/21408921/answer/129036707)
- [Python有哪些优雅的代码实现](https://www.zhihu.com/question/37751951/answer/73425339)

## python之禅

```python
>>> import this
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
```

## 切片

切片操作由三部分组成：起点，终点，步长。

```python
>>> L = [1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> L[2:-2]
[3, 4, 5, 6, 7]
>>> L[-2:2] # 表示L[-2:2:1]
[]
>>> L[-2:2:-1]
[8, 7, 6, 5, 4]
```

## 列表推导式

```python
>>> [i*i for i in range(1, 21) if i% 2 == 0]
[4, 16, 36, 64, 100, 144, 196, 256, 324, 400] # 列表
>>> {i*i for i in range(1, 21) if i% 2 == 0}
set([64, 144, 36, 100, 324, 256, 16, 400, 196, 4]) # 集合
>>> {i:i*i for i in range(1, 21) if i% 2 == 0}
{2: 4, 4: 16, 6: 36, 8: 64, 10: 100, 12: 144, 14: 196, 16: 256, 18: 324, 20: 400} # 字典

>>> [item for item in os.listdir(os.path.expanduser('.')) if os.path.isfile(item)]
>>> [item for item in os.listdir(os.path.expanduser('.')) if os.path.isdir(item)]
>>> {item: os.path.realpath(item) for item in os.listdir(os.path.expanduser('.'))}
```

## 内置函数

- sum
- sort
- max
- min
- any

```python
any(...)
    any(iterable) -> bool
    
    Return True if bool(x) is True for any x in the iterable.
    If the iterable is empty, return False.
```

- all

```python
all(...)
    all(iterable) -> bool
    
    Return True if bool(x) is True for all values x in the iterable.
    If the iterable is empty, return True.
```

- enumerate

```python
L = [i*i for i in range(5)]

for index, data in enumerate(L):
    print index + 1, ':', data

for index, data in enumerate(L,1):
    print index, ':', data
```

- reversed

```python
L = [1, 2, 3, 4]

for item in L[::-1]:
    print item

for item in reversed(L):
    print item
```

## 交换

Bad:

```python
t = a
a = b
b = t
```

Good:

```python
a, b = b, a
```

## 上下文管理器

Bad:

```python
try:
    f = open('/path/to/file', 'r')
    print f.read()
finally:
    if f:
        f.close()
```

Good:

```python
with open('/path/to/file', 'r') as f:
    print f.read()
```

## 报错

Bad:

```python
import sys
sys.stderr.write('It failed!')
raise SystemExit(1)
```

Good:

```python
raise SystemExit('It failed!')
```

## 装饰器

Bad:

```python
class Store(object):
    def get_food(self, username, food):
        if username != 'admin':
            raise Exception("This user is not allowed to get food")
        return self.storage.get(food)

    def put_food(self, username, food):
        if username != 'admin':
            raise Exception("This user is not allowed to put food")
        self.storage.put(food)
```

Better:

```python
def check_is_admin(username):
    if username != 'admin':
        raise Exception("This user is not allowed to get food")

class Store(object):
    def get_food(self, username, food):
        check_is_admin(username)
        return self.storage.get(food)

    def put_food(self, username, food):
        check_is_admin(username)
        return self.storage.put(food)
```

Good:

```python
def check_is_admin(f):
    def wrapper(*args, **kwargs):
        if kwargs.get('username') != 'admin':
            raise Exception("This user is not allowed to get food")
        return f(*arg, **kargs)
    return wrapper

class Storage(object):
    @check_is_admin
    def get_food(self, username, food):
        return self.storage.get(food)

    @check_is_admin
    def put_food(self, username, food):
        return storage.put(food)
```

## 动态类型语言

Bad:

```python
def a():
    return 'a'

def b():
    return 'b'

def c():
    return 'c'

cmd = raw_input('> ')
if cmd == 'a':
    print a()
elif cmd == 'b':
    print b()
elif cmd == 'c':
    print c()
else:
    raise Exception('Action not found')
```

Good:

```python
class A:
    def fetch_func(self, action):
        func= getattr(self, action, None)
        return func

    def execute(self, action):
        func= self.fetch_func(action)
        if func is None:
            return False
        return func()

    def a(self):
        return 'a'

    def b(self, msg):
        return 'b'

    def c(self, msg):
        return 'c'

i = A()
cmd = raw_input('> ')
result = i.execute(cmd)
if result:
    print result
else:
    raise Exception('Action not found')
```