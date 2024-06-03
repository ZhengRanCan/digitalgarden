---
{"tags":["Project/AndroidStudio/Kotlin"],"dg-publish":true,"permalink":"/Project/AndroidStudio/LaunchedEffect/","dgPassFrontmatter":true}
---

Jetpack Compose的一个API，用于在组合函数中启动[[Project/AndroidStudio/协程\|协程]]，主要用途是在特定的生命周期或状态变化时执行一次性的操作，例如初始化、加载数据或启动动画等。

## 基本用法：
1. `LaunchedEffect` 会在其键（key）发生变化时重新启动其块中的代码。它的生命周期与组合函数的生命周期相关联，当组合函数退出组合时，`LaunchedEffect` 会被取消。
### 参数
1.  **key**：`LaunchedEffect` 的键，当键的值发生变化时，`LaunchedEffect` 会重新启动。可以是单个值或多个值的组合。
2.  **block**：要执行的代码块，通常是一个挂起函数。




# REF:
1. 