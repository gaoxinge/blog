下面我们研究python中的命名空间和作用域。

参考资料如下：

- [Python命名空间和作用域窥探](http://python.jobbole.com/81367)
- [Python LEGB 原则](https://zhuanlan.zhihu.com/p/25223919)
- [Python是一种纯粹的语言](https://zhuanlan.zhihu.com/p/23926957)

## 改变引用：一切皆变量

- 赋值

- 函数

- 类

- 模块

- 参数

## 命名空间

命名空间指的是键为名字，值为对象的字典。如

```python
a_namespace = {'name_a': object_1, 'name_b': object_2}
```

## 作用域

作用域是特殊的命名空间，反映了部分代码的引用状况。变量的作用域在编译时确定，值在运行时确定。

### 例子

```python
import requests

a = 1

def f(b):
    c = 1

    def g():
        d = 1
        return d

    return c

class foo:
    pass
```

作用域为

```python
g_namespace.keys() = ['d']
f_namespace.keys() = ['c', 'g']
global_namespace.keys()  = ['requests', 'a', 'f', 'foo']
builtins_namespace.keys() = ['__builtins__']
```

### 例子

```python
if True:
    a = 1

print a # 1
```

```python
for x in range(4):
    print x

print x # 3
```

## LEGB

LEGB是变量搜索作用域是的顺序：local（当前作用域）--->enclosure--->global---->builtins。变量搜索在运行时确定。

### 例子

```python
def f():
    a = b
    return a
```

### 例子

```python
def multipliers(n):
    funcs = []
    for i in range(0, n):
        def _func(b):
            return i * b
        funcs.append(_func)
    return funcs

ms = multipliers(10)
print ms[2](3) # 27
```

```python
def multipliers(n):
    funcs = []
    for i in range(0, n):
        def _func(b):
            return i * b
        funcs.append(_func(3)))
    return funcs

ms = multipliers(10)
print ms[2] # 6
```

### 例子

```python
class A:
    a = 1
    
    class B:
        b = a
```

```python
class A:
    a = 1
    
    class B:
        b = A.a
```

```python
class A:
    a = 1
    
    class B:
        def __init__(self):
            b = A.a
```

## locals和globals

### 当前作用域

类中的当前作用域比较奇怪。

### locals

locals()返回当前作用域。

### globals

globals()返回global_namespace和builtins_namespace。
