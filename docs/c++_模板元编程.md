下面我们研究一下c++中的template。

参考资料如下：

- [C++模板(template)使用介绍](http://blog.csdn.net/chenchen410/article/details/39502199)
- [CppTemplateTutorial](https://github.com/wuye9036/CppTemplateTutorial)
- [Modern C++ Design](https://book.douban.com/subject/1755195)

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
   cout << Min<int>(n1, n2) << endl;
   
   double d1 = 1.5;
   double d2 = 5.6;
   cout << Min<double>(d1, d2) << endl;
   
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

## 特化和偏特化

```c++
#include <iostream>
using namespace std;

template <typename T> class TypeToID
{
	public:
		static int const ID = -1;
};

template <> class TypeToID<int>
{
	public:
		static int const ID = 0;
};

template <> class TypeToID<float>
{
	public:
		static int const ID = 0xF10A7;
};

template <typename T> class TypeToID<T*>
{
	public:
		static int const ID = 0x80000000;
};

template <> class TypeToID<int*>
{
	public:
		static int const ID = 0x12345678;
};

int main()
{
	cout << "ID of int: "    << TypeToID<int>::ID    << endl;
	cout << "ID of float: "  << TypeToID<float>::ID  << endl;
	cout << "ID of flaot*: " << TypeToID<float*>::ID << endl;
	cout << "ID of int*: "   << TypeToID<int*>::ID   << endl;
	cout << "ID of char: "   << TypeToID<char>::ID   << endl;
	return 0;
}
```

## 默认参数

```c++
template <typename T0, typename T1 = void> struct DoWork;

template <typename T> struct DoWork<T> {};
template <>           struct DoWork<int> {};
template <>           struct DoWork<float> {};
template <>           struct DoWork<int, int> {};

DoWork<int> i;
DoWork<float> f;
DoWork<double> d;
DoWork<int, int> ii;
```

## 不定长

```c++
template <typename... Ts>             class X {};           // ok 
template <typename... Ts, typename U> class X {};           // error
template <typename... Ts, typename U> class X<U, Ts...> {}; // ok
template <typename... Ts, typename U> class X<Ts..., U> {}; // error
```