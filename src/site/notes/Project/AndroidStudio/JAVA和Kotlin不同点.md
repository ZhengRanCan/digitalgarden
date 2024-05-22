---
{"tags":null,"dg-publish":true,"permalink":"/Project/AndroidStudio/JAVA和Kotlin不同点/","dgPassFrontmatter":true}
---

## 内容
1. 构造函数，Kotlin用Construct和初始块，JAVA则是用同名函数去做
2. final和val
	1. final和val区别不大，但是val有setter和getter两个方法，可以改一下getter，从而改变输出
	2. Kotlin函数参数默认是Val类型，所以参数前面不需要写val关键字
3. Kotlin中没有静态函数的概念，用其他替换掉了，
	1. object：用于单例对象，
		1. 全局唯一实例
	2. companion object：定义在类内部的对象，用于访问类的私有成员
	3. 顶层设计：将对象或成员直接写在文件的最上面，不用类包装
	4. 工具类功能：写在顶层函数，继承别的类，用object
4. const：只有基本类型和 String 类型可以声明成常量
5. 数组和集合
	1. Kotlin的数组是拥有泛型的类
	2. 三种集合类型：List、Set和Map
		1. List以固定顺序存储一组元素
			1. 元素不是基本类型时，相比 `Array`，用 `List` 更方便些。
		2. Set存储一组不相等的元素，顺序不定
		3. Map：存储键值对的数据集合
		4. 可变集合/不可变集合：只有mutableMapOf () 创建的Map才可以变化



## 静态函数：
在Java中，"静态对象"一般指的是静态变量引用的对象或包含静态成员的对象。静态成员属于类，而不是类的实例。这意味着静态成员在所有实例之间共享，可以通过类名直接访问，而不需要创建类的实例。
单例对象（Singleton Object）是一种设计模式，用于确保一个类只有一个实例，并提供一个全局访问点。

# REF:
1. [Kotlin 里那些「不是那么写的」 (rengwuxian.com)](https://rengwuxian.com/kotlin-basic-2/)