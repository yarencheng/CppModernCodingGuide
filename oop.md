# OOP

TBD

---

#### 使用 pure abstract class 當作 interface

甚麼是 abstract class：

* abstract class：至少有一個 virtual function 而且`=0`。
* pure abstract class：全部的 function 都是 virtual 而且`=0`。

```cpp
class A {    // abstract class
    virtual void fn1() = 0;
    void fn2();
};

class B {    // abstract class
    virtual void fn1() = 0;
    virtual void fn2() = 0;
};
```

##### Example, bad

```cpp
class Shape {  // bad: interface class loaded with data
public:
    Point center() const { return c; }
    virtual void draw() const;
    virtual void rotate(int);
    // ...
private:
    Point c;
    vector<Point> outline;
    Color col;
};

class Squar: public Shape { ... };
```

##### Example, good

```cpp
class Shape {    // better: Shape is a pure interface
public:
    virtual Point center() const = 0;   // pure virtual function
    virtual void draw() const = 0;
    virtual void rotate(int) = 0;
    // ...
    // ... no data members ...
};

class Squar: public Shape { ... };
```

---

#### Interface 必須有 virtual destructor

道理和[Base class 的 Destructor 必須是 public 且 virtual，或者是 protected 且 nonvirtual](/raii/constructor.md#base-class-destructor)一樣。

---

#### Interface 和 implementation 必須分開

假設現在有 3 個 class 它們之間的關係是

* Shape：形狀，基本的形狀類別
* Circle： 圓形，繼承自形狀
* Triangle：三角形，繼承自圓圈
* `Shape   <-   Circle   <-   Triangle`

混合 interface 和 implementation 這種繼承方式在早期 \(1980s ~ 1990s\) 很常見，它們發生的時間點順序是

1. 先有 Shape
2. 需要更多圓形的功能，部份功能和 Shape 相似，所以 Circle 繼承自 Shape
3. 需要更多三角形功能，部分功能和 Circle 相似，所以Triangle 繼承自 Circle
4. ... 繼續下去

這樣會導致很多種問題，最直接的問題\(和單元測試有關\)就是無法抽換，





這樣其實有很多缺點，例如



C.129: When designing a class hierarchy, distinguish between implementation inheritance and interface inheritance

---

#### Reference

* [C++ Core Guidelines: A class with a virtual function should have a virtual or protected destructor](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Rc-dtor-virtual)
* [C++ Core Guidelines: Prefer abstract classes as interfaces to class hierarchies](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Ri-abstract)
* [C++ Core Guidelines: When designing a class hierarchy, distinguish between implementation inheritance and interface inheritance](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Rh-kind)



