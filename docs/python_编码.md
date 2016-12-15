具体可参见

- [Python编码为什么那么蛋疼](https://www.zhihu.com/question/31833164)
- [Python中文处理系列之源代码与文件IO](https://zhuanlan.zhihu.com/p/20179963?refer=python-dev)

## 字符集与编码

字符集有

- 26个字母（大小写）、10个数字、标点符号、控制符
- GB-18030，GB-2312：中国国家标准，前者是后者的扩展
- GBK：中国全国信息技术标准化技术委员会发布的文件，是GB-2312的扩展，兼容第一项
- Unicode：国际标准，规定于 ISO/IEC 10646
- GB-13000：中国国家标准，为Unicode的子集

编码有

- ASCII
- GB-18030，GBK，GB-2312：事实上采用EUC-CN
- Unicode，GB-13000：采用多种编码方案，常用的有UTF-8，UTF-16等；这其中，UTF-8由于对ASCII良好的兼容性，广泛用于程序代码、网页设计等中英混合的场合

## 文件的编码

```
     编码       解码
文件 ----> 内存 ----> 文件
```

- 记事本：在另存为时可以选择
- Notepad++：在格式中可以选择

## python的编码

```
basestring子类：str类和unicode类
str类：print使用str，交互使用repr
unicode类：print使用unicode，交互使用repr

          编码        解码
unicode类 ----> str类 ----> unicode类
```

- idle：默认环境ascii（sys.getdefaultencoding()），需要添加#coding=utf-8（PEP 263）
- command line：默认环境gbk（sys.getdefaultencoding()）

```python
#coding=utf-8

a = '你好'
print a
a = u'你好'
print a
```

```python
#coding=gbk

a = '你好'
print a
a = u'你好'
print a
```

- 非国际字符：无需加u
- 国际字符：加#coding=utf-8，加u

## 写文件

```python
#coding=utf-8

a = '你好'
with open('test.txt', 'w') as f:
    f.write(a)
```

```python
#coding=utf-8

a = u'你好'
with open('test.txt', 'w') as f:
    f.write(a)
```

```python
#coding=gbk

a = '你好'
with open('test.txt', 'w') as f:
    f.write(a)
```

```python
#coding=gbk

a = u'你好'
with open('test.txt', 'w') as f:
    f.write(a)
```

由于文件的写入必须使用str类，因此unicode必须先编码，再写入，可以考虑下面一些方法：

```
#coding=utf-8

a = u'你好'
with open('test.txt', 'w') as f:
    f.write(a.encode('utf-8'))
```

```
#coding=utf-8

a = u'你好'
with open('test.txt', 'w') as f:
    f.write(a.encode('gbk'))
```

```
#coding=utf-8
import codecs

a = u'你好'
with open('test.txt', encoding='utf-8', mode='w') as f:
    f.write(a)
```

```
#coding=utf-8
import codecs

a = u'你好'
with open('test.txt', encoding='gbk', mode='w') as f:
    f.write(a)
```

## 读文件

和写文件一样，python读取的字符串为str类，要想转化为unicode类，必须事先知道文件的编码：utf-8对应的是utf-8，ANSI对应的是gbk

## 读网页

和读文件不一样，python读取的字符串为unicode类，要想转化为str类，可以查看response.encoding