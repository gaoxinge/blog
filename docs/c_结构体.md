下面我们研究一下c中的结构体。

## 定义和声明

```c
#include <stdio.h>
#define MAXTITL 41

struct book {
    char title[MAXTITL];
};

int main() {
    struct book Book; // can't omit struct
    gets(Book.title);
    printf("%s", Book.title);
    return 0;
}
```

## typedef

```c
#include <stdio.h>
#define MAXTITL 41

struct book {
    char title[MAXTITL];
};

typedef struct book BOOK;

int main() {
    BOOK Book;
    gets(Book.title);
    printf("%s", Book.title);
    return 0;
}
```

## 函数指针

```c
#include <stdio.h>
int id(int);

int main() {
    printf("%d\n", id(1));    // result is 1
    printf("%d\n", (*id)(1)); // result is 1
    printf("%d\n", id);
    printf("%d\n", *id);
    printf("%d\n", &id);
    return 0;
}

int id(int x) {
    return x;
}
```