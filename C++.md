# C++面试常考问题及答案解析

##### 1、为什么析构函数可以是虚函数而构造函数不可以是虚函数？

构造函数不可以是虚函数：

1、构造一个对象的时候，必须知道对象的实际类型，而虚函数的行为是在运行期间确定的。而在构造一个对象时，编译器还无法知道对象的实际类型是该类本身还是该类的一个派生类。

2、虚函数的执行依赖于虚函数表，而虚函数表是在构造函数中进行初始化，即虚函数指针vptr在构造函数中进行初始化，如果构造函数为虚函数，此时vptr还未初始化。

析构函数设为虚函数的作用：

```cpp
#include<bits/stdc++.h>
using namespace std;

class base
{
public:
    base(){cout << "base ctor\n";}
    virtual ~base(){cout << "base dtor\n";}
};

class derived : public base
{
public:
    derived(){cout << "derived ctor\n";}
    ~derived(){cout << "derived dtor\n";}
};

int main()
{
    base *p = new derived();
    delete p;
    return 0;
}
```

运行结果:

base ctor
		derived ctor
		derived dtor
		base dtor

如果base中的析构函数不是虚函数，那么仅会调用derived的析构函数，则有可能会造成内存泄露。

##### 2、类的虚拟继承？

