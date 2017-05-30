下面我们研究一下c中的多维数组。

## 指针

```c
int *ip[4]; // 指向int指针的数组
int (*ip)[4]; // 指向int数组的指针
```

## 函数

```c
// 函数原型
void f(int (*)[4]);
void f(int [][4]);

// 函数定义
void f(int (*a)[4]) {}
void f(int a[][4]) {}
```