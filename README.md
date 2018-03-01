[read this article on GitBook](https://yarencheng.gitbooks.io/c-modern-coding-guide/content/)

# \[\[\[\[\[ TBD \]\]\]\]\]

# C++ Modern Coding Guide

這裡整理了關於 C++ 應該使用的觀念、技巧、或事相關的 std 含式庫，主要目標是避免繼續使用 legacy C，以及提升 C++ 的維護性、易閱讀性、測試性...等。

#### [RAII](http://zh.cppreference.com/w/cpp/language/raii)

資源獲取即初始化（ Resource Acquisition Is Initialization ），或稱 RAII ，是一種 C++ 編程技術，它將必須在使用前請求的資源的生命周期綁定到一個對象的生存期。

#### [OOP](https://zh.wikipedia.org/wiki/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1)

C++ 是物件導向語言，在 class 的繼承結構裡，通常 base class 是拿來當作 interface，而 child class 通嘗是實做 interface 的 concrete class。

#### [Testable Code](https://en.wikipedia.org/wiki/Unit_testing)

為了要讓一個 class 或是一個 function 可以更容易被測試，在設計階段會把測試性納入設計考量裡，越容易被測試的程式也會帶來更多好處，例如降低耦合\(coupling\)。





