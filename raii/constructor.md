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
    X& operator=(const X&& x) { ... }    // a move constructor
}

void fn() {
    X x(std::move(anotherX));
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
    ~X() { ... }                         // a destructor
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
    A a;        // compile error, an implicit A::A() is not 
                // defined since another constructor exists
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
```

使用 in-class member initializers 並且讓 compiler 自動產生 default constructor。

---

#### Rule of three

destructor、copy constructor、copy assignment 任一個被使用者定義出來的時候，這三個必須同時都被使用者定義。

##### Reason

避免使用 compiler 自動產生的 implicit 程式，導致沒有正常操作資源。

##### Example, bad

```cpp
class Bad {
public:
    int* i;
    Bad() {
        i= new int(123);
    }
    ~Bad() {
        delete i;
    }
}

void fn()
{
    Bad b1;    // 執行 user-defined default constructor Bad::Bad()
               // 並且 allocate i

    Bad b2(b1) // 執行 implicit copy constructor Bad::Bad(const Bad&)
               // 指標 b1.i 和 b2.i 指到一樣的位置

} // 離開 fn()，執行 user-defined destructor 2 次，造成 double free
```

##### Example, good

```cpp
class Good {
public:
    int* i;
    Good () {
        i= new int(123);
    }
    ~Good () {
        delete i;
    }
    Good (const Good& other) {
        i= new int(*(other.i));
    }
    Good& operator=(const Good& other) {
        i= new int(*(other.i));
    }
}

class GoodAlso {
public:
    int* i;
    GoodAlso () {
        i= new int(123);
    }
    ~GoodAlso () {
        delete i;
    }
    GoodAlso (const Good& other) = delete;
    GoodAlso & operator=(const Good& other) = delete;
}
```

---

#### Rule of Five

如果使用者有自行定義的 move constructor 或是 move assignment，那麼前面兩個和 destructor、copy-constructor、copy-assignment 共五個必須同時被使用者自行定義。

##### Reason

避免使用 compiler 自動產生的 implicit 程式，導致沒有正常操作資源。

---

#### Base class 的 Destructor 必須是 public 且 virtual，或者是 protected 且 nonvirtual {#base-class-destructor}

##### Example, bad

```cpp
class Base {
public:
    ~Base () { // free something ... }
};

class Derived: public Base {
public:
    ~Derived () { // free something ... }
};

void fn()
{
    unique_ptr<Base> base = make_unique<Derived>();
} // 呼叫 ~Base() 但是沒有 呼叫 ~Derived ()
```

##### Example, good

```cpp
class Base {
public:
    virtual ~Base () { // free something ... }    // use virtual
};

class Base final {                                // use final
public:
    ~Base () { // free something ... }
};
```

TODO: example of protected and nonvirtual

---

#### Destructor 不可以拋出例外

##### Example, bad

```cpp
class A{
    ~A () {
        ...
        if (...) {
            throw exception();
        }
        ...
    }
}
```

##### Example, good

destructor 不能丟 exception 而且要標記 noexcept

```cpp
class A{
    ~A () noexcept {
        ...
        if (...) {
            std::terminate();
        }
        ...
    }
}
```

除了 destructors 以外，swap functions、move operations、還有 default constructors 也不應該拋出例外.

---

#### Base Class 應該禁止 "被複製"，如果非要不可的話，提供 virtual copy。

如果沒有禁止，child class 的資料可能會消失\(slicing\)。

##### Example, bad

```cpp
class A {
public:
    int data;
};

class B: public A {
public:
    int more_data;
}

void fn()
{
    auto b = make_unique<B>();

    // 呼叫 A::A(const A&) 導致 b.more_data 遺失
    auto a = make_unique<A>(b);
}
```

##### Example, good

```cpp
class A {
public:
    int data;
    A(const A&) = delete;
    A& operator(const A&) = delete;
    virtual unique_ptr<A> clone() { return /* A object */; }
};

class B: public A {
public:
    int more_data;
    unique_ptr<A> clone() override { return /* B object */; }
}

void fn()
{
    auto b = make_unique<B>();
    auto a = b->clone(); // ok, deep clone
}
```

---

#### Reference

* [cppreference.com: The rule of three/five/zero](http://en.cppreference.com/w/cpp/language/rule_of_three)
* [cppreference.com: Default constructors](http://en.cppreference.com/w/cpp/language/default_constructor)
* [cppreference.com: Move constructors](http://en.cppreference.com/w/cpp/language/move_constructor)
* [C++ Core Guidelines: Constructors, assignments, and destructors](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#S-ctor)
* [C++ Core Guidelines: If you can avoid defining default operations, do](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Rc-zero)
* [C++ Core Guidelines: Don't define a default constructor that only initializes data members; use in-class member initializers instead](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#c45-dont-define-a-default-constructor-that-only-initializes-data-members-use-in-class-member-initializers-instead)
* [C++ Core Guidelines: A base class destructor should be either public and virtual, or protected and nonvirtual](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#c35-a-base-class-destructor-should-be-either-public-and-virtual-or-protected-and-nonvirtual)
* [C++ Core Guidelines: A base class should suppress copying, and provide a virtual`clone`instead if "copying" is desired](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#c67-a-base-class-should-suppress-copying-and-provide-a-virtual-clone-instead-if-copying-is-desired)
* Effective Modern C++: Item 17: Understand special member function generation



