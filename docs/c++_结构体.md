下面我们研究一下c++中的struct。

## 第一种

```c++
// Book是一个类型
struct Book
{
	char title[50];
	char author[50];
	char subject[100];
	int book_id;
};
```

```c++
#include <iostream>
#include <cstring>
using namespace std;
void printBook(struct Book book); // 不可以省略struct

struct Book
{
   char  title[50];
   char  author[50];
   char  subject[100];
   int   book_id;
};

// void printBook(Book book); 
// 可以省略struct

void printBook(Book book) // 可以省略struct
{
   cout << "Book title: " << book.title << endl;
   cout << "Book author: " << book.author << endl;
   cout << "Book subject: " << book.subject << endl;
   cout << "Book id: " << book.book_id << endl;
}

int main()
{

	Book book; // 可以省略struct
	
	strcpy(book.title, "Learn C++ Programming");
	strcpy(book.author, "Chand Miyan");
	strcpy(book.subject, "C++ Programming");
	book.book_id = 1;
	
	printBook(book);
	
	return 0;
}
```

## 第二种

```c++
// Book是一个类型，book是一个变量
struct Book
{
	char title[50];
	char author[50];
	char subject[100];
	int book_id;
}book;
```

```c++
#include <iostream>
#include <cstring>
using namespace std;
void printBook(struct Book book); // 不可以省略struct

struct Book
{
   char  title[50];
   char  author[50];
   char  subject[100];
   int   book_id;
}book;

// void printBook(Book book); 
// 可以省略struct

void printBook(Book book) // 可以省略struct
{
   cout << "Book title: " << book.title << endl;
   cout << "Book author: " << book.author << endl;
   cout << "Book subject: " << book.subject << endl;
   cout << "Book id: " << book.book_id << endl;
}

int main()
{
	// book
	strcpy(book.title, "Learn C++ Programming");
	strcpy(book.author, "Chand Miyan");
	strcpy(book.subject, "C++ Programming");
	book.book_id = 1;
	
	printBook(book);
	
	// Book book
	// book is local and hides global book, which can be find C++ Primer on page 55
	Book book; // 可以省略struct
	
	strcpy(book.title, "Learn C++ Programming");
	strcpy(book.author, "Chand Miyan");
	strcpy(book.subject, "C++ Programming");
	book.book_id = 1;
	
	printBook(book);
	
	// Book booky
	Book booky; // 可以省略struct
	
	strcpy(booky.title, "Learn C++ Programming");
	strcpy(booky.author, "Chand Miyan");
	strcpy(booky.subject, "C++ Programming");
	booky.book_id = 1;
	
	printBook(booky);
	
	return 0;
}
```

## 第三种

```c++
// Book是一个变量
struct Book
{
	char title[50];
	char author[50];
	char subject[100];
	int book_id;
}Book;
```

```c++
#include <iostream>
#include <cstring>
using namespace std;

struct Book
{
	char title[50];
	char author[50];
	char subject[100];
	int book_id;
}Book;

int main()
{	
	strcpy(Book.title, "Learn C++ Programming");
	strcpy(Book.author, "Chand Miyan");
	strcpy(Book.subject, "C++ Programming");
	Book.book_id = 1;
	
	cout << "Book title: " << Book.title << endl;
    cout << "Book author: " << Book.author << endl;
    cout << "Book subject: " << Book.subject << endl;
    cout << "Book id: " << Book.book_id << endl;
	
	return 0;
}
```

## 第四种

```c++
// Book是一个类型
typedef struct
{
	char title[50];
	char author[50];
	char subject[100];
	int book_id;
}Book;
```

```c++
#include <iostream>
#include <cstring>
using namespace std;
// 这里函数申明会报错

typedef struct
{
	char title[50];
	char author[50];
	char subject[100];
	int book_id;
}Book;

void printBook(Book book); // 不能加struct

void printBook(Book book)
{
   cout << "Book title: " << book.title << endl;
   cout << "Book author: " << book.author << endl;
   cout << "Book subject: " << book.subject << endl;
   cout << "Book id: " << book.book_id << endl;
}

int main()
{
	Book book; // 不能加struct
	
	strcpy(book.title, "Learn C++ Programming");
	strcpy(book.author, "Chand Miyan");
	strcpy(book.subject, "C++ Programming");
	book.book_id = 1;
	
	printBook(book);
	
	return 0;
}
```

## 第五种

```c++
// Book是一个类型，Booky是一个类型
typedef struct Book
{
	char title[50];
	char author[50];
	char subject[100];
	int book_id;
}Booky;
```

```c++
#include <iostream>
#include <cstring>
using namespace std;
void printBook(struct Book book); // 不可以省略struct

struct Book
{
   char  title[50];
   char  author[50];
   char  subject[100];
   int   book_id;
}Booky;

// void printBook(Book book); 
// 可以省略struct

void printBook(Book book) // 可以省略struct
{
   cout << "Book title: " << book.title << endl;
   cout << "Book author: " << book.author << endl;
   cout << "Book subject: " << book.subject << endl;
   cout << "Book id: " << book.book_id << endl;
}

int main()
{

	Book book; // 可以省略struct
	
	strcpy(book.title, "Learn C++ Programming");
	strcpy(book.author, "Chand Miyan");
	strcpy(book.subject, "C++ Programming");
	book.book_id = 1;
	
	printBook(book);
	
	return 0;
}
```

```c++
#include <iostream>
#include <cstring>
using namespace std;
// 这里函数申明会报错

typedef struct Book
{
	char title[50];
	char author[50];
	char subject[100];
	int book_id;
}Booky;

void printBook(Booky book); // 不能加struct

void printBook(Booky book)
{
   cout << "Book title: " << book.title << endl;
   cout << "Book author: " << book.author << endl;
   cout << "Book subject: " << book.subject << endl;
   cout << "Book id: " << book.book_id << endl;
}

int main()
{
	Booky book; // 不能加struct
	
	strcpy(book.title, "Learn C++ Programming");
	strcpy(book.author, "Chand Miyan");
	strcpy(book.subject, "C++ Programming");
	book.book_id = 1;
	
	printBook(book);
	
	return 0;
}
```

## 第六种

```c++
// Book是一个类型
typedef struct Book
{
	char title[50];
	char author[50];
	char subject[100];
	int book_id;
}Book;
```
```c++
#include <iostream>
#include <cstring>
using namespace std;
void printBook(struct Book book); // 不可以省略struct

struct Book
{
   char  title[50];
   char  author[50];
   char  subject[100];
   int   book_id;
}Book;

// void printBook(Book book); 
// 可以省略struct

void printBook(Book book) // 可以省略struct
{
   cout << "Book title: " << book.title << endl;
   cout << "Book author: " << book.author << endl;
   cout << "Book subject: " << book.subject << endl;
   cout << "Book id: " << book.book_id << endl;
}

int main()
{

	Book book; // 可以省略struct
	
	strcpy(book.title, "Learn C++ Programming");
	strcpy(book.author, "Chand Miyan");
	strcpy(book.subject, "C++ Programming");
	book.book_id = 1;
	
	printBook(book);
	
	return 0;
}
```