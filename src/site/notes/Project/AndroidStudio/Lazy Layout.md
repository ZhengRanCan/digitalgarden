---
{"tags":["Project/AndroidStudio","Project/AndroidStudio/Kotlin"],"dg-publish":true,"permalink":"/Project/AndroidStudio/Lazy Layout/","dgPassFrontmatter":true}
---

Lazy Layout只是一个可复用列表的统称，分为LazyColumn和LazyRow，与View系统相比的话就是Listview和RecyclerView
## 用法示例 ：
```kotlin
@Composable
fun ScrollableList() {
    val list = ('A'..'Z').map { it.toString() }
    LazyColumn {
        items(list) { letter ->
            Card(
                modifier = Modifier
                    .fillMaxWidth()
                    .height(120.dp)
            ) {
                Text(
                    text = letter,
                    textAlign = TextAlign.Center,
                    fontSize = 20.sp,
                    modifier = Modifier
                        .fillMaxSize()
                        .wrapContentHeight(Alignment.CenterVertically)
                )
            }
        }
    }
}
```
得到每行对应的index
```kotlin
@Composable
fun ScrollableList() {
    val list = ('A'..'Z').map { it.toString() }
    LazyRow {
        itemsIndexed(list) { index, letter ->
            Card(
                modifier = Modifier
                    .width(120.dp)
                    .height(200.dp)
            ) {
                Text(
                    text = "$index $letter",
                    textAlign = TextAlign.Center,
                    fontSize = 20.sp,
                    modifier = Modifier
                        .fillMaxSize()
                        .wrapContentHeight(Alignment.CenterVertically)
                )
            }
        }
    }
}
```
## rememberLazyListState
调用rememberLazyListState函数，将能够得到一个LazyListState对象。
```kotlin
val state = rememberLazyListState() 
//可以通过访问它的firstVisibleItemIndex属性来得知当前第一个可见子项元素的下标。
state.firstVisibleItemIndex 
//可以访问firstVisibleItemScrollOffset属性来得到当前第一个可见子项元素的偏移距离。
state.firstVisibleItemScrollOffset
```
示例
```kotlin
@Composable
fun MainLayout() {
    val state = rememberLazyListState()
    Box {
        ScrollableList(state)
        val shouldShowAddButton = state.firstVisibleItemIndex  == 0
        AddButton(shouldShowAddButton)
    }
}

@Composable
fun ScrollableList(state: LazyListState) {
    val list = ('A'..'Z').map { it.toString() }
    LazyColumn(state = state) {
        items(list) { letter ->
            Card(
                modifier = Modifier
                    .fillMaxWidth()
                    .height(120.dp)
                    .padding(10.dp)
            ) {
                Text(
                    text = letter,
                    textAlign = TextAlign.Center,
                    fontSize = 20.sp,
                    modifier = Modifier
                        .fillMaxSize()
                        .wrapContentHeight(Alignment.CenterVertically)
                )
            }
        }
    }
}

@Composable
fun BoxScope.AddButton(isVisible: Boolean) {
    if (isVisible) {
        FloatingActionButton(
            onClick = { /*TODO*/},
            shape = CircleShape,
            modifier = Modifier
                .align(Alignment.BottomEnd)
                .padding(20.dp)
        ) {
            Icon(Icons.Filled.Add, "Add Button")
        }
    }
}
```

# REF：
1. [写给初学者的Jetpack Compose教程，Lazy Layout_郭霖 jetpack compose 教程-CSDN博客](https://guolin.blog.csdn.net/article/details/135038175?spm=1001.2014.3001.5502)