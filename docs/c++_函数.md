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