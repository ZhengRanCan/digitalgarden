---
{"tags":["Project/AndroidStudio/Kotlin"],"dg-publish":true,"permalink":"/Project/AndroidStudio/Runnable接口/","dgPassFrontmatter":true}
---

`Runnable` 接口是一个功能接口，表示一个可以运行的任务，它只有一个抽象方法 `run( )`，需要在类中定义具体的任务逻辑
```kotlin
val runnable = Runnable {
    println("Task is running")
}

val executor = Executors.newSingleThreadExecutor()
executor.execute(runnable)
executor.shutdown()

```

# REF:
1. 