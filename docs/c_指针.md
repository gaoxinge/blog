下面我们研究一下c中的指针。

## 操作和运算

- 赋值（assignment）
- 取值（dereferencing）：地址运算符&
- 取地址：间接运算符*
- 加整数（或者自增）
- 减整数（或者自减）
- 求差值（differencing）
- 比较
- sizeof

## 非法操作

```c
// 对未初始化的指针取值
int *pt;
*pt = 5;
```

```c
// 对数组进行指针操作
int urn[3];
urn++;
```

## 函数传递

```c
// 值传递
#include <stdio.h>
void swap(int, int);

int main()
{
  int x = 1;
  int y = 2;
  
  swap(x, y);
  printf("%d %d\n", x, y); // 1, 2
  
  return 0;
}

void swap(int a, int b)
{
  int temp;
  
  temp = a;
  a    = b;
  b    = temp;
  
  printf("%d %d\n", a, b); // 2, 1
}
```

```c
// 指针传递
#include <stdio.h>
void swap(int *, int *);

int main()
{
  int x = 1;
  int y = 2;
  
  swap(&x, &y);
  printf("%d %d\n", x, y);   // 1, 2
  
  return 0;
}

void swap(int *a, int *b)
{
  int temp;
  
  temp = a;
  a    = b;
  b    = temp;
  
  printf("%d %d\n", *a, *b); // 2, 1
}
```

```c
// 指针传递
#include <stdio.h>
void swap(int *, int *);

int main()
{
  int x = 1;
  int y = 2;
  
  swap(&x, &y);
  printf("%d %d\n", x, y);  // 2, 1
  
  return 0;
}

void swap(int *a, int *b)
{
  int temp;
  
  *temp = *a;
  *a    = *b;
  *b    = *temp;
  
  printf("%d %d\n", *a, *b); // 2, 1
}
```

```c
// 引用传递
#include <stdio.h>
void swap(int, int);

int main()
{
  int x = 1;
  int y = 2;

  swap(x,y);
  printf("%d %d\n", x, y);   // 2, 1

  return 0;
}

void swap(int &a,int &b)
{
  int temp;

  temp = a;
  a    = b;
  b    = temp;

  printf("%d %d\n", *a, *b); // 2, 1
}
```