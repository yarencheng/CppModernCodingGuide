# 不要：使用`goto end`來釋放資源

##### Reason

* 容易讓程式形成 spaghetti code，容易產生Bug
* 當程式符合 RAII，將不再需要`goto end`。

##### Reference

* [Wiki: Spaghetti Code](/c438706e88b5068111c4a223e3c542b48a7f5324)

* [C++ Core Guidelines: Don't: Place all cleanup actions at the end of a function and goto exit](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#nr6-dont-place-all-cleanup-actions-at-the-end-of-a-function-and-goto-exit)



