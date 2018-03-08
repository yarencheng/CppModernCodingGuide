# 使用 `std::array` 取代 `T[]`

```cpp
class A{
    int* bad1;
    int bad2[10];
    std::array<int, 10> good;
};

void bad(int* p, int size);
void bad(const std::array<int, 10>&);



```





