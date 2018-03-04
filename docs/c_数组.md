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
// ubuntu 64位，c 64位
#include <stdio.h>
int s1(int *);
int s2(int []);
long s3(int *);

int main() {
    int a[] = {1, 2, 3};
    int *b = a;
    printf("%d\n", sizeof(a));      # 12
    printf("%d\n", sizeof(b));      # 8
    printf("%d\n", sizeof(int));    # 4
    printf("%d\n", sizeof(int *));  # 8
    printf("%d\n", s1(a));          # 8
    printf("%d\n", s2(a));          # 8
    printf("%d\n", s3(a));          # 8
    return 0;
}

int s1(int *a) {return sizeof(a);}
int s2(int a[]) {return sizeof(a);}
long s3(int *a) {return sizeof(a);}
```

```c
// windows 64位，c 32位
#include <stdio.h>
int s1(int *);
int s2(int []);
long s3(int *);

int main() {
    int a[] = {1, 2, 3};
    int *b = a;
    printf("%d\n", sizeof(a));      # 12
    printf("%d\n", sizeof(b));      # 4
    printf("%d\n", sizeof(int));    # 4
    printf("%d\n", sizeof(int *));  # 4
    printf("%d\n", s1(a));          # 4
    printf("%d\n", s2(a));          # 4
    printf("%d\n", s3(a));          # 4
    return 0;
}

int s1(int *a) {return sizeof(a);}
int s2(int a[]) {return sizeof(a);}
long s3(int *a) {return sizeof(a);}
```

- sizeof的值只与作用的对象类型有关，与输出的占位符类型无关