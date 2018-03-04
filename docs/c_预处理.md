下面我们研究一下c中的预处理。

参考资料如下：

- [宏定义的黑魔法-宏菜鸟起飞手册](http://onevcat.com/2014/01/black-magic-in-macro)
- [造轮子手记](https://zhuanlan.zhihu.com/wheel-creatation)
- [Cloak](https://github.com/pfultz2/Cloak)

## 预处理

```c
#include <stdio.h>
#define _STDIO_H
// #define _STDIO_H 1
#define p
#define q 1

int main() {
    printf("%s\n", __DATE__);
    printf("%s\n", __TIME__);
    printf("%d\n", __FILE__);
    printf("%d\n", __LINE__);
    printf("%d\n", __STDC__);
    printf("%d\n", __STDC_HOSTED__);
    printf("%d\n", __STDC_VERSION__);
    // printf("%d\n", _STDIO_H);
    // printf("%d\n", p);
    printf("%d\n", q);
    return 0;
}
```

## 头文件

```c
// usehotel.c
#include <stdio.h>
#include "hotel.h"

int main()
{
  int a = 1, b = 2;
  printf("%d\n", f(a, b));
  return 0;
}
```

```c
// hotel.c
#include "hotel.h"

int f(int a, int b) {return a+b;}
```

```c
// hotel.h
int f(int, int);
```

在cmd中输入:

- gcc usehotel.c hotel.c
- a.exe