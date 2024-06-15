---
{"tags":["Project/AndroidStudio/Kotlin"],"dg-publish":true,"permalink":"/Project/AndroidStudio/Navigation/","dgPassFrontmatter":true}
---

# 介绍
使用Navigation组件进行导航，可以创建多界面应用
Navigation组件共有三个部分：
1. NavController
2. NavGraph
3. NavHost
# 使用
## 在应用为目标页面定义路线
1. 在Compose应用中，导航的一个基本概念就是路线。路线是与目标页面对应的字符串。这类似于网址的概念。就像不同网址映射到网站上的不同页面一样，路线是可映射至目标页面并作为其唯一标识符的字符串。目标页面通常是与用户看到的内容相对应的单个可组合项或一组可组合项。
2. 用枚举类来定义应用的路线
3. ~~在Activity中创建一个枚举类~~
4. 通过GPT好像发现了一种更好的数据形式（object创建）
```kotlin
//枚举类
enum class CupcakeScreen() {
	Start,
	Flavor,
	Pickup,
	Summary
}
//一开始GPT给的是`selected class`形式
sealed class NavigationItem(var route: String, var icon: ImageVector, var title: String) {
    object Home : NavigationItem("home", Icons.Filled.Home, "首页")
    object Status : NavigationItem("status", Icons.Filled.Assessment, "状态")
    object Dynamic : NavigationItem("dynamic", Icons.Filled.TrendingUp, "动态")
    object Profile : NavigationItem("profile", Icons.Filled.Person, "我的")
}
//优点
//类型安全，单一实例
//缺点：
//扩展不变，额外的类开销大

//`object`和`data class`形式
object NavigationItem {
    val Home = NavItem("home", Icons.Filled.Home, "首页")
    val Status = NavItem("status", Icons.Filled.Assessment, "状态")
    val Dynamic = NavItem("dynamic", Icons.Filled.TrendingUp, "动态")
    val Profile = NavItem("profile", Icons.Filled.Person, "我的")

    data class NavItem(val route: String, val icon: ImageVector, val title: String)
}
//简洁，灵活，集中管理但是类型安全弱
//对我个人而言的话就是容易集中管理吧，可以把导航想都写在一个`object里`

//object
object NavigationItem {  
	val Home = NavItem("home", Icons.Filled.Home, "看舌")  
	val Favorites = NavItem("favorites", Icons.Filled.Favorite, "状态")  
	val Profile = NavItem("profile", Icons.Filled.Person, "我的")  
	  
	data class NavItem(val route: String, val icon: ImageVector, val title: String)  
}
```
## 为应用添加NavHost
NavHost是一个可组合项，会根据路线来显示其他可组合项目目标页面。如果路线为Flavor，NavHost会显示对应的屏幕
NavHost接收函数类型作为其内容
在 `NavHost` 的内容函数中，调用 `composable()` 函数。`composable()` 函数有两个必需参数。
1.  **`route`**：与路线名称对应的字符串。这可以是任何唯一的字符串。您将使用 `CupcakeScreen` 枚举的常量的名称属性。
2. **`content`**：您可以在此处调用要为特定路线显示的可组合项
3. 针对每一个路线需要调用一次composable () 函数
4. composable（）函数是NavGraphBuilder的扩展函数
```kotlin
//使用例
NavHost(
	navController,//NavHostController类的实例，通过调用navigate()方法导航到另一个目标页面
	startDestination,//用来显示初始界面
	modifier,
){
	//content
}
//真使用
@Composable
fun NavHostDemo() {
    val navController = rememberNavController()
    NavHost(navController = navController, startDestination = RouteConfig.ROUTE_PAGEONE) {

        composable(CupcakeScreen.Start.name) {
            PageOne(navController)
        }

        composable(CupcakeScreen.Flavor.name) {
            PageTwo(navController)
        }
    }
}


```
## 页面跳转
传入NavController，使用NavController.navigate进行跳转
```kotlin
@Composable
fun PageOne(navController:NavController){
	Button(onClick = { //点击跳转到页面2 
	navController.navigate(RouteConfig.ROUTE_PAGETWO) 
	})
}
```
## 传入数据

