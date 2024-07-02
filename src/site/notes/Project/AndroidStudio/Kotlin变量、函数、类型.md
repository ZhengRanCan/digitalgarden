---
{"tags":["Project/AndroidStudio/Kotlin"],"dg-publish":true,"permalink":"/Project/AndroidStudio/Kotlin变量、函数、类型/","dgPassFrontmatter":true}
---

## 基本规则 ：
1. 变量一般而言非空，声明时一般要初始化，因为变量没有默认值
```Kotlin
val view:View = View
//如果我们已经确定这个变量一定不会为空，但是第一时间无法赋值的，我们设置一个lateinit属性
lateinit val view:View
//解除非空限制
var name:String? = null
//重点关注于使用时非空
```
2. 类型推断，而非动态类型
3. 基本类型跟JAVA的不一样
	1. 基本类型为：数字、字符、布尔值、数组、字符串

## val
1. `val`（只读引用）：一旦变量被初始化，它的引用不能再改变。但是，引用的对象本身可以是可变的。
2. `val` 关键字用于声明只读变量，但这并不意味着变量引用的对象是不可变的。它只是意味着你不能重新分配这个变量指向一个新的对象。然而，变量引用的对象本身可能是可变的。
## by和赋值的区别
```kotlin
val view:ViewModel = ViewModel 
val view:ViewModel by ViewModel
```
1. 直接初始化：第一行是将ViewModel的一个实例对象赋值给view变量
2. 属性委托：第二行使用Kotlin的属性委托语法，`by` 关键字表示将 `view` 属性的实现委托给 `viewModels（）` 返回的对象，返回一个懒加载的 `ViewModel` 实例
3. 直接初始化：适用于立即创建实例的情况，不涉及生命周期管理
4. 属性委托：适用于延迟初始化和需要Android生命周期继承的情况

# 变量非空
kotlin中的变量一般不为null，但在从远程数据库或者ViewModel中可能会为空，如果函数设置的变量的不为空，此时会出现Type mismatch: inferred type is Bitmap? but Bitmap was expected。
有几种解决方法：确保传送的对象是非空
## 从源头出发
### 初始化确保变量非空，
就是不要用bitmap？这种形式

## 从调用出发
### 提供默认值
在patientInfo为空时提供一个默认值
patientInfo = patient ?: defaultPatientInfo
### 条件检查
在调用 `PhotoUploadSection` 之前检查 `patientInfo` 是否为空，并根据检查结果进行不同的处理。
```kotlin
if (patient != null) {
    PhotoUploadSection(
        launcher = launcher,
        selectedImageUri = selectedImageUri,
        context = context,
        reportViewModel = reportViewModel,
        patientInfo = patient
    )
} else {
    // 处理 patient 为空的情况，例如显示一个提示或默认界面
    Text(text = "请选择一个患者")
}
```
### [[Project/AndroidStudio/let函数\|let函数]]
使用 `let` 函数在 `patientInfo` 非空时执行操作。
```kotlin
name?.let {
    println("Name is $it")
}

```
### 使用非空断言
如果确定某个？对象在某个上下文中一定是非空的，可以使用非空断言（!！）将其转换为非空类型

# REF：
1. [【码上开学】Kotlin 的变量、函数和类型 (rengwuxian.com)](https://rengwuxian.com/kotlin-basic-1/)