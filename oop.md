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

道理和[Base class 的 Destructor 必須是 public 且 virtual，或者是 protected 且 nonvirtual](/raii/constructor.md/base-class-destructor)一樣。

C.129: When designing a class hierarchy, distinguish between implementation inheritance and interface inheritance

---

#### Reference

* [C++ Core Guidelines: A class with a virtual function should have a virtual or protected destructor](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Rc-dtor-virtual)

* [C++ Core Guidelines: Prefer abstract classes as interfaces to class hierarchies](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Ri-abstract)



