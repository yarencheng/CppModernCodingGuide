# Named Cast

在 C++ 中避免使用 cast，如果要，應該使用 named cast。

```cpp
class B { /* ... */ };
class D { /* ... */ };

template<typename D> D* upcast(B* pb)
{
    D* pd0 = pb;                        // error: no implicit conversion from B* to D*
    D* pd1 = (D*)pb;                    // legal, but what is done?
    D* pd2 = static_cast<D*>(pb);       // error: D is not derived from B
    D* pd4 = dynamic_cast<D*>(pb);      // OK: return nullptr
}
```

---

#### `dynamic_cast`的用法

##### Example

```cpp
struct B {   // an interface
    virtual void f();
    virtual void g();
};

struct D : B {   // a wider interface
    void f() override;
    virtual void h();
};

void bad(B* pb)
{
    D* pd = dynamic_cast<D*>(pb);        // BAD, 可能是 nullptr
    // ... use D's interface ...
}

void good(B* pb)
{
    if (D* pd = dynamic_cast<D*>(pb)) {  // OK, 檢查是否轉型成功
        // ... use D's interface ...
    }
    else {
        // ... make do with B's interface ...
    }
}
```

---

#### Reference

* C++ Core Guidelines: [cppreference.com: If you must use a cast, use a named cast](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Res-casts-named)
* [C++ Core Guidelines: Use dynamic\_cast where class hierarchy navigation is unavoidable](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#c146-use-dynamic_cast-where-class-hierarchy-navigation-is-unavoidable)



