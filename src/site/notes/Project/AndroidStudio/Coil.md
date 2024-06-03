---
{"tags":["Project/AndroidStudio/Kotlin"],"dg-publish":true,"permalink":"/Project/AndroidStudio/Coil/","dgPassFrontmatter":true}
---

**Coil**（Coroutine Image Loader）是一个用于 Android 的现代图片加载库，专为 Kotlin 和 Jetpack Compose 设计。它利用 Kotlin 协程和现代 Android 架构组件来提供高效、简洁的图片加载功能。

## 基本用法
```kotlin
import coil.ImageLoader 
import coil.request.ImageRequest 
import coil.compose.rememberImagePainter 

// 在 Jetpack Compose 中使用 Coil 加载图片 
Image( painter = rememberImagePainter(data = "https://example.com/image.jpg"),
								  contentDescription = "Example Image" ) 
// 在传统的 View 中使用 Coil 加载图片 
val imageLoader = ImageLoader(context) 
val request = ImageRequest.Builder(context) 
						.data("https://example.com/image.jpg") 
						.target(imageView) 
						.build() imageLoader.enqueue(request)
						
imageLoader.enqueue(request)
```

# REF:
1. 