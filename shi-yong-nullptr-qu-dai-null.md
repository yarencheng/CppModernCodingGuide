# 使用`nullptr`取代`NULL`

原因：`NULL`其實是 0 也就是`int`，但是`nullptr`是被定義用來當作指標。

```cpp
void fn(int);
void fn(void*);

fn(123);        // call fn(int)
fn(0);          // call fn(int)
fn(NULL);       // call fn(int)，有的 compiler 會 error
fn(nullptr);    // call fn(void*)
```

---

#### Reference

* Effective Modern C++: item8: Prefer nullptr to 0 and NULL
* [C++ Core Guidelines: Use`nullptr`rather than`0`or`NULL`](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Res-nullptr)