### NavHost配置
假如页面1跳转到页面2时需要传递一个name参数和age参数
直接将传递的参数使用"/“拼写在路由地址后面添加占位符，默认情况下，所有的参数都会被解析成字符串，所以我们可以使用arguments来为参数指定type类型。
```kotlin
//参数配置
const val PARAMS_NAME = "name"
const val PARAMS_AGE = "age"

//修改NavHost的配置
NavHost(navController = navController, startDestination = RouteConfig.ROUTE_PAGEONE) {

    composable(RouteConfig.ROUTE_PAGEONE) {
        PageOne(navController)
    }
	//固定参数
    composable(
    "${RouteConfig.ROUTE_PAGETWO}/{${ParamsConfig.PARAMS_NAME}}/{${ParamsConfig.PARAMS_AGE}}",
        arguments = listOf(
            navArgument("$ParamsConfig.PARAMS_NAME") {},
            navArgument("$ParamsConfig.PARAMS_AGE") { type = NavType.IntType }
        )
    ) {
        val argument = requireNotNull(it.arguments) 
        val name = argument.getString(ParamsConfig.PARAMS_NAME) 
        val age = argument.getInt(ParamsConfig.PARAMS_AGE) 
        PageTwo(name,age,navController)
    }
    //可选参数
    //类似于get请求的添加方式?name=name,必须设置一个默认值
        composable(
            "${RouteConfig.ROUTE_PAGETWO}/{${ParamsConfig.PARAMS_NAME}}" +
                    "?${ParamsConfig.PARAMS_AGE}={${ParamsConfig.PARAMS_AGE}}",
            arguments = listOf(
                navArgument(ParamsConfig.PARAMS_NAME) {},
                navArgument(ParamsConfig.PARAMS_AGE) {
                    defaultValue = 30
                    type = NavType.IntType
                }
            )
        ) {
            val argument = requireNotNull(it.arguments)
            val name = argument.getString(ParamsConfig.PARAMS_NAME)
            val age = argument.getInt(ParamsConfig.PARAMS_AGE)
            PageTwo(name, age, navController)
        }

}
```
### PageTwo页面配置
```kotlin
//接收参数
{
        Text(text = "这是页面2")
        Spacer(modifier = Modifier.height(20.dp))
        Text(text = "我是$name,我今年$age 岁了")
        Spacer(modifier = Modifier.height(20.dp))
        Button(onClick = {
            //点击返回页面1
            navController.popBackStack()
        }) {
            Text(
                text = "返回页面1",
                modifier = Modifier.fillMaxWidth(),
                textAlign = TextAlign.Center
            )
        }
    }
```
### PageOne页面
```kotlin
navController.navigate("${RouteConfig.ROUTE_PAGETWO}/黄林晴/26")
```

# 在Navigate中Compose和Fragment的对比
## Compose
- **简洁和声明式**：Compose 的导航是声明式的，代码更简洁，易于理解和维护。
- **轻量级**：Compose 的导航不需要 `Fragment`，直接在 `Composable` 函数之间导航，减少了复杂性和开销。
- **更好的状态管理**：Compose 的状态管理更自然，结合 `ViewModel` 和 `State` 可以更方便地管理界面状态。
## Fragment
### 优点
- **成熟的生态系统**：`Fragment` 和 `NavController` 是 Android 应用开发的标准，生态系统成熟，文档和社区支持丰富。
- **兼容性**：对于已经使用 `Fragment` 的项目，继续使用 `Fragment` 进行导航可以避免大量的代码重构。
### 缺点
- **复杂性**：`Fragment` 的生命周期和状态管理较为复杂，容易引入错误。
- **样板代码**：需要编写大量的样板代码，代码冗长且不易维护。

因为我目前已经专用Compose了，所以也不用去专门搞个Fragment去折磨，之前之所以想用Fragment，大概是不懂[[Project/AndroidStudio/生命周期\|生命周期]]的相关内容。
但是对于除了Navigate之外的内容，还不是很清楚。




# REF:
1. https://juejin.cn/post/7179590175515738168
2. https://developer.android.com/codelabs/basic-android-kotlin-compose-navigation?hl=zh-cn#0
3. [在 Compose 中使用 Navigation 导航，看这一篇就够了~_compose navitation 如何清除所以页面,并打开-CSDN博客](https://blog.csdn.net/weixin_55362248/article/details/123709131) （感觉这一篇讲的好一点...）
4. [[Project/AndroidStudio/Fragment\|Fragment]]