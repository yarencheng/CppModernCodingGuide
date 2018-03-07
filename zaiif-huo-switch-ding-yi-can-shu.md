# 在 if 或 switch 定義參數

##### Example, if

```cpp
// C++14

A* a = get();
B* b = dynamic_cast<B*>(a);
if (b) {
    ...
}


// C++17

A* a = get();
if (B* b = dynamic_cast<B*>(a)) {
    ...
}

if (int i = get(); i == 123) {
    ...
}
```

##### Example, switch

```cpp
// C++14

int i = get();
switch(i){
    ...
}


// C++17

switch(int i = get()){
    ...
}
switch(int i = get(); i+123){
    ...
}
```

---

#### Reference

* [Wiki: C++17](https://en.wikipedia.org/wiki/C%2B%2B17)



