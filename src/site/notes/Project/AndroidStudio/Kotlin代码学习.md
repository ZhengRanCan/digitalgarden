---
{"tags":["#Project/AndroidStudio/Kotlin"],"dg-publish":true,"permalink":"/Project/AndroidStudio/Kotlin代码学习/","dgPassFrontmatter":true}
---

# Kotlin学习
## 基础
[[Project/AndroidStudio/Kotlin变量、函数、类型\|Kotlin变量、函数、类型]]
[[Project/AndroidStudio/JAVA和Kotlin不同点\|JAVA和Kotlin不同点]]
[[Project/AndroidStudio/Kotlin的Lambda\|Kotlin的Lambda]]
[[Project/AndroidStudio/Modifier\|Modifier]]
[[Project/AndroidStudio/状态管理\|状态管理]]
[[Project/AndroidStudio/Navigation\|Navigation]]
[[Project/AndroidStudio/作用域函数\|作用域函数]]
[[Project/AndroidStudio/函数签名\|函数签名]]
[[Project/AndroidStudio/Class\|Class]]
## Compose
1.  [[Project/省中医APP开发/UI/使用Figma转换代码\|使用Figma转换代码]]
2. [[Project/AndroidStudio/Compose组件\|Compose组件]]
3. [[Project/AndroidStudio/Fragment\|Fragment]]
4. [[Project/AndroidStudio/生命周期\|生命周期]]
## 协程
[[Project/AndroidStudio/协程\|协程]]
[[Project/AndroidStudio/kotlin Flow\|kotlin Flow]]
[[Project/AndroidStudio/Executor\|Executor]]
## 数据存储
[[Project/AndroidStudio/数据存储\|数据存储]] (DataStore和SharedPreferences)
[[Project/AndroidStudio/依赖注入\|依赖注入]]
[[Project/AndroidStudio/Room\|Room]]

## [[Project/AndroidStudio/权限申请\|权限申请]]

## 案例
1. [[Project/AndroidStudio/侧滑删除效果\|侧滑删除效果]]
2. 熟练案例: [[Project/AndroidStudio/仿微信界面\|仿微信界面]]
## API
[[Project/AndroidStudio/Coil\|Coil]]
[[Project/AndroidStudio/LaunchedEffect\|LaunchedEffect]]
[[Project/AndroidStudio/CameraX\|CameraX]]
[[Project/AndroidStudio/GenericShape\|GenericShape]]
# REF:
1. [【码上开学】Kotlin 的变量、函数和类型_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1x4411o7Wy/?spm_id_from=333.788&vd_source=ed636aea03b32e53457a090439165487)
2. [Kotlin 里那些「不是那么写的」 (rengwuxian.com)](https://rengwuxian.com/kotlin-basic-2/) 朱凯讲得好啊，可以多看看






将目前说的发给AI，说还要学的
### 基础
1. **Kotlin 基础**
    - **控制流**：条件语句（`if`、`when`）、循环（`for`、`while`）
    - **集合操作**：集合（`List`、`Set`、`Map`）及其操作（`filter`、`map`、`reduce` 等）
    - **异常处理**：`try-catch`、自定义异常
    - **扩展函数**：如何为现有类添加新功能
    - ref: [[Project/AndroidStudio/Kotlin变量、函数、类型\|Kotlin变量、函数、类型]] 、 [[Project/AndroidStudio/JAVA和Kotlin不同点\|JAVA和Kotlin不同点]]
2. **面向对象** 
    - **继承和多态**：`open` 类、`override` 方法
    - **接口和抽象类**：`interface`、`abstract` 类
    - **数据类和密封类**：`data class`、`sealed class`  
    - **对象表达式和声明**：`object` 关键字、单例模式
    - ref: [[Project/AndroidStudio/Class\|class]]
### Compose
1. **Compose 基础**
    - **Compose 主题**：自定义主题、Material 设计
    - **动画**：`animate*` API、过渡动画
    - **手势处理**：点击、拖动、缩放等手势
    - **自定义布局**：创建自定义布局、`Modifier` 的高级用法
    - ref: [[Project/AndroidStudio/Modifier\|Modifier]]、
2. **高级 Compose**
    - **Compose 性能优化**：重组、快照、避免不必要的重组
    - **Compose 测试**：如何编写 UI 测试、使用 `ComposeTestRule`
    - **Compose 与 View 互操作**：在 Compose 中使用传统 View，在 View 中使用 Compose
### 协程
1. **协程进阶**
    - **协程上下文和调度器**：`Dispatchers`、`Job`、`SupervisorJob`
    - **协程异常处理**：`CoroutineExceptionHandler`
    - **结构化并发**：`coroutineScope`、`supervisorScope`
    - **通道和流**：`Channel`、`Flow` 的高级用法
### 数据存储
1. **数据存储进阶**
    - **Room 数据库高级用法**：复杂查询、关系映射、多线程访问
    - **DataStore**：`Proto DataStore` 的使用
    - **加密存储**：如何安全地存储敏感数据
### 网络和 API
1. **网络请求**
    - **Retrofit**：基本使用、拦截器、错误处理
    - **OkHttp**：高级用法、连接池、缓存
    - **GraphQL**：使用 Apollo 客户端进行 GraphQL 请求
2. **API**
    - **WorkManager**：后台任务管理
    - **Paging**：分页加载数据
    - **Navigation Component**：深层链接、导航动画、Safe Args
### 案例
1. **高级案例**
    - **MVVM 模式**：使用 ViewModel 和 LiveData 构建应用
    - **Clean Architecture**：分层架构设计
    - **多模块项目**：如何拆分项目为多个模块
### 测试
1. **单元测试**
    - **JUnit**：基本使用、参数化测试
    - **Mockito**：模拟对象、行为验证
    - **Robolectric**：模拟 Android 环境进行单元测试
2. **UI 测试**
    - **Espresso**：基本使用、与 Compose 的互操作
    - **UI Automator**：跨应用 UI 测试
### 其他
1. **性能优化**
    - **内存优化**：内存泄漏检测、优化内存使用
    - **启动优化**：减少启动时间、优化冷启动和热启动
    - **电量优化**：优化电量消耗、使用 JobScheduler 和 WorkManager
2. **发布和运维**
    - **CI/CD**：使用 GitHub Actions 或 Jenkins 进行持续集成和部署
    - **应用签名**：签名配置、自动签名
    - **应用发布**：发布到 Google Play、使用 App Bundle
