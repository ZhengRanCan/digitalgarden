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

## by和赋值的区别
```
val view:ViewModel = ViewModel 
val view:ViewModel by ViewModel
```
1. 直接初始化：第一行是将ViewModel的一个实例对象赋值给view变量
2. 属性委托：第二行使用Kotlin的属性委托语法，`by` 关键字表示将 `view` 属性的实现委托给 `viewModels（）` 返回的对象，返回一个懒加载的 `ViewModel` 实例
3. 直接初始化：适用于立即创建实例的情况，不涉及生命周期管理
4. 属性委托：适用于延迟初始化和需要Android生命周期继承的情况


# REF：
1. [【码上开学】Kotlin 的变量、函数和类型 (rengwuxian.com)](https://rengwuxian.com/kotlin-basic-1/)