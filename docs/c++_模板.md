下面我们研究一下c++中的template。

参考资料如下：

- [C++模板(template)使用介绍](http://blog.csdn.net/chenchen410/article/details/39502199)
- [CppTemplateTutorial](https://github.com/wuye9036/CppTemplateTutorial)

## 函数模板

```c++
#include <iostream>
using namespace std;

template <typename T> T Min(T x, T y)
{return x<y ? x : y;}

int main()
{
   int n1 = 2;
   int n2 = 10;
   cout << Min(n1, n2) << endl;
   
   double d1 = 1.5;
   double d2 = 5.6;
   cout << Min(d1, d2) << endl;
   
   return 0;
}
```

## 结构体模板

```c++
#include <iostream>
using namespace std;

template <typename T1, typename T2> struct MyStruct
{
	private:
		T1 I;
		T2 J;
	
	public:
		MyStruct(T1 a, T2 b)
		{
			I = a;
			J = b;
		}
		
		void show()
		{cout << "I = " << I << ", J = "  << J << "." << endl;}
};

int main()
{
   MyStruct<int, int> struct1(3,5);
   struct1.show();
   
   MyStruct<int, char> struct2(3, 'a');
   struct2.show();
   
   MyStruct<double, int> struct3(2.9, 10);
   struct3.show();
   
   return 0;
}
```

## 类模板

```c++
#include <iostream>
using namespace std;

template <typename T1, typename T2> class MyClass
{
	private:
		T1 I;
		T2 J;
	
	public:
		MyClass(T1 a, T2 b)
		{
			I = a;
			J = b;
		}
		
		void show()
		{cout << "I = " << I << ", J = "  << J << "." << endl;}
};

int main()
{
   MyClass<int, int> class1(3,5);
   class1.show();
   
   MyClass<int, char> class2(3, 'a');
   class2.show();
   
   MyClass<double, int> class3(2.9, 10);
   class3.show();
   
   return 0;
}
```