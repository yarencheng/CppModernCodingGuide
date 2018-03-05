[read this article on GitBook](https://yarencheng.gitbooks.io/c-modern-coding-guide/content/)

# \[\[\[\[\[ TBD \]\]\]\]\]

# C++ Modern Coding Guide

這裡整理了關於 C++ 應該使用的觀念、技巧、或事相關的 std 含式庫，主要目標是避免繼續使用 legacy C，以及提升 C++ 的維護性、易閱讀性、測試性...等。除非有特別說明否則這裡說提到的任何東西一定都可以在 C++17 使用。

#### [RAII](http://zh.cppreference.com/w/cpp/language/raii)

資源獲取即初始化（ Resource Acquisition Is Initialization ），或稱 RAII ，是一種 C++ 編程技術，它將必須在使用前請求的資源的生命周期綁定到一個對象的生存期。

#### [OOP](https://zh.wikipedia.org/wiki/面向对象程序设计)

C++ 是物件導向語言，在 class 的繼承結構裡，通常 base class 是拿來當作 interface，而 child class 通嘗是實做 interface 的 concrete class。

#### [Testable Code](https://en.wikipedia.org/wiki/Unit_testing)

為了要讓一個 class 或是一個 function 可以更容易被測試，在設計階段會把測試性納入設計考量裡，越容易被測試的程式也會帶來更多好處，例如降低耦合\(coupling\)。

#### Reference

* [C++ Core Guidelines](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md)
  ![](https://github.com/isocpp/CppCoreGuidelines/raw/master/cpp_core_guidelines_logo_text.png)
* [Effective C++](https://www.amazon.com/Effective-Specific-Improve-Programs-Designs/dp/0321334876/ref=sr_1_1?s=movies-tv&ie=UTF8&qid=1519358108&sr=8-1&keywords=effective+c%2B%2B)
  ![](https://images-na.ssl-images-amazon.com/images/I/51aICwPHi0L._SX396_BO1,204,203,200_.jpg)
* [Effective Modern C++](https://www.amazon.com/Effective-Modern-Specific-Ways-Improve/dp/1491903996/ref=sr_1_2?s=movies-tv&ie=UTF8&qid=1519358108&sr=8-2&keywords=effective+c%2B%2B)
  ![](https://images-na.ssl-images-amazon.com/images/I/51eFBpqPSLL._SX379_BO1,204,203,200_.jpg)



