# Function

這例會介紹如何設計一個 function 的介面。

---

#### 如果不會丟出`excception`，宣告成`noexcept`

如果一個 function 宣告成`noexcept`，編譯器會針對它作最佳化，所以如果確定一個 function 不會丟出`exception`，或是它是一個 C style 的 function 請宣告成`noexcept`。

```cpp
void not_throw() noexcept
{
    ...
}
```

---

#### Input 參數應該用`T`還是用`T&`

如果一個物件小於 4 ~ 6 個 byte 請使用`T`，否則使用`T&`。

##### Example

```cpp
void fn(int i);           // OK

void fn(const int& i);    // BAD, overhead

void fn(string s);        // BAD, expensive

void fn(const string& s); // OK
```

---

#### Reference

* [C++ Core Guidelines: If your function may not throw, declare it`noexcept`](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Rf-noexcept)



