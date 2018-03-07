# 使用 using 取代 typedef

優點：

* 更容易閱讀
* 支援 template

```cpp
typedef int (*PFI)(int);     // OK, but convoluted

using PFI2 = int (*)(int);   // OK, preferred

template<typename T>
typedef int (*PFT)(T);       // error

template<typename T>
using PFT2 = int (*)(T);     // OK
```

---

#### Reference

* Effective Modern C++: Item 9: Prefer using declarations to typdefs
* [C++ Core Guidelines: Prefer`using`over`typedef`for defining aliases](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#t43-prefer-using-over-typedef-for-defining-aliases)



