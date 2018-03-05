# Thread

以下會介紹如何使用 C++ 的 library，透過 RAII 的方式去操作 thread。

---

#### 使用`std::thread`取代`pthread`

TODO: example

---

#### 如果要 join thread，考慮使用`gsl::joining_thread`

##### Example, bad

```cpp
static void * thread_start(void *arg) { ... }

void bad()
{
    pthread_t thread_id;
    
    pthread_create(&thread_id, NULL, &thread_start, NULL);
    
    pthread_join(thread_id, NULL);
    
    
}
```

s

---

#### Reference

* [cppreference.com: std::thread](http://en.cppreference.com/w/cpp/thread/thread)



