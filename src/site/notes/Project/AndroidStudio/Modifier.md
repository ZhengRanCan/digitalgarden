---
{"tags":["Project/AndroidStudio/Kotlin","Project/AndroidStudio"],"dg-publish":true,"permalink":"/Project/AndroidStudio/Modifier/","dgPassFrontmatter":true}
---

## 对于Componse函数什么时候用Modifier的疑惑
弄清楚Componse对于Modifier能做的事情
Modifier主要负责四个大类：
1. 修改Componse组件的尺寸、布局，样式，行为
2. 为Componse控件增加额外的信息，如无障碍标签
3. 处理用户的输入
4. 添加上层交互功能，如让组件变得可交互

### 修改Componse组件的尺寸、布局，样式，行为
```kotin
modifier = Modifier
		   .fillMaxSize()
		   .wrapContentSize(align = Alignment.CenterStart)
		   .clip(CircleShape)
		   .rotate
		   .
```

### 处理用户的输入
```kotlin
modifier = Modifier
		   .pointerInput()
```
pointerInput () 更接近底层一点，clickable要更加更加上层
#### 添加上层交互功能，
```kotlin
modifier = Modifier
           .clickable{}
           .verticalScroll(rememberScrollState())
           .draggable()
           
```
### 串接顺序有影响
1. Modifier的链式调用模式对于串接的顺序是有要求的

## 为Composable函数增加Modifier参数
比如一个Icon函数，Modifier可以控制它的样式，但是不应该控制icon的对齐方式，应由父组件去控制
```
@Composable
fun ParentLayout(modifier: Modifier = Modifier) {
    Column {
        IconImage(Modifier.align(Alignment.CenterHorizontally))
    }
}

@Composable
fun IconImage(modifier: Modifier = Modifier) {
    Image(
        painter = painterResource(id = R.drawable.icon),
        contentDescription = "Icon Image",
        modifier = modifier
            .wrapContentSize()
            .background(Color.Gray)
            .padding(18.dp)
            .border(5.dp, Color.Magenta, CircleShape)
            .clip(CircleShape)
    )
}

```
# REF:
1. [写给初学者的Jetpack Compose教程，Modifier-CSDN博客](https://blog.csdn.net/guolin_blog/article/details/132253342)