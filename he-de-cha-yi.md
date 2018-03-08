# 使用 {} 初始化物件

優點：

* simpler
* more general
* less ambiguous
* 比其他初始化方式更安全

##### Example

```cpp
int i{123};
vector<int> v = {1, 2, 3};


class A{
public:
    int i;
    string s;
};

A a{123, "haha"};


class B{
public:
    int i{123};
    string s{"haha"};
};
```

##### Example, T\(\) 到底是變數還是 function

```cpp
int a();        // function
int b(123);     // variable
int c{};        // variable
int d{123};     // variable

cout << "a = " << a << " " << typeid(a).name() << endl;
cout << "b = " << b << " " << typeid(b).name() << endl;
cout << "c = " << c << " " << typeid(c).name() << endl;
cout << "d = " << d << " " << typeid(d).name() << endl;


執行結果
a = 1   FivE
b = 123 i
c = 0   i
d = 123 i

=================

class T { ... };

T a();        // variable or function ?
T b(123);     // variable
T c{};        // variable
T d{123};     // variable
```

##### Example, `{}`不允許 narrowing conversions

```cpp
int x {7.9};   // compile error: narrowing
int y = 7.9;   // BAD: y becomes 7. Hope for a compiler warning

double x, y, z;
int sum{ x + y + z };   // compile error: narrowing
int sum = x + y + z;    // BAD
```

##### Example, 搭配`auto`使用

```cpp
auto x1 {7};        // x1 is an int with the value 7
auto x2 = {7};      // x2 is an initializer_list<int> with an element 7

auto x11 {7, 8};    // error: two initializers
auto x22 = {7, 8};  // x2 is an initializer_list<int> with elements 7 and 8
```

---

#### Reference

* [C++ Core Guidelines: Prefer the {} initializer syntax](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#es23-prefer-the--initializer-syntax)
* Effective Modern C++: Item 7: Distinguish between \(\) and {} when creating objects



