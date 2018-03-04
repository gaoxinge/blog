下面我们研究一下c++中的function。

参考资料如下：

- [值传递、指针传递、引用传递的区别](http://www.cnblogs.com/yjkai/archive/2011/04/17/2018647.html)

## 值传递

```c++
#include <iostream>
using namespace std;

void swap(int a, int b)
{
    int temp;
	 
    temp = a;
    a    = b;
    b    = temp;
	 
    cout << a << ", " << b << endl; // 2, 1
}

int main()
{
    int x = 1;
    int y = 2;
    
    swap(x, y);
    cout << x << ", " << y << endl; // 1, 2
    
    return 0;
}
```

## 指针传递

```c++
#include <iostream>
using namespace std;

void swap(int *a, int *b)
{
    int *temp;
	 
    temp = a;
    a    = b;
    b    = temp;
	 
    cout << *a << ", " << *b << endl; // 2, 1
}

int main()
{
    int x = 1;
    int y = 2;
    
    swap(&x, &y);
    cout << x << ", " << y << endl;  // 1, 2
    
    return 0;
}
```

```c++
#include <iostream>
using namespace std;

void swap(int *a, int *b)
{
    int temp;
	 
    temp = *a;
    *a   = *b;
    *b   = temp;
	 
    cout << *a << ", " << *b << endl; // 2, 1
}

int main()
{
    int x = 1;
    int y = 2;
    
    swap(&x, &y);
    cout << x << ", " << y << endl;  // 2, 1
    
    return 0;
}
```

## 引用传递

```c++
#include <iostream>
using namespace std;

void swap(int &a,int &b)
{
     int temp;
	 
     temp = a;
     a    = b;
     b    = temp;
	 
     cout << a << ", " << b << endl; // 2, 1
}

int main()
{
    int x = 1;
    int y = 2;
	
    swap(x,y);
    cout << x << ", " << y << endl; // 2, 1
	
    return 0;
}
```

## 常量引用传递

```c++
#include <iostream>
using namespace std;

int f(const int &a) {
    return a;
}

int main() {
   int a = 1;
   int &b = a;
   const int &c = a;
   cout << f(1) << endl;
   cout << f(1+2) << endl;
   cout << f(a) << endl;
   cout << f(b) << endl;
   cout << f(c) << endl;
   return 0;
}
```

## 引用返回

```c++
#include <iostream>
using namespace std;

char &get_val(string &str, string::size_type ix) {return str[ix];}

int main() {
    string s("a value");
	cout << s << endl;
	cout << get_val(s, 0) << endl;
	
	get_val(s, 0) = 'A';
	cout << s << endl;
	cout << get_val(s, 0) << endl;
	return 0;
}
```

## 重载

```c++
#include <iostream>
using namespace std;
int f(int);
double f(double, double);
int g(int);
int g(int); // redeclare

int main() {
    cout << f(123) << endl;
    cout << f(1.23, 2.45) << endl;
    cout << g(3) << endl;
    return 0;
}

int f(int x) {
    return x;
}

double f(double x, double y) {
    return y;
}

int g(int x) {
    return x;
}
```

## 函数指针

```c++
#include <iostream>
using namespace std;
int f(int*, int);
int g(int*, int);

int main() {
    typedef int (*PF)(int*, int);
    typedef int func(int*, int); // function type is not a pointer to functions
    
    PF p = f;
    // g = f; // error
    // func = f; // error
    return 0;
}

int f(int* x, int y) {
    return y;
}

int g(int* x, int y) {
    return y+1;
}
```