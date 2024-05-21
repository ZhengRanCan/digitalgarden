---
{"tags":["Project/JAVA"],"dg-publish":true,"permalink":"/Project/AndroidStudio/JAVA入门/","dgPassFrontmatter":true}
---

# 基础语法

1. 运行第一个程序
```JAVA
$javac HelloWorld.java
$java HelloWorld
```
2. 基础语法
	1. 对象
	2. 类
	3. 方法
	4. 变量
		1. 局部变量：方法、构造函数或者块的变量
		2.  参数变量：
		3. 实例变量：属于实例
		4. 静态变量或类变量：属于类而不是实例
```JAVA
public class HelloWorld {
	public static void main (String[] args){
		System.out.printIn("Hello World")
}
}

//public 访问修饰符
//static 关键字
//void 返回类型
//main 方法名
```

# JAVA对象和类
1. 多态
2. 继承
3. 封装
4. 抽象
5. 类
6. 对象
7. 实例
8. 方法
9. 重载
10. 跟C++很像吧，类中包含对象的共性，有属性和方法，类进行实例化之后就是对象，
	1. 实例化：声明一个对象，使用关键字new来创建对象，在创建对象的时候，会调用构造方法初始化对象。
	2. 一个源文件中只能有一个public类
	3. 一个源文件可以有多个非public类
	4. 源文件的名称应该与public类的类名保持一致
```
public class Pubby{
	Public Pubby(String name){
		System.out.printIn("this is my dog:"+name);
	}
	Public static void main(String[] args){
		Pubby myPubby = new Pubby("Tonny");
	}
}

```

# 基本数据类型
1. byte
	1. 8位，有有符号数
2. short
	1. 16位，有符号数
3. int
	1. 32位，有符号数
4. long
	1. 64位，有符号数
5. float
	1. 单精度、32位
6. double
	1. 双精度，64位
7. boolean
	1. 一位
8. char
	1. 存字符，16位的Unicode字符
9. 可以通过. SIZE查看字节数
10. fina： 常量无法修改
11. 数组
	1. 声明数组变量
		1. dataType【】, arrayRefVar;
		2. arrayRefVar = new dataType【Size】//创建
		3. dataType【】 arr = new datatype 【size】
		4. dataType【】arr = {1,2,3,4...}
	2. For-Each循环
		1. for (type element:arr){System. out. printIn (element); }

# REF:
1. [Java 教程 | 菜鸟教程 (runoob.com)](https://www.runoob.com/java/java-tutorial.html)