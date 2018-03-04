c语言由Thompson和Ritchie开发。

## 标准

- 1989年，ANSI发布了ANSI C，定义了语言和标准库，称为C89。
- 1990年，ISO发布了C90。
- 1999年，ISO发布了C99。
- 2011年，ISO发布了C11。

## 编译器

c的编译器主要有

- gcc：GNU
- cl：Microsoft
- clang

## UNIX

UNIX自带编译器cc，不属于上述的任何一种。

## Linux

Linux自带编译器gcc，下面说一下注意事项

- 后缀c
- 命令

```
cc/gcc/g++ test.c # 生成a.out
./a.out           # 运行

cc/gcc/g++ -o test test.c # 生成test
./test                    # 运行
```

- 查看main函数中的return

```
echo $?
```

## Windows

Windows无自带编译器，推荐使用vc，vs等ide，提供编译器cl，但由于体积过大，本人并没有安装。

本人安装了MinGW+Notepad++，即使用gcc，下面是注意事项

- 后缀c
- 命令

```
gcc/g++ test.c # 生成a.exe
a/a.exe        # 运行

gcc/g++ -o test test.c # 生成test.exe
test/test.exe          # 运行
```

- 查看main函数中的return

```
echo %ERRORLEVEL%
```

## Mac

Mac无自带编译器，推荐使用集成环境Xcode，其提供多种c编译器。

## 其他

- [Linux 系统结构详解](http://blog.jobbole.com/103511/?utm_source=blog.jobbole.com&utm_medium=relatedPosts)
- [C程序编译过程浅析](http://blog.jobbole.com/109399/?utm_source=blog.jobbole.com&utm_medium=relatedPosts)
- [通过这9本开源好书学习C语言](http://blog.jobbole.com/110148/?utm_source=blog.jobbole.com&utm_medium=relatedPosts)
- [C语言有什么奇技淫巧？](https://www.zhihu.com/question/27417946)
- [用C语言写一个简单的Unix Shell](http://blog.jobbole.com/111738/?utm_source=blog.jobbole.com&utm_medium=relatedPosts)