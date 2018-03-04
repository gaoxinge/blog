下面我们研究一下c++中的类。

## 类型查找

```cc
typedef double Money;
class Account {
    public:
        Money balance() {return bal;} // ok
    private:
        Money bal;
};


class Account {
    public:
        typedef double Money;
        Money balance() {return bal;} // ok
    private:
        Money bal;
};


class Account {
    public:
        Money balance() {return bal;} // error
    private:
        typedef double Money;
        Money bal;
};


typedef double Money;
class Account {
    public:
        typedef int Money;            // ok
        Money balance() {return bal;} // Money is int
    private:
        Money bal = 1.22;             // Money is int
};


typedef double Money;
class Account {
    public:
        Money balance() {return bal;} // Money is double
    private:
        typedef int Money;            // ok
        Money bal = 1.22;             // Money is int
};


typedef double Money;
class Account {
    public:
        Money balance() {return bal;} // Money is double
    private:
        Money bal = 1.22;             // Money is double
        typedef int Money;            // ok
};


class Account {
    public:
        typedef double Money;
        Money balance() {return bal;}
    private:
        typedef int Money;            // error
        Money bal;
};
```

## 构造器

```cc
#include <iostream>

int main() {
    int a(9);
	std::cout << a << std::endl;
	
	int b = 9;
	std::cout << b << std::endl;
	
	return 0;
}
```

```cc
#include <iostream>
#include <string>

class Sales_item {
    public:
        std::string isbn;
        unsigned units_sold;
        double revenue;
        Sales_item(const std::string &book):
        isbn(book), units_sold(0), revenue(0.0) {}
};

int main() {
    Sales_item sales_item("book");
    std::cout << sales_item.isbn << std::endl;
    return 0;
}
```

```cc
#include <iostream>
#include <string>

class Sales_item {
    public:
        std::string isbn;
        unsigned units_sold;
        double revenue;
        Sales_item(const std::string &book) {
            isbn = book;
            units_sold = 0;
            revenue = 0.0;
        }
};

int main() {
    Sales_item sales_item("book");
    std::cout << sales_item.isbn << std::endl;
    return 0;
}
```

## friend

- [What is the meaning of prepended double colon “::”?](https://stackoverflow.com/questions/4269034/what-is-the-meaning-of-prepended-double-colon)
- [C++ : what is :: for?](https://stackoverflow.com/questions/2282725/c-what-is-for)

```cc
class X {
    friend class Y;   // class: Y defined before X or in X
    friend void f();  // nonmember function: f defined before X or in X
	friend A& A::relocate(A::index, A::index, X&);  // member function: A defined before X, relocate defined in X
};

class Z {
    Y *ymem;
    void g() {return ::f();};
}
```