---
{"tags":["Project/AndroidStudio/Kotlin"],"dg-publish":true,"permalink":"/Project/AndroidStudio/Executor/","dgPassFrontmatter":true}
---

# 介绍及使用
在Android开发中，`Executor`是用于管理和控制线程的一个接口，它为异步任务的执行提供了一种统一的调度机制。`Executor`是Java标准库的一部分，Android通过它来简化并发编程，避免手动创建和管理线程的复杂性。
## 主要功能和优势
1. **线程池管理**：`Executor`可以通过线程池来管理多个线程的创建和销毁，减少了开销，提高了性能。
2. **任务调度**：它可以方便地调度任务的执行，指定任务执行的顺序和方式。
3. **简化代码**：通过使用`Executor`，开发者不需要直接处理线程的细节，只需关注任务本身的实现。
## 常见的Executor类型
### SingleThreadExecutor
1. 使用单个线程来执行所有提交的任务
2. 适用于需要保证任务按顺序执行的场景
```java
ExecutorService executor = Executors.newSingleThreadExecutor();
executor.execute(new Runnable() {
    public void run() {
        // Task to be executed
    }
});
```
### FixedThreadPool：
1.  创建一个固定大小的线程池。
2. 适用于需要限制并发线程数量的场景。
```java

ExecutorService executor = Executors.newFixedThreadPool(4);
for (int i = 0; i < 10; i++) {
    executor.execute(new Runnable() {
        public void run() {
            // Task to be executed
        }
    });
}
```
### CachedThreadPool:
1. 创建一个按需创建线程的线程池
2. 适用于执行很多短期异步任务的场景
```kotlin
ExecutorService executor = Executors.newCachedThreadPool();
for (int i = 0; i < 10; i++) {
    executor.execute(new Runnable() {
        public void run() {
            // Task to be executed
        }
    });
}
```
### ScheduleThreadPool:
1. 创建一个可以定期或延迟执行任务的线程池
2. 适用于需要定期执行任务的场景
```kotlin
ScheduledExecutorService executor = Executors.newScheduledThreadPool(4);
executor.scheduleAtFixedRate(new Runnable() {
    public void run() {
        // Task to be executed
    }
}, 0, 1, TimeUnit.SECONDS);

```
---
# 注意点
## 与协程的区别
协程运行在现有的线程上，但是它们本身不直接管理线程。而Executor管理底层线程资源，通过线程池来优化资源的使用。`Executor` 适用于传统的基于线程的并发模型，而 `CoroutineScope` 利用了Kotlin协程的优势，提供了更加灵活和高效的异步编程方式。在现代Android开发中，Kotlin协程逐渐成为处理异步任务的首选。

## 线程池大小
线程池大小指的是线程池中同时运行的线程数量上限，而不是任务数量。线程池通过限制并发线程数来优化资源使用和系统性能。
## submit 与 execute 的应用场景
- **submit**：当你需要任务的返回结果或需要检查任务的状态时使用。适用于需要异步处理并获取结果的任务。
- **execute**：当你只需要执行任务且不关心任务结果时使用。适用于简单的异步任务执行。
```kotlin
import java.util.concurrent.*

fun main() {
    // 创建一个固定大小为3的线程池
    val executor = Executors.newFixedThreadPool(3)

    // 使用execute方法提交任务，不关心结果
    executor.execute {
        println("Task executed using execute method")
    }

    // 使用submit方法提交Runnable任务，得到Future对象
    val futureRunnable: Future<*> = executor.submit {
        println("Task executed using submit with Runnable")
    }

    // 使用submit方法提交Callable任务，得到Future对象并获取结果
    val futureCallable: Future<String> = executor.submit(Callable {
        "Result from Callable task"
    })

}

```
# REF:
1. [遗嘱执行人 |Android 开发人员 (google.cn)](https://developer.android.google.cn/reference/java/util/concurrent/Executor)
2. [[Project/AndroidStudio/协程\|协程]]
3. [[Project/AndroidStudio/Runnable接口\|Runnable接口]]
4. [[Project/AndroidStudio/Future对象\|Future对象]]