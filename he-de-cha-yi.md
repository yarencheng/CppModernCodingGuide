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
```

##### Example, `{}`不允許 narrowing conversions

```cpp
int x {7.9};   // error: narrowing
int y = 7.9;   // OK: y becomes 7. Hope for a compiler warning
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



