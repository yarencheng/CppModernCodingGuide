# std::filesystem

```cpp
// C++17

#include <filesystem>

void fn()
{
    std::filesystem::path from = "/aaa/bbb/ccc";
    std::filesystem::path to = "/xxx/yyy/zzz";
    
    std::filesystem::copy(from, to);
}
```

##### Example, boost library

```cpp
// C++14

#include <boost/filesystem.hpp>

void fn()
{
    boost::filesystem::path from = "/aaa/bbb/ccc";
    boost::filesystem::path to = "/xxx/yyy/zzz";
    
    boost::filesystem::copy(from, to);
}
```

---

#### Reference

* [Wiki: C++17](https://en.wikipedia.org/wiki/C%2B%2B17)
* [cppreference.com: Filesystem library](http://en.cppreference.com/w/cpp/filesystem)
* [boost Filesystem Library](http://www.boost.org/doc/libs/1_66_0/libs/filesystem/doc/index.htm)



