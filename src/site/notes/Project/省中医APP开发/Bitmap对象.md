---
{"tags":["Project/省中医APP开发"],"dg-publish":true,"permalink":"/Project/省中医APP开发/Bitmap对象/","dgPassFrontmatter":true}
---

## 定义：
Android系统用于处理图像的一个类，可用于获取图像信息，图像操作，并按指定格式保存图像文件。
## 格式：
Bitmap是位图，由像素点组成
Bitmap中有两个内部枚举类：`Config` 和 `CompressFormat`, `Config` 是用来设置颜色信息的，`CompressFormat` 是用来设置压缩方式的
### 存储格式
![Pasted image 20240421145136.png](/img/user/Project/%E7%9C%81%E4%B8%AD%E5%8C%BBAPP%E5%BC%80%E5%8F%91/%E5%9B%BE%E7%89%87/Pasted%20image%2020240421145136.png)

## 创建
1. Bitmap的静态方法
2. BitmapFactory的decode系列静态方法
	1. `decodeFile、 decodeResource、decodeStream和decodeByteArray`, 分别用于支持从文件系统、资源、输入流以及字节数组中加载出一个Bitmap对象，其中`decodeFile和decodeResource`又间接调用了`decodeStream`方法，这四类方法最终是在Android的底层实现的，对应着`BitmapFactory`类的几个native 方法。
![Pasted image 20240421145539.png](/img/user/Project/%E7%9C%81%E4%B8%AD%E5%8C%BBAPP%E5%BC%80%E5%8F%91/%E5%9B%BE%E7%89%87/Pasted%20image%2020240421145539.png)


# REF：
1. [Android：安卓学习笔记之Bitmap的简单理解和使用_android bitmap-CSDN博客](https://blog.csdn.net/JMW1407/article/details/122489367)