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

#### 使用 return value 比使用 output parameter 更好

使用 return value

* 可讀性高
* C++11 後編譯器會作最佳化，不會有因為 copy 或 move 造成的效率問題。

##### Example, bad

```cpp
void fn(vector<int>& v1, vector<int>& v2);// BAD, 哪一個是 output ?

void fn(int&); // BAD, overhead
```

##### Copy elision

```cpp
class T {};

T x = T(T(T())); // 只會呼叫一次 constructor
```

每個編譯器最佳化的方式可能會不一樣，例如下面的程式，用不同的編譯器會造成不同的結果

```cpp
struct C {
  C() {}
  C(const C&) { std::cout << "A copy was made.\n"; }
};

C f() {
  return C();
}

int main() {
  std::cout << "Hello World!\n";
  C obj = f();
}
```

可能的結果

```cpp
Hello World! 
A copy was made. 
A copy was made.
```

```
Hello World! 
A copy was made.
```

```
Hello World!
```

---

#### 回傳多個  output

##### Example, C++11 可以使用`tuple`

```cpp
std::tuple<double, char, std::string> callee()
{
    std::make_tuple(3.8, 'A', "Lisa Simpson");
}

void caller()
{
    auto o = callee();

    cout << std::get<0>(o) << endl;
    cout << std::get<1>(o) << endl;
    cout << std::get<2>(o) << endl;
}
```

##### Example, C++17 可以使用 Structured binding declarations

```cpp
struct A {
    int i;
    bool b;
};

struct A callee()
{
    struct A a = { 123, true };
    return a;
}

void caller()
{
    auto [ val1, val2 ] = callee();

    cout << val1 << endl;  // A.i
    cout << val2 << endl;  // A.b
}
```

---

#### 使用`T&&`來增進效率

如果某個參數給 function 使用後就不再被其他人使用，可以使用`T&&`和`std::move`來增進效率。

##### Example, bad

```cpp
class HeavyObject{ ... };

void consume(const HeavyObject& o)
{
    HeavyObject save_o(o);         // Bad, 呼叫 HeavyObject(const HeavyObject&) copy 成本很高
}

void fn()
{
    HeavyObject o;

    consume(o);

    // After consume(), o is never used.
}
```

##### Example, good

```cpp
class HeavyObject{ ... };

void consume(HeavyObject&& o)
{
    HeavyObject save_o(std::move(o));         // Good, 呼叫 HeavyObject(const HeavyObject&&) 比 copy 快
}

void fn()
{
    HeavyObject o;

    consume(std::move(o));

    // o 不可以再被使用

    // After consume(), o is never used.
}
```

注意：不是因為`std::move`讓`o`無法使用，是因為建立`save_o`的時候會呼叫`HeavyObject(const HeavyObject&&)`導致的。

---

#### Reference

* [C++ Core Guidelines: If your function may not throw, declare it`noexcept`](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Rf-noexcept)
* [C++ Core Guidelines: To return multiple "out" values, prefer returning a tuple or struct](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Rf-out-multi)
* [cppreference.com: Copy elision](http://en.cppreference.com/w/cpp/language/copy_elision)
* [stackoverFlow: What are copy elision and return value optimization?](https://stackoverflow.com/questions/12953127/what-are-copy-elision-and-return-value-optimization)



