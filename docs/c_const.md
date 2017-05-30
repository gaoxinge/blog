下面我们研究一下c中的const。

## 基础

```c
#include <stdio.h>

int main()
{
  const int a = 1;
  const int b = a;
  // a = 2; compile error
  // b = 2; compile error
  printf("%d\n", a); # 1
  printf("%d\n", b); # 1
  return 0;
}
```

```c
#include <stdio.h>

int main()
{
  const int a = 1;
  int b = a;
  // a = 2; compile error
  b = 2;
  printf("%d\n", a); # 1
  printf("%d\n", b); # 2
  return 0;
}
```

```c
#include <stdio.h>

int main()
{
  int a = 1;
  const int b = a;
  a = 2;
  // b = 2; compile error
  printf("%d\n", a); # 2
  printf("%d\n", b); # 1
  return 0;
}
```

```c
#include <stdio.h>

int main()
{
  int a = 1;
  int b = a;
  a = 2;
  b = 2;
  printf("%d\n", a); # 2
  printf("%d\n", b); # 2
  return 0;
}
```

## 指针和数组

- 一般指针

```c
int *a; // 只能指向非常量数据
```

- 指向常量对象的指针

```c
const int *a; // 能指向常量和非常量数据，不能改变数据的值，但能改变自己的值
```

- 常量指针

```c
int *const a; // 只能指向非常量数据，能改变数据的值，但不能改变自己的值
```

- 指向常量对象的常量指针

```c
const int* const a;
```

- 数组

```c
const int a[] = {1, 2, 3};
int const a[] = {1, 2, 3};
const int const a[] = {1, 2, 3};
```

## 函数

可以使用const修饰形参。