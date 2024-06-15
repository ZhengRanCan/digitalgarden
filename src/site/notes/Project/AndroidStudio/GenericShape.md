---
{"tags":["Project/AndroidStudio/Kotlin"],"dg-publish":true,"permalink":"/Project/AndroidStudio/GenericShape/","dgPassFrontmatter":true}
---

`GenericShape` 是一个用于定义任意形状的类。它的构造函数接受一个 lambda 表达式，该表达式用于定义形状的路径。这个 lambda 表达式的签名是 `(Size, LayoutDirection) -> Unit`，其中：
- `Size` 表示当前组件的大小。
- `LayoutDirection` 表示布局的方向（LTR 或 RTL）。
1. **定义形状**：    
    - 使用 `GenericShape` 的构造函数，传递一个 lambda 表达式。在这个 lambda 表达式中，你可以使用 `Path` API 来定义形状的路径。
    - `size` 参数表示当前组件的大小。
    - 使用 `Path` API 的方法（如 `moveTo`、`cubicTo`、`lineTo` 等）来绘制形状的路径。
2. **使用形状**：
    - 将创建好的形状传递给 `Modifier.background`，以应用到 `Box` 上。
    - `background` 方法的第一个参数是颜色，第二个参数是形状。
## Path API
- `moveTo(x: Float, y: Float)`: 将起点移动到指定的坐标。
- `lineTo(x: Float, y: Float)`: 从当前点绘制一条直线到指定的坐标。
- `cubicTo(x1: Float, y1: Float, x2: Float, y2: Float, x3: Float, y3: Float)`: 绘制三次贝塞尔曲线。
- `close()`: 关闭当前路径。
### 贝塞尔曲线
#### 二次
二次贝塞尔曲线由一个起点、一个终点和一个控制点定义。使用 `quadTo` 方法绘制二次贝塞尔曲线：差不多抛物线
```kotlin
quadTo(
	controlX: Float, controlY: Float, 
	endX: Float, endY: Float
)
```
#### 三次
三次贝塞尔曲线由一个起点、一个终点和两个控制点定义。使用 `cubicTo` 方法绘制三次贝塞尔曲线：
```kotlin
cubicTo(
	controlX1: Float, controlY1: Float, 
	controlX2: Float, controlY2: Float, 
	endX: Float, endY: Float
)
```
## 代码示例
```kotlin

import androidx.compose.foundation.background
import androidx.compose.foundation.layout.Box
import androidx.compose.foundation.layout.size
import androidx.compose.foundation.shape.GenericShape
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.graphics.Path
import androidx.compose.ui.unit.dp

@Composable
fun CustomShapeExample() {
    val customShape = GenericShape { size, _ ->
        // 使用 Path API 定义形状
        moveTo(size.width * 0.5f, 0f)
		lineTo(
			x:Float,y:Float
		)
        cubicTo(
            size.width, size.height * 0.9f,
            size.width * 0.5f, size.height,
            size.width * 0.5f, size.height
        )
        cubicTo(
            size.width * 0.1f, size.height,
            0f, size.height * 0.9f,
            0f, size.height * 0.7f
        )
        cubicTo(
            0f, size.height * 0.4f,
            size.width * 0.1f, 0f,
            size.width * 0.5f, 0f
        )
        close()
    }

    Box(
        modifier = Modifier
            .size(200.dp)
            .background(Color.Gray, customShape)
    )
}
```
# REF:
1. 