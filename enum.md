# Enum

---

#### 多使用`enum`而不是`#define`

```cpp
// webcolors.h (third party header)
#define RED   0xFF0000
#define GREEN 0x00FF00
#define BLUE  0x0000FF

// productinfo.h
// The following define product subtypes based on color
#define RED    0
#define PURPLE 1
#define BLUE   2

int webby = BLUE;   // webby == 2; probably not what was desired
```

---

#### 用`enum class`取代`enum`

##### Example, bad

```cpp
void Print_color(int color);

enum Web_color { red = 0xFF0000, green = 0x00FF00, blue = 0x0000FF };
enum Product_info { Red = 0, Purple = 1, Blue = 2 };

Web_color webby = Web_color::blue;

// Clearly at least one of these calls is buggy.
Print_color(webby);
Print_color(Product_info::Blue);
Print_color(0);
```

##### Example, good

```cpp
void Print_color(Web_color color);

enum class Web_color { red = 0xFF0000, green = 0x00FF00, blue = 0x0000FF };
enum class Product_info { red = 0, purple = 1, blue = 2 };

Print_color(Web_color::red);     // GOOD
Print_color(Product_info::Red);  // Error
Print_color(0);                  // Error
```

---

#### Reference

* Effective Modern C++: Prefer scoped enums to unscoped enums
* [C++ Core Guidelines: Prefer enumerations over macros](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Renum-macro)
* [C++ Core Guidelines: Prefer class enums over "plain" enums](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Renum-class)



