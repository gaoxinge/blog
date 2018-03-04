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