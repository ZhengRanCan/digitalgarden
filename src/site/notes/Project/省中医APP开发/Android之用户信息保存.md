---
{"tags":["Project/AndroidStudio"],"dg-publish":true,"permalink":"/Project/省中医APP开发/Android之用户信息保存/","dgPassFrontmatter":true}
---

# SharedPreferences
## 介绍
AndroidStudio提供了SharedProferences类，这是一个轻量储存类，适用于存储用户登录信息这些小数据。它提供了六种数据类型：`String`，`set`，`int`，`long`，`float`，`boolean`。数据以键值对的形式保存在xml文件中。

## 保存：
```java
 //获取SharedPreferences对象
  SharedPreferences sharedPreferences = getSharedPreferences("user",MODE_PRIVATE);
 //获取Editor对象的引用
  SharedPreferences.Editor editor = sharedPreferences.edit();
 //将获取过来的值放入文件
  editor.putString("name", "lucas");
  editor.putInt("age", 30);
  editor.putBoolean("islogin",true);
  // 提交数据
  editor.commit();
```
从上面保存的例子可以看出sharedPreferences有模式的区别，其中共有四种模式。
![Pasted image 20240505150848.png](/img/user/Project/%E7%9C%81%E4%B8%AD%E5%8C%BBAPP%E5%BC%80%E5%8F%91/%E5%9B%BE%E7%89%87/Pasted%20image%2020240505150848.png)
## 读取：
```java
 SharedPreferences sharedPreferences= getSharedPreferences("user", MODE_PRIVATE);
 String name=sharedPreferences.getString("name","");
 int age = sharedPreferences.getInt("age",0);
 boolean islogin = sharedPreferences.getBoolean("islogin",true);
 Log.i("lucashu","name:"+ name +" age:" + age +" islogin:" + islogin);
```

根据上面的例子，可以看出修改数据的时候创建一个Editor对象，用Editor对象进行修改，有三种形式put, remove, clear。读取的时候则是用sharedPreferences对象

# REF:
1. [Android学习之保存用户登录信息-阿里云开发者社区 (aliyun.com)](https://developer.aliyun.com/article/642423#:~:text=%E6%88%91%E4%BB%AC%E5%8F%AF%E4%BB%A5%E9%80%9A%E8%BF%87SharedProferences%E7%B1%BB%E7%9A%84getSharedPreferences%20%28String,NAME%2C%20int%20MODE%29%E6%96%B9%E6%B3%95%E6%9D%A5%E5%AE%9E%E7%8E%B0%E5%AF%B9%E7%94%A8%E6%88%B7%E7%99%BB%E5%BD%95%E4%BF%A1%E6%81%AF%E7%9A%84%E4%BF%9D%E5%AD%98%EF%BC%8C%E5%A6%82%EF%BC%9A%E7%94%A8%E6%88%B7%E5%90%8D%EF%BC%8C%E5%AF%86%E7%A0%81%EF%BC%8C%EF%BD%83%EF%BD%8F%EF%BD%8F%EF%BD%8B%EF%BD%89%EF%BD%85%E7%AD%89%E3%80%82)
2. [Android SharedPreferences使用详解_android getsharedpreferences-CSDN博客](https://blog.csdn.net/huweiliyi/article/details/105496932)
# DataStore：
`SharedPreferences`目前有替代方案：`DataStore`，~~但是还没有深入研究，因为哥们🤠已经用SQL数据库了~~（回旋镖这就来了）
## 介绍：
1. SharedPreferences的全新版本
### 优势：
1. DataStore基于事务方式处理数据更新。
2. 异步操作，不会阻塞UI线程

### 了解：
1. [[Project/AndroidStudio/数据存储\|数据存储]]
2. [[Project/AndroidStudio/ViewModel\|ViewModel]] 、[[Project/AndroidStudio/LiveData\|LiveData]] 、[[Project/AndroidStudio/kotlin Flow\|kotlin Flow]] 、[[Project/AndroidStudio/协程\|协程]]

## 使用
```kotlin
//构建Preferences DataStore，构建一次就行，不要将上面的代码给写到Activity上

val Context.bookDataStorePf: DataStore<Preferences> by preferencesDataStore(
    // 文件名称
    name = "pf_datastore")
//preferencesDataStore()是一个顶层函数，包含以下几个参数
/*
name:创建的文件名字
corruptionHanler:如果读取数据时出错，就会执行这个函数
produceMigrations：
scope：携程作用域
*/

//存数据
//声明一个实体类BookModel:
data class BookModel(
	var name:String = "",
	var price:Float = 0f,
	var type:Type = Type.ENGLISH
)


const val KEY_BOOK_NAME = "key_book_name"
const val KEY_BOOK_PRICE = "key_book_price"
const val KEY_BOOK_TYPE = "key_book_type"

//Preferences.Key<T>类型
object PreferenceKeys {
    val P_KEY_BOOK_NAME = stringPreferencesKey(KEY_BOOK_NAME)
    val P_KEY_BOOK_PRICE = floatPreferencesKey(KEY_BOOK_PRICE)
    val P_KEY_BOOK_TYPE = stringPreferencesKey(KEY_BOOK_TYPE)
}

/**
 * Preferences DataStore存数据
 */
suspend fun saveBookPf(book: BookModel) {
    context.bookDataStorePf.edit { preferences ->
        preferences[PreferenceKeys.P_KEY_BOOK_NAME] = book.name
        preferences[PreferenceKeys.P_KEY_BOOK_PRICE] = book.price
        preferences[PreferenceKeys.P_KEY_BOOK_TYPE] = book.type.name
    }
}

```
# REF:
1. [【面试黑洞】Android 的键值对存储有没有最优解？_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1FU4y197dL/?spm_id_from=333.337.search-card.all.click&vd_source=ed636aea03b32e53457a090439165487)
2. [Android Jetpack DataStore教程一_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Z44y1g7BB/?spm_id_from=333.788&vd_source=ed636aea03b32e53457a090439165487)
3. [使用 Preferences DataStore (google.cn)](https://developer.android.google.cn/codelabs/android-preferences-datastore?hl=zh-cn#5)