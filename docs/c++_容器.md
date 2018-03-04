下面我们研究一下c++中的容器。

## 默认构造

```c++
#include <iostream>
#include <vector>
using namespace std;

class Foo {
    
    private:
        int x;
    
    public:
        Foo(int);
        void setX(int);
        int getX();
};

Foo::Foo(int x) {this->x = x;}
void Foo::setX(int x) {this->x = x;}
int Foo::getX() {return this->x;}

int main() {
    /*
     * error: no constructor Foo::Foo()
     *
     * Foo f;
     * f.setX(1);
     * cout << f.getX() << endl;
     */
    
    vector<Foo> v1;
    // error: no constructor Foo::Foo()
    // vector<Foo> v2(5);
    vector<Foo> v3(5, 1);
    return 0;
}
```

## 二维向量

```c++
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<vector<int>> v(5, vector<int>(5, 0));
    
    cin >> v[0][0];
    cin >> v[1][1];
    cin >> v[2][2];
    cin >> v[3][3];
    cin >> v[4][4];
    
    for (int m = 0; m < v.size(); ++m) {
        for (int n = 0; n < v[m].size(); ++n)
            cout << v[m][n] << " ";
        cout << "\n";
    }
    
    return 0;
}
```

## 算法

```cc
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>
using namespace std;

int main() {
    string ws[] = {"the", "quick", "red", "fox", "jumps", "over", "the", "slow", "red", "turtle"};
    vector<string> words(ws, ws+9);
    sort(words.begin(), words.end());
    vector<string>::iterator end_unique = unique(words.begin(), words.end());
    words.erase(end_unique, words.end());
    for (vector<string>::iterator iter = words.begin(); iter != words.end(); ++iter)
        cout << *iter << endl;;
    return 0;
}
```