# IO

---

#### 使用`std::fstream`代替`fopen()`和`fclose()`。

##### Example, bad

```cpp
void bad()
{
    FILE* input = fopen("name", "r");
    // ...
    if (something) return;           // Bad: leaked
    // ...
    fclose(input);
}
```

##### Example, good

```cpp
void good()
{
    ifstream input {"name"};
    // ...
    if (something) return;   // Good: no leak
    // ...
}
```

---

#### Reference

* [C++ Core Guidelines: Don't leak any resources](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#p8-dont-leak-any-resources)



