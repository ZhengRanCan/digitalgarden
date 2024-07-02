---
{"tags":["Project/AndroidStudio/Kotlin"],"dg-publish":true,"permalink":"/Project/AndroidStudio/Class/","dgPassFrontmatter":true}
---


# 介绍
Kotlin 的 `class` 是定义对象的基本单位。与许多面向对象编程语言类似，Kotlin 的类用于封装数据和功能。Kotlin 的类可以包含属性、方法、初始化块、继承关系等。以下是对 Kotlin `class` 的详细讲解。
## 基本实现
```kotlin
//定义一个简单的类
class Person {
    var name: String = ""
    var age: Int = 0

    fun greet() {
        println("Hello, my name is $name and I am $age years old.")
    }
}
//创建类的实例
val person = Person()
person.name = "John"
person.age = 30
person.greet() // 输出：Hello, my name is John and I am 30 years old.

//主构造函数
//在class的主构造函数中声明的参数会自动成为类的属性，无需显式地进行赋值操作。这是 Kotlin 的一个简化特性。
class Person(val name: String, var age: Int)
//次构造函数
class Person(val name: String) {
    var age: Int = 0

    constructor(name: String, age: Int) : this(name) {
        this.age = age
    }
}

//初始化块
//如果需要在主构造函数之外执行一些初始化逻辑，可以使用 `init` 块：
class Person(val name: String, var age: Int) {
    init {
        println("Person is created: $name, $age")
    }
}

```

## 属性和方法
Kotlin 的类属性可以是 `var`（可变）或 `val`（只读）。属性可以有自定义的 getter 和 setter。
```kotlin
class Person {
    var name: String = "John"
        get() = field.toUpperCase() // 自定义 getter
        set(value) {
            field = value.trim() // 自定义 setter
        }
}

val person = Person()
println(person.name) // 输出：JOHN
person.name = " Jane "
println(person.name) // 输出：JANE

class Person(val name: String) {
    fun greet() {
        println("Hello, my name is $name.")
    }
}

val person = Person("John")
person.greet() // 输出：Hello, my name is John.

```
## 继承
Kotlin 类默认是 `final` 的，不可继承。要使类可继承，需使用 `open` 关键字。
```kotlin
open class Person(val name: String) {
    open fun greet() {
        println("Hello, my name is $name.")
    }
}

class Student(name: String, val studentId: String) : Person(name) {
    override fun greet() {
        println("Hello, my name is $name and my student ID is $studentId.")
    }
}

val student = Student("John", "12345")
student.greet() // 输出：Hello, my name is John and my student ID is 12345.

```
## 抽象类和接口
抽象类不能被实例化，可以包含抽象方法和非抽象方法。
接口可以定义方法和属性，但不能包含状态。类可以实现一个或多个接口。
```kotlin
abstract class Person(val name: String) {
    abstract fun greet()
}

class Student(name: String) : Person(name) {
    override fun greet() {
        println("Hello, my name is $name.")
    }
}

interface Greetable { 
	fun greet() 
} 
class Person(val name: String) : Greetable { 
	override fun greet() { 
		println("Hello, my name is $name.") 
	} 
}
```
## 嵌套类和内部类
嵌套类是静态的，不持有外部类的引用。
内部类持有外部类的引用，可以访问外部类的成员。
```kotlin
class Outer {
    class Nested {
        fun greet() = "Hello from Nested"
    }
}

val nested = Outer.Nested()
println(nested.greet()) // 输出：Hello from Nested

class Outer {
    val name: String = "Outer"

    inner class Inner {
        fun greet() = "Hello from Inner, ${this@Outer.name}"
    }
}

val inner = Outer().Inner()
println(inner.greet()) // 输出：Hello from Inner, Outer

```

## 单例
单例（Singleton）是一种设计模式，它确保一个类只有一个实例，并提供全局访问点。单例模式通常用于控制对资源的访问，例如数据库连接、文件系统访问器、日志记录器等。
在 Kotlin 中，单例模式可以通过 `object` 关键字轻松实现。使用 `object` 定义的类是线程安全的，并且在整个应用程序运行期间只有一个实例。
```kotlin
object Singleton {
    var counter: Int = 0

    fun increment() {
        counter++
    }
}

fun main() {
    Singleton.increment()
    Singleton.increment()
    println(Singleton.counter) // 输出：2
}

```
### 单例的使用场景
1. **日志记录**：在应用程序中使用单例来集中管理日志记录。
2. **配置管理**：单例可以用于读取和保存应用程序的配置信息。
3. **缓存**：单例可以用于实现全局缓存，提高性能。
4. **数据库连接**：确保只有一个数据库连接实例，以避免资源争用和数据不一致问题。
# Data Class
## DataClass
### 特点
1. **自动生成的函数**：
- `equals()`：比较两个对象是否相等。
- `hashCode()`：生成对象的哈希码。
- `toString()`：返回对象的字符串表示，包含所有属性。
- `copy()`：创建一个对象的副本，可以选择修改部分属性。
- `componentN()`：用于解构声明。
2. **主要用于数据容器**：
- `data class` 主要用于存储数据的对象，旨在减少样板代码。
### 使用场景
- 存储和传递数据。
- 需要对比对象的内容而不是引用。
- 需要生成对象的字符串表示。
- 需要使用解构声明来提取对象的属性。
```kotlin
data class ReportData(
    val patient: String = "郑栩轩",
    val analysis: String = "这是舌象分析",
    val report: String = "这是舌象报告",
    val time: String
)
```




# REF:
1. 