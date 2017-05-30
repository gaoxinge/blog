下面我们研究一下c中的数组。

## 声明

```c
#define SIZE 4

int main()
{
  int a[4];
  int a[SIZE];
  int n = 4;
  int a[n];
  return 0;
}
```

## 初始化

```c
int main()
{
  int a;                       // 0
  int a1[4];                   // undefined
  int a2[4] = {};              // {0, 0, 0, 0}
  int a3[4] = {1, 2, 3, 4, 5}; // {1, 2, 3, 4}, but warning: excess elements in array initializer
  return 0;
}
```

```c
#define SIZE 4

int main()
{
  int a[] = {1, 2, 3, 4};
  int a[4] = {1, 2, 3, 4};
  int a[SIZE] = {1, 2, 3, 4};
  int n = 4;
  int a[n];
  for(int i=0; i<n; ++i)
    a[i] = i+1;
  return 0;
}
```

## 索引

```c
#include <stdio.h>
#define SIZE 4

int main()
{
  int value1 = 44;
  int arr[SIZE];
  int value2 = 88;
  
  printf("value1 = %d, value2 = %d\n", value1, value2);
  for(int i=-1; i<=SIZE; ++i)
    arr[i] = 2 * i + 1;
  for(int i=-1; i<7; ++i)
    printf("%2d %d\n", i, arr[i]);
  printf("value1 = %d, value2 = %d\n", value1, value2);
  
  return 0;
}
```

## 指针

```c
int main()
{
  /* a      == &a[0]
   * a+1    == &a[1]
   * *a     == a[0]
   * *(a+1) == a[1]
   */
  int a[] = {1, 2};
  return 0;
}
```

## 函数

```c
// 函数原型
int sum(int *);
int sum(int *, int);
int sum(int [], int);
int sum(int *, int *);

// 函数定义
int sum(int ar) {}
int sum(int *ar, int n) {}
int sum(int ar[], int n) {}
int sum(int *start, int *end) {}
```

```c
#include <stdio.h>
long s(int *);

int main()
{
  int a[] = {1, 2, 3};
  printf("%ld\n", sizeof(a));     # 12
  printf("%ld\n", s(a));          # 8
  printf("%ld\n", sizeof(int));   # 4
  printf("%ld\n", sizeof(int *)); # 8
  return 0;
}

long s(int *a) {return sizeof(a);}
```