# 不要：使用`goto end`來釋放資源

Reason

* 容易讓程式形成 spaghetti code，容易產生Bug
* 當程式符合 RAII，將不再需要`goto end`。





