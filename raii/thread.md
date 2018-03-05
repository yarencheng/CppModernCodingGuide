# Thread

以下會介紹如何使用 C++ 的 library，透過 RAII 的方式去操作 thread。

---

#### 使用`std::thread`取代`pthread`

TODO: example

---

#### 如果要 join thread，考慮使用`gsl::joining_thread`

##### Example

```cpp
void f() { std::cout << "Hello "; }

void bad()
{
    std::thread thread{f};

    thread.join()            // Bad, 可能沒有join
}

void good()
{
    gsl::joining_thread thread{f};
    
} // Good, 自動 join
```

---

#### Reference

* [cppreference.com: std::thread](http://en.cppreference.com/w/cpp/thread/thread)
* [C++ Core Guidelines: Prefer`gsl::joining_thread`over`std::thread`](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Rconc-joining_thread)



