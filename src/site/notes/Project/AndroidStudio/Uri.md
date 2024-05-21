---
{"tags":["Project/AndroidStudio"],"dg-publish":true,"permalink":"/Project/AndroidStudio/Uri/","dgPassFrontmatter":true}
---

# 定义
1. 用于表示统一资源标识符（Uniform Resource Identifier），通常用于标识和定位文件、内容提供者、网址等资源。在 Android 开发中，Uri类经常用于处理和操作应用中的各种资源。


# 使用
1. 从字符串创建Uri:
```java
Uri uri = Uri.parse();
```
3. 使用Uri. Builder创建Uri: 
```java
Uri uri = new Uri.Builder()
.scheme("https")//设置方案
.authority("www.example.com") //设置主机名
.appendPath("path") //设置路径
.appendQueryParameter("key","value")//设置查询参数
.build()
```
3. 通过intent获取Uri
```java
Uri uri = intent.getData();
```
4. 使用ContentResolver查询内容提供者返回的Uri:
 ```java
 Uri uri = cursor.getUri();
 ```
 