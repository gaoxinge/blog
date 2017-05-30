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

## 数据结构

### 元组

```python
from collections import namedtuple

DiskDevice = namedtuple('DiskDevice', ['major_number minor_number device_name read_count read_merged_count',
                                       'read_sections time_spent_reading write_count write_merged_count ',
                                       'write_sections time_spent_write io_requests time_spent_doing_io',
                                       ' weighted_time_spent_doing_io'])

def get_disk_info(disk_name):
    with open('/proc/diskstats') as f:
        for line in f:
            if line.split()[2] == disk_name:
                return DiskDevice(*(line.split()))
```

### 字典

Bad:

```python
port = kwargs.get('port')
if port is None:
    port = 3306
```

Good:

```python
port = kwargs.get('port', 3306)
```

Bad:

```python
d = {}
for key, value in pairs:
    if key not in d:
        d[key] = []
    d[key].append(value)
```

Good:

```python
from collections import defaultdict

d = defaultdict(list)
for key, value in pairs:
    d[key].append(value)
```

Bad:

```python
d = {}
with open('/etc/passwd') as f:
    for line in f:
        for word in line.strip().split(':'):
            if word not in d:
                d[word] = 0
            d[word] += 1
```

Better:

```python
from collections import defaultdict

d = defaultdict(int)
with open('/etc/passwd') as f:
    for line in f:
        for word in line.strip().split(':'):
            d[word] += 1
```

Good:

```python
from collections import Counter

word_counts = Counter()
with open('test.txt') as f:
    for line in f:
        word_counts.update(line.strip().split(':'))
```

Bad:

```python
result = sorted(zip(d.values(), d.keys()), reverse=True)[:3]
for val, key in result:
    print  key, ':', val
```

Good:

```python
for key, val in word_counts.most_common(3):
    print key, ':', val
```

### 时间复杂度

```
list              --- 数组，判断元素有无：慢
collections.deque --- 双向链表
set               --- hash表，判断元素有无：快
```

## else

```python
if xxx:
    pass
else:
    pass
```

```python
while xxx:
    pass
else:
    pass
```

```python
try:
    pass
except:
    pass
else:
    pass
finally:
    pass
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

## 全局变量

```python
import sys
import test

a = 1

def func1():
    global a
    a += 1

def func2():
    test.a += 1

def func3():
    module = sys.modules['test']
    module.a += 1

func1()
func2()
func3()
```

## 上下文管理器

### 文件

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

Bad:

```python
with open('data.txt') as source:
    with open('target.txt', 'w') as target:
        target.write(source.read())
```

Good:

```python
with open('data.txt') as source, open('target.txt', 'w') as target:
    target.write(source.read())
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

### 游标

```python
import MySQLdb

db = MySQLdb.connect('localhost', 'root', 'password')
with db as cursor:
    cursor.execute('show databases')
    print cursor.fetchall()
```

### 运算精度

```python
import decimal

with decimal.localcontext() as ctx:
    ctx.prec = 22
    print decimal.getcontext().prec
```

## 并发

Bad:

```python
import time
import random
from threading import Thread, Condition
    
queue = []
MAX_NUM = 10
condition = Condition()

class ProducerThread(Thread):
    def run(self):
        nums = range(5)
        global queue
        while True:
            condition.acquire()
            if len(queue) == MAX_NUM:
                print "Queue full, producer is waiting"
                condition.wait()
                print "Space in queue, Consumer notified the producer"
            num = random.choice(nums)
            queue.append(num)
            print "Produced", num
            condition.notify()
            condition.release()
            time.sleep(random.random())


class ConsumerThread(Thread):
    def run(self):
        global queue
        while True:
            condition.acquire()
            if not queue:
                print "Nothing in queue, consumer is waiting"
                condition.wait()
                print "Producer added something to queue and notified the consumer"
            num = queue.pop(0)
            print "Consumed", num
            condition.notify()
            condition.release()
            time.sleep(random.random())


ProducerThread().start()
ConsumerThread().start()
```

Good:

