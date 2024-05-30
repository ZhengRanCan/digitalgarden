---
{"tags":["Project/AndroidStudio/Kotlin"],"dg-publish":true,"permalink":"/Project/AndroidStudio/Componse组件/","dgPassFrontmatter":true}
---

全面介绍Componse组件，大概介绍Text、TextField、Preview、Column、Row、Button、Card、AlertDialog、MaterialDesign
# 基本使用
## 可组合函数
在Componse中，可组合函数用编程方式定义应用程序的所有UI
```kotlin

@Composable
fun SimpleText(displayText: String) {
    Text(text = displayText)
}

//调用
SimpleText(getString("I am learning"))

```
## 样式应用
```kotlin
//参数displayText为要显示的文本，style为样式，maxLines为文本允许的最大行数
@Composable
fun SetTextStyleing(displayText:String,style:TextStyle?=null,maxLines:Int? =null){
	Text(
		text = displayText,
		modifier = modifier.padding(16.dp),
		style = style?:TextStyle.Default,
		overflow = TextOverflow.Ellipsis,
		maxLines = maxLines?:Int.MAX_VALUE
	)
}
```
## 预览
在Composable注解上面再加一个注解@Preview

# 组件介绍
## [[Project/AndroidStudio/Scaffold\|Scaffold]]
## Column
将所有子级垂直放置，类似View系统中的LinearLayout
如果要水平放置，则选择Row，下面的把Column改成Row就行
### Scrollable Column
如果内部内容超出了屏幕内容，可以对屏幕滑动。类似于View系统中的ScrollView
### Lazy Column
Scollable Column会一开始加载所有内容，而LazyColumn不会。类似于View系统中的RecycleView，Listview
```kotlin
@Composable
fun LazyColumnScrollableComponent(blogList: List<Blog>) {
    LazyColumnFor(items = blogList, modifier = Modifier.fillMaxHeight()) { blog ->
        val context = ContextAmbient.current
        Card(
            shape = RoundedCornerShape(4.dp),
            modifier = Modifier.fillParentMaxWidth().padding(16.dp).clickable(onClick = {
                Toast.makeText(context, "Author: ${blog.author}", Toast.LENGTH_SHORT).show()
            }),
            backgroundColor = Color(0xFFFFA867.toInt())
        ) {
            Text(
                blog.name, style = TextStyle(
                    fontSize = 16.sp,
                    textAlign = TextAlign.Center
                ), modifier = Modifier.padding(16.dp)
            )
        }
    }
}
```

## Box
一种组合式布局，用于相对于其边缘放置子级
## Button
用户点击时执行某些操作
1. 加入圆角：shape = RoundedCornerShape (12. dp)
2. 制作一个带边框的Button：border = BorderStroke (width = 1. dp, brush = SoildColor (Color. Green))

## Card
做一个卡片式布局

## Image
显示图像，直接val image = imageResource (R.drawable.)

## Alert Dialog
显示一个对话框出来，在AlertDialog中有标题、文本、确认按钮和关闭按钮
```kotlin
@Composable
fun AlertDialogComponent() {
    val openDialog = remember { mutableStateOf(true) }
    if (openDialog.value) {
        AlertDialog(
            onDismissRequest = { openDialog.value = false },
            title = { Text(text = "Alert Dialog") },
            text = { Text("Hello! I am an Alert Dialog") },
            confirmButton = {
                TextButton(
                    onClick = {
                        openDialog.value = false
                        /* Do some other action */
                    }
                ) {
                    Text("Confirm")
                }
            },
            dismissButton = {
                TextButton(
                    onClick = {
                        openDialog.value = false
                        /* Do some other action */
                    }
                ) {
                    Text("Dismiss")
                }
            },
            backgroundColor = Color.Black,
            contentColor = Color.White
        )
    }
}
```
## Material AppBar
分为：TopAppBar或BottomAppBar
```kotlin
@Composable
fun TopAppBarComponent() {
    TopAppBar(
        modifier = Modifier.padding(16.dp).fillMaxWidth(),
        title = { Text("App Name") },
        navigationIcon = {
            IconButton(onClick = { /* doSomething() */ }) {
                Icon(Icons.Filled.Menu)
            }
        },
        actions = {
            IconButton(onClick = { /* doSomething() */ }) {
                Icon(Icons.Filled.Favorite)
            }
            IconButton(onClick = { /* doSomething() */ }) {
                Icon(Icons.Filled.Favorite)
            }
        }
    )
}
```
## Material BottomNavigation
BottomNavigation用于在屏幕底部显示应用程序的一些重要操作
```kotlin
@Composable
fun BottomNavigationWithLabelComponent() {
    var selectedItem by remember { mutableStateOf(0) }
    val items = listOf("Home", "Blogs", "Profile")
    BottomNavigation(
        modifier = Modifier.padding(16.dp).fillMaxWidth(),
        backgroundColor = Color.Black,
        contentColor = Color.Yellow
    ) {
        items.forEachIndexed { index, item ->
            BottomNavigationItem(
                label = { Text(text = item) },
                icon = { Icon(Icons.Filled.Favorite) },
                selected = selectedItem == index,
                onClick = { selectedItem = index }
            )
        }
    }
}
```
## TextField
可编辑文本
```kotlin
TextField(
    value = text,
    onValueChange = {
        text = it
    },
    singleLine = true,//将可编辑文本设置成只有一行
    label = {Text("邮箱")},//label标签，聚焦的时候会改变标签字体大小
    leadingIcon = {
	    Icon(Icon.Filled.Search,null)//在文本框前加一些提示
    },
    trailingIcon = {
	    Text()//在尾部加一些提示
    },
    modifier = Modifier.height(20.dp)
    //Color参数，有好多，直接看官方文档吧，嘻嘻
    colors = TextFieldDefaults.textFieldColors(
	    textColor = Color(0xFF0079D3),
	    backgroundColor = Color.Transparent
    )
)
```
## Material Checkbox
当我们有多个选项并且用户可以选择或取消选择选项，使用复选框
## Material ProgressBar
进度条，用于实现应用程序中正在发生的一些进度
## Material Slider
滑块
## Material Snackbar
在屏幕底部显示一些信息，并放置在所有UI元素上。

## Crossfade动画
我们也可以在 Compose 中使用动画。
```kotlin
@Composable
fun CrossFadeAnimation() {
    val colors = listOf(Color.Red, Color.Green, Color.Blue, Color.Gray)
    var current by remember { mutableStateOf(colors[0]) }
    Column(modifier = Modifier.fillMaxSize()) {
        Crossfade(current = current) { color ->
            Box(Modifier.fillMaxSize().clickable(
                onClick = {
                    current = colors.random()
                }
            ).background(color))
            Text(
                modifier = Modifier.fillMaxSize(),
                textAlign = TextAlign.Center,
                text = "Click To See"
            )
        }
    }
}
```
# REF:
1. [TextField | 你好 Compose (jetpackcompose.cn)](https://jetpackcompose.cn/docs/elements/textfield/)（官方）
2. https://blog.csdn.net/qq_39312146/article/details/131353491 （他觉得他全面，但是感觉一般）
3. https://www.composables.com/material3/outlinedtextfield