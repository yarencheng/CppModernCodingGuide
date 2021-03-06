# RAII

* 於 1984-1989 在設計 C++ 的 exception-safe 和 resource-management 的時候被提出
* C++ 要求所有資源的 **獲取** 或 **釋放** 都必須在 **建構子** 或 **解構子** 裡面完成，例如：`fopen` / `fclose`、`lock`/ `unlock`、`new`/ `delete`。
* 大部份的`std`函式庫都已經遵照 RAII 實做，例如`std::string`、`std::vector`、`std::thread`。
* 滿足 RAII 的 class 必須將資源封裝到 class 管理，其中
  * 建構子，constructor，負責獲取資源，如果無法獲得資原則拋出 exception
  * 解構子，destructor，負責釋放資源，不疼拋出 exception

---

### 範例：用 std::lock\_guard

`std::lock_guard`是 C++11 為了滿足 RAII 所提供的一個 mutex wrapper，原始碼如下：

```cpp
template <class Mutex> class lock_guard {
private:
    Mutex& mutex_;

public:
    lock_guard(Mutex& mutex) : mutex_(mutex) { mutex_.lock(); }
    ~lock_guard() { mutex_.unlock(); }

    lock_guard(lock_guard const&) = delete;
    lock_guard& operator=(lock_guard const&) = delete;
};
```

當使用`std::lock_guard`的時候，就不用去煩惱甚麼時候要`unlock`的問題。

```cpp
extern void unsafe_code();  // 可能丟出例外

using std::mutex;
using std::lock_guard;

mutex g_mutex;

void access_critical_section()
{
    lock_guard<mutex> lock(g_mutex);
    unsafe_code();
}
```

##### Reference

* [Wiki: RAII](https://zh.wikipedia.org/wiki/RAII)
* Effective C++:  Chapter 3 Resource Management
* [C++ Core Guidelines: RAII](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Rr-raii)
* [cppreference: std::lock\_guard](http://en.cppreference.com/w/cpp/thread/lock_guard)



