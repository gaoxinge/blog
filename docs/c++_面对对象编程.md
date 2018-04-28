﻿下面我们研究一下c++中的面向对象编程。

## Defining Base and Derived Classes

```cc
#include<iostream>
using namespace std;

class Shape {
    public:
        void draw() {
            cout << "draw Shape" << endl;
        }   
};

class Circle: public Shape {
    public:
        void draw() {
            cout << "draw Circle" << endl;
        }      
};

int main() {
    Shape* s = new Circle();
    s->draw(); // draw Shape
    return 0;
}
```

```
#include<iostream>
using namespace std;

class Shape {
    public:
        virtual void draw() {
            cout << "draw Shape" << endl;
        }   
};

class Circle: public Shape {
    public:
        virtual void draw() {
            cout << "draw Circle" << endl;
        }      
};

int main() {
    Shape* s = new Circle();
    s->draw(); // draw Circle
    s->Shape::draw(); // draw Shape
    return 0;
}
```