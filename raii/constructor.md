# Constructors, Assignments, and Destructors

TBD，C++ class 的 constructor 隨著版本演進有著不同的變化，以下會解釋每一種的不同。

這些 function 負責操作一個物件的生命週期，包含建立、複製、移動、摧毀。適當的設計這些 function 可以達到目地，例如容易使用、容易維護，效能提升...等。

---

#### Special Member Function 介紹

這些 function 有的時候又稱為 default function，但是這裡偏好使用 special member function，因為在某些情況下，編譯器不會自動產生這些 function。

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

---

#### 甚麼是 `=delete`和`=default`

```cpp
class A {

};

class B {
public:
    B() = delete;
};

void fn()
{
    A a;        // OK, implicit constructor A::A() is called
    B b;        // compile error, use of deleted function B::B()
}
```

```cpp
class A{
public:
    A(int) {}    
};

class B{
public:
    B(int) {}
    B() = default;
};

void fn()
{
    A a;        // compile error, an implicit A::A() is not defined since another constructor exists
    B b;        // OK
}
```

上面除了講解基本`=delete`和`=default`的功能外，也說明了編譯器會在某些特定條件下，自動產生一些東西，或者不自動產生一些東西，如果想知道更多細節請參考下面的 [Reference](#reference)。

---

#### Rule of Zero

如果可以的話，盡量不要宣告任何的 special member function，理由如下：

* 避免因為不當維護資源造成 leak
* 大部分情況編譯器，請相信 compiler 比你強

##### Example: 多餘的 default constructor

```cpp
class Bad {
public:
    int a;
    unique_ptr<int> b;
    Bad() : a(123), b(make_unique<int>(456)) {} // redundant
};

class Good {
public:
    int a = 123;
    unique_ptr<int> b = make_unique<int>(456);
}

void fn()
{
    Bad bad;
    cout << "bad  = " << bad.a << " " << *(bad.b) << endl;
    
    Good good;
    cout << "good = " << good.a << " " << *(bad.b) << endl;

}
```

---

#### Reference

* [cppreference.com: The rule of three/five/zero](http://en.cppreference.com/w/cpp/language/rule_of_three)
* [cppreference.com: Default constructors](http://en.cppreference.com/w/cpp/language/default_constructor)
* [cppreference.com: Move constructors](http://en.cppreference.com/w/cpp/language/move_constructor)
* [C++ Core Guidelines: Constructors, assignments, and destructors](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#S-ctor)
* [C++ Core Guidelines: If you can avoid defining default operations, do](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Rc-zero)



