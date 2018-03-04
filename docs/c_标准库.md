下面我们研究一下c中的标准库。

## stdio

- scanf, printf, sprintf
- getchar, putchar
- gets, puts, fgets, fputs
- fopen, fclose, getc, putc
- fscanf, fprintf, fgets, fputs
- fseek, ftell, fgetpos, fsetpos
- ungetc, fflush, setvbuf, fread, fwrite, feof, ferror
- EOF

## stdlib

- atoi, atof, atol
- strtol, strtoul, strtod
- rand, srand
- malloc, calloc, free
- exit
- EXIT_FAILURE, EXIT_SUCCESS

## time

- time

## math

- [math.h](http://baike.baidu.com/link?url=OCi3X5OBlmQk_k8o8fPIMnlXsnWMejBrW8DNtW3i4HV1MP2sTq6fT4aB2mhuld84ZJem2zEvYuS1FHLL4H4TCK)
- [math.h](http://c.biancheng.net/cpp/u/math_h/)
- [math.h](http://www.yiibai.com/c_standard_library/math_h.html)

## ctype

- isalnum, isalpha, isblank, iscntrl, isdigit, isgraph, islower, isprint, ispunct, isspace, isupper, isxdigit
- tolower, toupper

## string

- strlen, strchr, strbprk, strrchr, strstr
- strcat, strncat
- strcmp, strncmp
- strcpy, strncpy

## stdarg

```c
#include <stdio.h>
#include <stdarg.h>
double sum(int, ...);

int main() {
    printf("%f", sum(3, 1.0, 2.0, 3.0));
    return 0;
}

double sum(int n, ...) {
    double s = 0.0;
    va_list ap;
    va_start(ap, n);
    for (int i = 0; i < n; ++i) 
        s += va_arg(ap, double);
    va_end(ap);
    return s;
}
```