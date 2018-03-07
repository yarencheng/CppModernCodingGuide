# Mutex & Lock

C++11 多了一些 mutex 的 wrapper 方便程式可以符合 RAII 的形式。

---

#### std::mutex

多使用`std::mutex`不要再繼續使用C的`pthread_mutex_t`。

##### Example, bad

```cpp
void fn()
{
    pthread_mutex_t mutex;

    pthread_mutex_init(&mutex, NULL);  // Bad, caller 初始化資源

    pthread_mutex_lock(&mutex);

    // ... critical section ...

    pthread_mutex_unlock(&mutex);

    pthread_mutex_destroy(&mutex);     // Bad, caller 清除資源
}
```

##### Example, good

```cpp
void fn()
{
    std::mutex mutex;            // RAII-styled mutex

    mutex.lock();

    // ... critical section ...

    mutex.unlock();
}
```

---

#### std::lock\_guard

mutex 的 wrapper，在不使用的時候會 unlock。

##### Example

```cpp
void bad()
{
    std::mutex mutex;

    mutex.lock();

    // ... critical section ...

    mutex.unlock();                // Bad, 可能不會呼叫
}

void good()
{
    std::mutex mutex;

    std::lock_guard<std::mutex> lock(mutex);

    // ... critical section ...

} // Good, 離開 scope 的時候 mutex 會被 unlock()
```

`std::lock_guard`並沒有提供`unlock()`，如果要的話可以使用`std::unique_lock`。

---

#### Reference

* [cppreference.com: std::lock\_guard](http://en.cppreference.com/w/cpp/thread/lock_guard)
* [cppreference.com: std::unique\_lock](http://en.cppreference.com/w/cpp/thread/unique_lock)



