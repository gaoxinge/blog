
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