---
{"tags":["Project/AndroidStudio","Project/AndroidStudio/Kotlin"],"dg-publish":true,"permalink":"/Project/AndroidStudio/Kotlin的Lambda/","dgPassFrontmatter":true}
---

Higher Order function (高阶函数)：函数的参数和返回值有函数类型
Kotlin不支持函数作为参数传递，这时候需要将函数变化为对象，才可以传递
函数类型的对象：三种方法
1. 双冒号：函数名左边加个双冒号，表示一个有相同函数功能的对象
	1.  就是调用对象的invoke属性
2. 匿名函数：就是直接把函数给拿到参数使用
```kotlin
//1
a(fun(param:Int):String{
	return param.toString
})
//2
class view:View{
	fun setOnclickListener(onClick:(View)->Unit){
		this.onClick = onClick
	}
}

view.setOnclickListerer(fun(v:View):Unit{
	//
})
```
3. Lambda：
```kotlin
//2的简化版本
view.setOnclickListener({v:View->})
//如果lambda是函数的最后一个参数，可以把lambda写在参数的外面
view.setOnclickListener(){v:View->}
//如果lambda是函数唯一的参数，可以把括号给去了
view.setOnclickListener{v:View->}
//如果lambda只有一个参数，参数可以省略不写
view.setOnclickListener{
//lambda对于这唯一的参数，有默认的名字-it

}
//如果函数作为对象赋值给一个变量，那么不用上面这种最简化的，因为推不出来函数的类型和返回值
//lambda能根据语段的最后一句推断出返回值类型，不用return
```
