## 标准

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
