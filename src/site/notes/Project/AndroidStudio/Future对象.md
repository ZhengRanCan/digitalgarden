---
{"tags":["Project/AndroidStudio/Kotlin"],"dg-publish":true,"permalink":"/Project/AndroidStudio/Future对象/","dgPassFrontmatter":true}
---

`Future` 对象表示一个异步计算的结果，你可以使用 `Future` 来检测任务是否完成、等待任务完成，并获取任务的结果。如果任务尚未完成，`get` 方法会阻塞直到任务完成。
```kotlin
val executor = Executors.newSingleThreadExecutor()

val future: Future<String> = executor.submit(Callable {
    Thread.sleep(2000)
    "Task result"
})

println("Task submitted, doing other work")

// 获取结果，会阻塞直到任务完成
val result = future.get()
println("Task result: $result")

executor.shutdown()

```
### CompletableFuture
`CompletableFuture` 是一个更强大的 `Future` 实现，支持完成、取消任务和链式异步操作。
```kotlin
import java.util.concurrent.CompletableFuture

fun main() {
    val completableFuture = CompletableFuture.supplyAsync {
        Thread.sleep(2000)
        "Task result"
    }

    completableFuture.thenAccept { result ->
        println("Task completed with result: $result")
    }

    println("Task submitted, doing other work")

    // 阻塞，等待任务完成
    completableFuture.join()
}

```

# REF:
1. 