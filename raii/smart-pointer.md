# Smart Pointer

智慧指標，C++11 提供`shared_ptr`、`unique_ptr`、`weak_ptr`等抽象資料類別，使用 template 來達成自動釋放指標所指向的記憶體或物件。

---

#### 使用 unique\_ptr 取代 T\*

```cpp
void bad()
{
    T* t = new T(); // bad

    // ...

    delete t; // bad, 因為可能不會被執行

    // ...
}

void good()
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
fn(unique_ptr<Foo>(new Foo()), bar());

// Exception-safe
fn(make_unique<Foo>(), bar());
```

---

#### `unique_ptr`v.s.`shared_ptr`的不同點

前者不允許 copy 後者可以

```cpp
unique_ptr<int> u1 = make_unique<int>(123);

unique_ptr<int> u2 = u1; // compiler error


shared_ptr<int> s1 = make_shared<int>(123);

shared_ptr<int> s2 = s1; // OK
```

`shared_ptr`會記綠use count，並且在沒有人使用的時候去釋放資源

```cpp
void fn()
{
    shared_ptr<int> s1 = make_shared<int>(123); // use count = 1

    if (...) {

        shared_ptr<int> s2 = s1 // use count = 2

        // ...

    } // use count = 1

    // ...

} // use  count = 0 ，delete int
```

---

#### 使用`weak_ptr`避免`shared_ptr`因為循環\(cycles\)導致的問題

```cpp
class A {
public:
    shared_ptr<B> _b;
}

class B {
public:
    shared_ptr<A> _a;
}

void bad()
{
    shared_ptr<A> a = make_shared<A>(); // A 的 use count = 1

    shared_ptr<B> b = make_shared<B>(); // B 的 use count = 1

    a._b = b; // B 的 reference count = 2

    b._a = a; // A 的 reference count = 2

} //      a 執行 deconstructor 的時候發現 A 的 use count = 1，不 delete A
  // 同理 b 執行 deconstructor 的時候發現 B 的 use count = 1，不 delete B
```

改用`weak_ptr`

```cpp
class A {
public:
    weak_ptr<B> _b;
}

class B {
public:
    weak_ptr<A> _a;
}

void good()
{
    shared_ptr<A> a = make_shared<A>(); // A 的 use count = 1

    shared_ptr<B> b = make_shared<B>(); // B 的 use count = 1

    a._b = b; // B 的 use count = 1

    b._a = a; // A 的 use count = 1

}  // safe
```

可以的話，應該先避免循環\(cycles\)，而不是直接使用`weak_ptr`。

---

#### 如果沒有要操作指標的擁有關係，使用`T*`或是`T&`來當作參數，而不是直接使用smart pointer當作參數

```cpp
void bad(shared_ptr<T> t)
{
    use(*t);
}

void bad_also(shared_ptr<T>& t)
{
    use(*t);
}


void good(T* t)
{
    use(*t);
}

void even_better(T& t)
{
    use(t)
}
```

除非是以下情況，否則不應該使用 smart pointer 當作參數

```cpp
// fn 要轉移指標擁有權的時候，fn 可能不會使用 t
void fn(unique_ptr<T> t)
{
    unique_ptr<T> another_t(std::move(t));

    ...
}

// 分享擁有關係，fn 可能不會使用 t
void fn(shared_ptr<T> t)
{
    shared_ptr<T> another_t = t;

    ...
}

// 重新指向另一個物件，fn 可能不會使用 t
void fn(unique_ptr<T>& t)
{
    t = make_unique<T>(...)

    ...
}

// 重新指向另一個物件，fn 可能不會使用 t
void fn(shared_ptr<T>& t)
{
    t = shared_ptr<T>(...)

    ...
}
```

---

#### 總結：使用時機

當要使用 smart pointer 的時候，應該要

1. 先考慮使用`unique_ptr`，
2. 如果指標同時會被多個物件分享使用，才可以使用`shared_ptr`，
3. 當使用`shared_ptr`產生循環\(cycles\)時，可以考慮使用`weak_ptr`。

---

#### Reference

* [Wiki: Smart Pointer](https://en.wikipedia.org/wiki/Smart_pointer)
* [C++ Core Guidelines: Use make\_unique\(\) to construct objects owned by unique\_ptr](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Rh-make_unique)
* [C++ Core Guidelines: Use`unique_ptr`or`shared_ptr`to avoid forgetting to`delete`objects created using`new`](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Rh-smart)
* [C++ Core Guidelines: Use`std::weak_ptr`to break cycles of`shared_ptr`s](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Rr-weak_ptr)
* [C++ Core Guidelines: For general use, take`T*`or`T&`arguments rather than smart pointers](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Rf-smart)
* [C++ Core Guidelines: Take a`unique_ptr<widget>&`parameter to express that a function reseats the`widget`](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Rr-reseat)
* [C++ Core Guidelines: Take a`shared_ptr<widget>&`parameter to express that a function might reseat the shared pointer](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#Rr-sharedptrparam)



