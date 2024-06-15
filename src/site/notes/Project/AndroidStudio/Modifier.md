---
{"tags":["Project/AndroidStudio/Kotlin"],"dg-publish":true,"permalink":"/Project/AndroidStudio/Modifier/","dgPassFrontmatter":true}
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


# 特殊的修饰符
## Modifier. Align，Alignment与Arrangement
在 Jetpack Compose 中，`Modifier.align` 和 `Alignment` 用于控制组件在其父容器中的对齐方式。
### Modifier.align

`Modifier.align` 用于将一个组件在其父容器内对齐。它通常用于容器内部的子组件。
#### 常见的 `Alignment` 值
- `Alignment.TopStart`: 左上角对齐
- `Alignment.TopCenter`: 上中对齐
- `Alignment.TopEnd`: 右上角对齐
- `Alignment.CenterStart`: 左中对齐
- `Alignment.Center`: 中心对齐
- `Alignment.CenterEnd`: 右中对齐
- `Alignment.BottomStart`: 左下角对齐
- `Alignment.BottomCenter`: 下中对齐
- `Alignment.BottomEnd`: 右下角对齐
### Alignment
`Alignment` 是一个接口，定义了组件在其父容器内的对齐方式。它可以在容器的 `contentAlignment` 参数中使用，以指定所有子组件的对齐方式。
### `Modifier.align` 与 `Alignment` 的区别

- **`Modifier.align`**: 用于单个子组件的对齐，作用于具体的子组件。
- **`Alignment`**: 通常作为父容器的参数，用于设置所有子组件的对齐方式。
- 当两者同时设置时，以Modifier. align为准
### Arrangement
`Arrangement` 控制的是 `Row` 或 `Column` 中子组件之间的排列和间距。`Arrangement` 提供了一些预定义的值，用于不同的排列策略。
#### 常见的 `Arrangement` 值

- `Arrangement.Start`: 子组件从左到右排列，并且从左边开始对齐（适用于 `Row`）。
- `Arrangement.End`: 子组件从左到右排列，并且从右边开始对齐（适用于 `Row`）。
- `Arrangement.Center`: 子组件在容器中居中排列。
- `Arrangement.SpaceBetween`: 子组件均匀分布，第一个子组件在开始位置，最后一个子组件在结束位置，其他子组件在中间均匀分布。
- `Arrangement.SpaceAround`: 子组件均匀分布，每个子组件之间的间距相等，且两边有一半的间距。
- `Arrangement.SpaceEvenly`: 子组件均匀分布，所有间距（包括两边）相等。
虽然 `Row` 和 `Column` 没有直接的 `Alignment` 参数，但它们使用 `verticalAlignment`（对于 `Row`）和 `horizontalAlignment`（对于 `Column`）来控制子组件在另一方向的对齐方式。
## Swipeable
Jetpack Compose 官方库提供的用于处理可拖动行为的主要工具是 `SwipeableState` 和相关的 `swipeable` 修饰符。
**SwipeableState**: 用于实现可滑动和可吸附的组件，支持定义锚点（anchors）、拖动阈值（thresholds）和方向（orientation）。适用于滑动解锁、滑动删除等场景。
```kotlin
val swipeableState = rememberSwipeableState(initialValue = 0)
Box(
    modifier = Modifier
        .swipeable(
            state = swipeableState,
            anchors = mapOf(0f to 0, 1f to 1),
            thresholds = { _, _ -> FractionalThreshold(0.5f) },
            orientation = Orientation.Horizontal
        )
) {
    // Content
}
```
### Modifier .swipe方法详细解释
```kotlin
fun Modifier.swipeable(
    state: SwipeableState<T>,
    anchors: Map<Float, T>,
    thresholds: (from: T, to: T) -> ThresholdConfig = FixedThreshold(56.dp),
    orientation: Orientation,
    enabled: Boolean = true,
    reverseDirection: Boolean = false,
    resistance: ResistanceConfig? = null,
    velocityThreshold: Dp = 125.dp,
    interactionSource: MutableInteractionSource = remember { MutableInteractionSource() },
    nestedScrollConnection: NestedScrollConnection = NestedScrollConnection.NoOp
): Modifier

```
#### 参数解释：
1. state: `SwipeableState<T>`
	1. `SwipeableState` 用于管理滑动状态。它包含当前值、偏移量、以及动画的执行。
2. **anchors**: `Map<Float, T>
	1.  一个浮点值到状态值的映射，定义组件可以吸附的具体位置。浮点值表示组件在滑动方向上的绝对位置，状态值表示滑动到该位置时的状态。
3. **thresholds**: `(from: T, to: T) -> ThresholdConfig`
	1. 用于定义滑动状态之间的阈值配置。它是一个函数，接收两个状态值并返回一个 `ThresholdConfig`。默认是一个固定的 56dp 阈值。(不是很懂)
	2. 此参数用于定义滑动到下一个状态所需的拖动距离或条件，它是一个函数，接收当前状态（from）和目标状态（to），并返回 `ThresholdConfig` 对象，定义从当前状态滑动到目标状态的阈值。
4. - **orientation**: `Orientation`
	1. 定义滑动的方向，可以是 `Orientation.Horizontal` 或 `Orientation.Vertical`。
5. **enabled**: `Boolean`
	1. - 指定滑动行为是否启用。默认为 `true`。
6.  **reverseDirection**: `Boolean`
	1. - 如果为 `true`，则滑动方向反转。默认为 `false`。
7.  **resistance**: `ResistanceConfig?`
    - 可选参数，定义滑动到边缘时的阻力配置。
8. **velocityThreshold**: `Dp`
    - 定义滑动吸附的速度阈值。默认值为 125dp。
9.  **interactionSource**: `MutableInteractionSource`
    - 用于控制和监听交互事件的源。
10.  **nestedScrollConnection**: `NestedScrollConnection`
    - 用于嵌套滚动的连接配置。默认是 `NestedScrollConnection.NoOp`
## `.graphicsLayer` 修饰符
`graphicsLayer` 修饰符用于在 Composable 组件上应用低级图形变换。它提供了直接访问 Android 的 `View` 层次结构的图形属性，比如透明度、旋转、缩放和平移等。这个修饰符允许你在 Compose 组件中应用这些变换效果。
```kotlin
Modifier.graphicsLayer {
    translationX = swipeableState.offset.value
    translationY
    alpha
    rotationX
    rotationY
    scaleX
    scaleY
}
```
## `.onSizeChanged`
`onSizeChanged` 是一个修饰符，用于在 Composable 组件的尺寸变化时执行特定操作。它的回调参数 `it` 是一个 `IntSize` 对象，表示组件的宽度和高度。
```kotlin
.onSizeChanged {
    bottomWidth = it.width.toFloat()
}
```
# REF:
1. [写给初学者的Jetpack Compose教程，Modifier-CSDN博客](https://blog.csdn.net/guolin_blog/article/details/132253342)