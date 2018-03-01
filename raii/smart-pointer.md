# Smart Pointer

智慧指標，C++11 提供`shared_ptr`、`unique_ptr`、`week_ptr`等抽象資料類別，使用 template 來達成自動釋放指標所指向的記憶體或物件。

---

#### 使用 unique\_ptr 取代 T\*

```cpp
void bad_example()
{
    T* t = new T(); // bad
    
    // ...
    
    delete t; // bad
    
    // ...
}

void good_example()
{
    unique_ptr<T> t = unique_ptr<T>(new T()); // good
    
    // ...
}// 在離開 scope 的時候，unique_ptr 會執行 delete t 的動作
```

---

#### 使用 make\_unique 來建立 unique\_ptr

使用`make_unique`可以讓程式碼更簡潔

```cpp
unique_ptr<Foo> p {new<Foo>{7}};  // 可以，但是不建議

auto q = make_unique<Foo>(7);     // 比較好
```

如果在過程中有丟出`exception`，使用`make_unique`可以避免 leak，

```cpp
// 不是 exception-safe，因為 compiler 所執行的順序可能是
//
// 1. 獲取 Foo 的記憶體
// 2. Foo 執行 constructor
// 3. 呼叫 bar()
// 4. unique_ptr 執行 constructor
//
// 如果 bar() 丟出 exception，Foo 的 destructor 將不會被執行，導致leak
f(unique_ptr<Foo>(new Foo()), bar());

// Exception-safe
f(make_unique<Foo>(), bar());
```

---

####  `shared_ptr`、`unique_ptr`、`week_ptr`之間的關係



---

#### Reference

* [Wiki: Smart Pointer](https://en.wikipedia.org/wiki/Smart_pointer)
* [C++ Core Guidelines: Use make\_unique\(\) to construct objects owned by unique\_ptr](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Rh-make_unique)
* [C++ Core Guidelines: Use`unique_ptr`or`shared_ptr`to avoid forgetting to`delete`objects created using`new`](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Rh-smart)



