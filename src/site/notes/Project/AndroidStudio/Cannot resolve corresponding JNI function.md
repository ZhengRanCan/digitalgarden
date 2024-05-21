---
{"tags":null,"dg-publish":true,"permalink":"/Project/AndroidStudio/Cannot resolve corresponding JNI function/","dgPassFrontmatter":true}
---

## 问题 ：
当把别人的c++代码搬到自己这边来的时候，出现问题
![Pasted image 20240412212420.png](/img/user/Project/AndroidStudio/%E5%9B%BE%E7%89%87/Pasted%20image%2020240412212420.png)
## 原因 ：
没有理解JNI
上图函数名中，除了原函数外，还有前缀，其中有部分是我们创建的package包的名称，另一部分是java文件名
## 解决：
改对应的java文件名字或者包名

# REF:
1. [Cannot resolve corresponding JNI function 一个可能的原因-CSDN博客](https://blog.csdn.net/sinat_38068956/article/details/88806232)
2. [“无法解析相应的 JNI 函数” Android Studio - 堆栈溢出 (stackoverflow.com)](https://stackoverflow.com/questions/42914167/cannot-resolve-corresponding-jni-function-android-studio)