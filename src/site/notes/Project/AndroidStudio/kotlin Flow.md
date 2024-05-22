---
{"tags":["Project/AndroidStudio/Kotlin"],"dg-publish":true,"permalink":"/Project/AndroidStudio/kotlin Flow/","dgPassFrontmatter":true}
---

## 介绍：
1. Kotlin中用于处理异步流式数据的一个重要抽象，是协程的一部分
2. 非阻塞的，只有遇到末端操作符，才会触发所有操作的执行
3. 发射出来的值都是顺序执行的
4.  `map` , `filter` , `take` , `zip` 等等是中间操作符，`collect` , `collectLatest` , `single` , `reduce` , `toList` 等等末端操作符

```kotlin
suspend fun printValue() = flow<Int> {  
    for (index in 1..10) {  
        emit(index)  
    }  
}.map { it -> it * it } // map, filter, take, zip 等等是中间操作符  
.filter { it -> it > 5 }   
.toList() // 只有遇到末端操作符 collect, collectLatest,single, reduce, toList 等等才会触发所有操作的执行
```
# REF:
1. [Kotlin StateFlow 搜索功能的实践 DB + NetWork (qq.com)](https://mp.weixin.qq.com/s?__biz=MzAwNDgwMzU4Mw==&mid=2247483976&idx=1&sn=08acf1ca53f2c496f21423723b8258a9&scene=21#wechat_redirect)
2. [[Project/AndroidStudio/协程\|协程]]