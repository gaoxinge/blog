c++是c语言的继承，由Stroustrup开发。

## 标准

- 1989年，C++98。
- 2003年，C++03。
- 2011年，C++11。
- 2014年，C++14。

## 编译器

c++的编译器主要有

- g++：GNU
- cl：Microsoft
- clang

## UNIX

UNIX自带编译器cc， 不属于上述的任何一种。

## Linux

Linux自带编译器g++，下面说一下注意事项

- 后缀cc，cp，cpp，cxx，C
- 命令

```
cc/gcc/g++ test.cc # 生成a.out
./a.out            # 运行

cc/gcc/g++ -o test test.cc # 生成test
./test                     # 运行
```
- 查看main函数中的return

```
echo $?
```

## Windows

Windows无自带编译器，推荐使用vc，vs等ide，提供编译器cl，但由于体积过大，本人并没有安装。

本人安装了MinGW+Notepad++，即使用gcc，下面是注意事项

- 后缀cc，cp，cpp，cxx，C
- 命令

```
gcc/g++ test.cc # 生成a.exe
a/a.exe         # 运行

gcc/g++ -o test test.cc # 生成test.exe
test/test.exe           # 运行
```

- 查看main函数中的return

```
echo %ERRORLEVEL%
```

## Mac

Mac无自带编译器，推荐使用集成环境Xcode，其提供多种c++编译器。

## 兼容

向前兼容的英文为Forwards Compatibility，Forward有“将来”的含义。因此向前兼容就是指：以前的版本支持现在版本生成的数据，现在的版本支持以后的版本数据。就是为win3.1开发的程序能在win10下运行。

向后兼容（Backward Compatibility），又称作向下兼容（Downward Compatibility）。在计算机中指在一个程序或者类库更新到较新的版本后，用旧的版本程序创建的文档或系统仍能被正常操作或使用，或在旧版本的类库的基础上开发的程序仍能正常编译运行的情况。就是说为win10下开发的程序也能在win3.1下也能运行。

## 其他

- [通过这9本开源书，学好C++](http://blog.jobbole.com/110975/)
- [CppCon2017](https://github.com/CppCon/CppCon2017)
- [C++有哪些奇技淫巧？](https://www.zhihu.com/question/27338446)