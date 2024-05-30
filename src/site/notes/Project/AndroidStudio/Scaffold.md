---
{"tags":["Project/AndroidStudio/Kotlin"],"dg-publish":true,"permalink":"/Project/AndroidStudio/Scaffold/","dgPassFrontmatter":true}
---

三个部分：
1. topBar: 屏幕顶部的应用栏
2. BottomBar：屏幕最底部的应用栏
3. floatingActionButton：悬停屏幕右下角的按钮，用于操作关键操作，我直接不吊他，是不是很大胆

通常用这个组件结合 [[Project/AndroidStudio/Navigation\|Navigation]] 来写屏幕主页，使用例子
```kotlin
@Composable  
fun MainScreen(){  
	val navController = rememberNavController()  
	Scaffold(  
		bottomBar = {BottomNavigationBar(navController = navController)}  
	) { innerPadding->  
		//Scaffold会自动为内容部分添加一些填充空间用来避免与底部按钮条重叠  
		Box(modifier = Modifier.padding(innerPadding)) {  
		NavigationHost(navController = navController)  
		}  
	}  
}
//底部按钮条与屏幕切换同步
@Composable  
fun BottomNavigationBar(navController: NavController){  
	val items = listOf(  
		NavigationItem.Home,  
		NavigationItem.Favorites,  
		NavigationItem.Profile  
	)  
	BottomNavigation(  
		backgroundColor = Color(0xFFECD99E)  
	) {  
		//当前路径  
	val currentRoute = navController.currentBackStackEntryAsState().value?.destination?.route  
		items.forEach { item ->  
			BottomNavigationItem(  
				icon = { Icon(item.icon, contentDescription = item.title) },  
				label = { Text(item.title) },  
				selected = currentRoute == item.route,  
				onClick = {  
					navController.navigate(item.route) {  
						popUpTo(navController.graph.startDestinationId) {  
							saveState = true  
					}  
					launchSingleTop = true  
					restoreState = true  
					}  
				}  
			)  
		}  
	}  
}

//Navigation主页
@Composable  
fun NavigationHost(navController: NavHostController){  
	NavHost(navController=navController, startDestination = NavigationItem.Home.route) {  
		composable(NavigationItem.Home.route) {  
		HomeScreen()  
		}  
		composable(NavigationItem.Favorites.route) {  
			FavoritesScreen()  
		}  
		composable(NavigationItem.Profile.route) {  
			ProfileScreen()  
		}  
	}  
}


```




# REF:
1. https://developer.android.com/develop/ui/compose/components/scaffold?hl=zh-cn