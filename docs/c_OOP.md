下面我们研究一下c中的OOP。

参考资料如下：

- [有趣的Coding实验](https://zhuanlan.zhihu.com/enjoyable-lab)
- [C语言面向对象编程](http://blog.csdn.net/foruok/article/details/18192167)

## 面向对象编程

```c
#include <stdio.h>
#include <string.h>

/*
 * C语言中：
 * 1. 不使用指针，就是按值传递；
 * 2. 没有按引用传递，&是C++中的。 
 */

struct Father {
    char name[20];
};

void fatherConstructer(struct Father this, const char* name) {
    strcpy(this.name, name);
}

void sing(struct Father this) {
    printf("singing~~~\n");
}

struct Mother {
    char age[20];
};

void motherConstructer(struct Mother this, const char* age) {
    strcpy(this.age, age);
}

void dance(struct Mother this) {
    printf("dancing~~~\n");
}

struct Child {
    struct Father father;
    struct Mother mother;
};

void childConstructer(struct Child this, const char* name, const char* age) {
    fatherConstructer(this.father, name);
    motherConstructer(this.mother, age);
}

void cry(struct Child this) {
    printf("mmm~~~\n");
}

int main () {
    struct Father father;
    struct Mother mother;
    struct Child child;
    
    fatherConstructer(father, "a");
    motherConstructer(mother, "1");
    childConstructer(child, "b", "2");
    
    printf("%s", father.name);
    sing(father);
    
    printf("%s", mother.age);
    dance(mother);
    
    printf("%s", child.father.name);
    printf("%s", child.mother.age);
    sing(child.father);
    dance(child.mother);
    cry(child);
    
    return 0;
}
```

```c
#include <stdio.h>
#include <string.h>

/*
 * C语言中：
 * 1. 从子类到父类要使用&this->father或者&child.father；
 * 2. 从子类到第一个父类可以使用((struct Father*) this)或者((struct Father*) &child)；
 * 3. 从子类到第二个父类可以使用offsetof；
 * 4. 从父类到子类要使用offsetof；
 * 5. offset函数在stddef.h头文件中。
 */

struct Father {
    char name[20];
};

void fatherConstructer(struct Father* this, const char* name) {
    strcpy(this->name, name);
}

void sing(struct Father* this) {
    printf("singing~~~\n");
}

struct Mother {
    char age[20];
};

void motherConstructer(struct Mother* this, const char* age) {
    strcpy(this->age, age);
}

void dance(struct Mother* this) {
    printf("dancing~~~\n");
}

struct Child {
    struct Father father;
    struct Mother mother;
};

void childConstructer(struct Child* this, const char* name, const char* age) {
    fatherConstructer(&this->father, name);
    motherConstructer(&this->mother, age);
}

void cry(struct Child* this) {
    printf("mmm~~~\n");
}

int main() {
    struct Father father;
    struct Mother mother;
    struct Child child;
    
    fatherConstructer(&father, "a");
    motherConstructer(&mother, "2");
    childConstructer(&child, "b", "1");
    
    sing(&father);
    printf("%s\n", father.name);
    
    dance(&mother);
    printf("%s\n", mother.age);
    
    sing(&child.father);
    printf("%s\n", child.father.name);
    dance(&child.mother);
    printf("%s\n", child.mother.age);
    cry(&child);
    
    return 0;
}
```

## 泛型

- 宏
- void*