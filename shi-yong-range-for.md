# 使用 range for

優點：

* 不用檢查臨界值
* 
```cpp
std::array<int, 10> a;
memset( a.data() , 0 , 10 );    // BAD
std:fill( a , 0 );


```



