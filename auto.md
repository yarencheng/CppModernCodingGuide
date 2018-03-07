# 使用`auto`宣告，縮短程式

##### Example

```cpp
map<int, string> data;

map<int, string>::iterator it = data.begin();  // OK
auto it = data.begin();                        // better

unsigned int i = data.size();  // OK
auto i = data.size();          // better, i 是 map<int, string>::size_type
```

但是有的時後會有問題，例如

```cpp
std::vector<bool> feature()
{
        return {true};
}

void () {

  bool b1 = feature()[0];
  cout << "b1 type name = " << typeid(b1).name() << endl;
  cout << "b1           = " << b1 << endl;

  auto b2 = feature()[0];
  cout << "b2 type name = " << typeid(b2).name() << endl;
  cout << "b2           = " << b2 << endl;  
}
```

執行結果

```cpp
b1 type name = b
b1           = 1
b2 type name = St14_Bit_reference  // std::vector<bool>::reference
b2           = 0
```

原因是因為`std::vector::operator[]`回傳的是`std::vector::reference`，它是一個proxy class。

Example, proxy class

```cpp
class Proxy{
public:
    operator bool() const { return true; };
    operator int() const { return 123; };
};

void fn()
{
    Proxy p = doSomeThing();

    bool b = doSomeThing();    // b = true
    int i = doSomeThing();     // i = 123
    auto a = doSomeThing()     // a 的 type 是 Proxy
}
```

---

#### Reference

* Effective Modern C++: Item 5: Prefer auto to explicit type declaration
* Effective Modern C++: Item 6: Use the explicitly typed initializer idiom when auto deduces undesired types
* [C++ Core Guidelines: Use auto to avoid redundant repetition of type names](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Res-auto)



