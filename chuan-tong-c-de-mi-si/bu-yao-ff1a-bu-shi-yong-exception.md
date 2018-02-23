# 不要：不使用 exception

##### Reason

exception 最常拿出來和`return error`做比較的分別是下面 2 點

* exception 沒有效率
  * 跟甚麼比較？
  * 考慮程式架構的話，exception 更有彈性、更容易寫出更簡短更乾淨的程式
  * 執行速度 exception 其實是比較快的\(參考下面的 zero cost\)，當然這個前提是要有能力寫出 "有效率的處理 exception 邏輯"，很多時候效率不好其實是 programer 自己的問題
* exception 很容易沒有處理，導致 core dump
  * `return error`如果沒有處理，也會出現一樣的問題，不是只有 exception
  * 跟`return error`最大的不同點是 exception 沒有處理的話會一直往上層的 caller 丟過去，可以避免 silent error 讓程式更早可以發現潛在問題\(Fail-fast\)

總結以上 2 點，其實 exception 並沒有很明顯的缺點，最明顯的缺點是：

* 當使用太多的 legacy C 函式庫，程式碼將會夾雜著 exception 和`return code`兩種不同的 error 處理方式。

exception 並不是也不等價return error，要記住：

* exception 是無法被 library 處理才丟出來的
* 不是所有的 exception 都要 catch，這是錯誤用法，而且會降低效能
* 
##### Reference

* [Wiki: Fail-fast](https://en.wikipedia.org/wiki/Fail-fast)

* [Google C++ Style: Exception](https://google.github.io/styleguide/cppguide.html#Exceptions)

* [敏捷式例外處理設計的第一步：決定例外處理等級](http://teddy-chen-tw.blogspot.tw/2010/03/blog-post_13.html)



