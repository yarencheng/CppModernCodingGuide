# Constructors, Assignments, and Destructors

TBD，C++ class 的 constructor 隨著版本演進有著不同的變化，以下會解釋每一種的不同。

這些 function 負責操作一個物件的生命週期，包含建立、複製、移動、摧毀。適當的設計這些 function 可以達到目地，例如容易使用、容易維護，效能提升...等。

---

#### 基本分類

```cpp
class X {
public: 
    X() { ... }                         // a default constructor
}

void fn() {
    X x;                              
}

class X {
public: 
    X(const X& x) { ... }               // a copy constructor
}

void fn() {
    X x(anotherX);
}

class X {
public: 
    X& operator=(const X& x) { ... }    // a copy assignment
}

void fn() {
    X x = anotherX;
}

class X {
public: 
    X& operator=(const X& x) { ... }    // a move constructor
}

void fn() {
    X x = anotherX;
}

class X {
public: 
    X& operator=(X&& x) { ... }        // a move assignment
}

void fn() {
    X x = std::move(anotherX);
}

class X {
public: 
    ~X { ... }                         // a destructor
}
```

aa

---

#### Reference

* [cppreference.com: The rule of three/five/zero](http://en.cppreference.com/w/cpp/language/rule_of_three)
* [cppreference.com: Move constructors](http://en.cppreference.com/w/cpp/language/move_constructor)
* [C++ Core Guidelines: Constructors, assignments, and destructors](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#S-ctor)



