# 不要：宣告所有變數在 function 開頭

##### Reason

* 在 legacy C 中，變數的宣告\(declare\)被要求一定要在其他的邏輯\(statement\)之前，但是在 C++ 這個限制已經不存在
* 變數宣告的位置如果距離使用到的地方越長，越容易出 Bug
* 有些`class`的建構子會做很多事情，影響效率

##### Example, bad

```cpp
void fn()
{
    int x = 123;
    
    //
    // ... some stuff ...
    //
    
    x++; // 在這裡才使用 x
}
```

##### Example, bad

```cpp
class HeavyIO
{
public:
    HeavyIO() {
        // do a lot of things
    }
};

void fn ()
{
    bool b = false;
    HeavyIO heavy; // 每次都會執行 constructor

    // 

    if (b) {
        heavy.doSomeThing()
    }
}
```

##### Example, good

```cpp
class HeavyIO
{
public:
    HeavyIO() {
        // do a lot of things
    }
};

void fn ()
{
    bool b = false;   

    // ... some stuff ...

    if (b) {
        HeavyIO heavy; // 要用的時候再執行 constructor
        heavy.doSomeThing()
    }
}
```



