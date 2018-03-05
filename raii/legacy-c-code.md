# Legacy C code

傳統 C 的介面都不是 RAII，以下介紹如何在 C++ 裡面使用 legacy C 又可以達成 RAII 的方法

---

#### 包成 RAII-styled 的 wrapper

如果某一段 legacy C 很成被使用，可以包成一個 wrapper class，讓沒有 RAII 的部分越少越好。

##### Example

```cpp
void bad()
{
    legacy_c * resource = legacy_c_open();
    
    ...
    
    legacy_c_close(resource );
}

class RaiiWrapper{
public:
    legacy_c* resource ;
    RaiiWrapper() { resource = legacy_c_open(); }
    ~RaiiWrapper() { legacy_c_close(resource ); }
};

void good()
{
    RaiiWrapper resource;
    
    ...
}
```

---

#### 使用`BOOST_SCOPE_EXIT`

##### Example

```cpp
void bad()
{
    legacy_c * resource = legacy_c_open();
    
    ...
    
    legacy_c_close(resource);
}

void good()
{
    legacy_c * resource = legacy_c_open();
    
    BOOST_SCOPE_EXIT(&resource) {
        legacy_c_close(resource);
    } BOOST_SCOPE_EXIT_END
    
    ...
    
} // 離開 scope 的時候執行 BOOST_SCOPE_EXIT 裡面的邏輯
```

---

#### Reference

* [C++ Core Guidelines: Encapsulate messy constructs, rather than spreading through the code](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#p11-encapsulate-messy-constructs-rather-than-spreading-through-the-code)

* [Boost.ScopeExit](https://theboostcpplibraries.com/boost.scopeexit)