```python
import time
import random
from Queue import Queue
from threading import Thread

queue = Queue(10)

class ProducerThread(Thread):
    def run(self):
        nums = range(5)
        global queue
        while True:
            num = random.choice(nums)
            queue.put(num)
            print "Produced", num
            time.sleep(random.random())

class ConsumerThread(Thread):
    def run(self):
        global queue
        while True:
            num = queue.get()
            queue.task_done()
            print "Consumed", num
            time.sleep(random.random())

ProducerThread().start()
ConsumerThread().start()
```

```python
import requests
from multiprocessing import Pool
    
def get_website_data(url):
    print requests.get(url)

if __name__ == '__main__':
    pool = Pool(2)
    urls = [
        'http://www.google.com',
        'http://www.baidu.com',
        'http://www.163.com'
    ]
    pool.map(get_website_data, urls)
```

```python
import requests
from requests.exceptions import ConnectionError
from multiprocessing.dummy import Pool as ThreadPool

def scrape(url):
    try:
        print requests.get(url)
    except ConnectionError:
        print 'Error Occured', url
    finally:
        print 'URL', url, 'Scraped'

pool = ThreadPool(processes=3)
urls = [
    'https://www.baidu.com',
    'http://www.meituan.com',
    'http://blog.csdn.net',
    'http://xxxyxxx.net'
]
pool.map(scrape, urls)
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

Bad:

```python
def is_admin(f):
    def wrapper(*args, **kwargs):
        if kwargs.get('username') != 'admin':
            raise Exception("This user is not allowed to get food")
        return f(*args, **kwargs)
    return wrapper


@is_admin
def barfoo(username='someone'):
    '''Do crazy stuff'''
    pass

print barfoo.func_doc
print barfoo.__name__
```

Good:

```python
import functools

def is_admin(f):
    @functools.wraps(f)
    def wrapper(*args, **kwargs):
        if kwargs.get("username") != 'admin':
            raise Exception("This user is not allowed to get food")
        return f(*args, **kwargs)
    return wrapper


@is_admin
def barfoo(username='someone'):
    '''Do crazy stuff'''
    pass

print barfoo.func_doc
print barfoo.__name__
```

Bad:

```python
import functools

def check_is_admin(f):
    @functools.wraps(f)
    def wrapper(*args, **kwargs):
        if kwargs.get("username") != 'admin':
            raise Exception("This user is not allowed to get food")
        return f(*args, **kwargs)
    return wrapper


@check_is_admin
def get_food(username, food='chocolate'):
    return "{0} get food: {1}".format(username, food)

print get_food('admin')
```

Good:

```python
import functools
import inspect

def check_is_admin(f):
    @functools.wraps(f)
    def wrapper(*args, **kwargs):
        func_args = inspect.getcallargs(f, *args, **kwargs)
        if func_args.get("username") != 'admin':
            raise Exception("This user is not allowed to get food")
        return f(*args, **kwargs)
    return wrapper


@check_is_admin
def get_food(username, food='chocolate'):
    return "{0} get food: {1}".format(username, food)

print get_food('admin')
```

```python
import inspect
import functools

def check_args(parameters):
    def decorated(f):
        @functools.wraps
        def wrapper(*args, **kwargs):
            func_args = inspect.getcallargs(f, *args, **kwargs)
            msg = func_args.get('msg')

            for item in parameters:
                if msg.body_dict.get(item) is None:
                    return False, "check failed, %s is not found" % item

            return f(*args, **kwargs)
        return wrapper
    return decorated

class AsyncMsgHandler(MsgHandler):
    @check.check_args(['ContainerIdentifier', 'MonitorSecretKey', "InstanceID", "UUID"])
    def init_container(self, msg):
        pass
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

## 设计模式

### 单例模式

```python
class Borg:
    _shared_state = {}

    def __init__(self):
        self.__dict__ = self.__shared_state
```

```python
def get():
    if not get.rates:
        _URL = 'http://www.bankofcanada.ca/stats/assets/csv/fx-seven-day.csv'
            with urllib.request.urlopen(_URL) as file:
                for line in file:
                    pass
    return get.rates
```

### 工厂模式

```python
class Shape:
    pass

class Circle(Shape):
    pass

class Square(Shape):
    pass

for name in ["Circle", "Square"]:
    cls = globals()[name]
    obj = cls()
```