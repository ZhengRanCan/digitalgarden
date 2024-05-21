---
{"tags":["Project/AndroidStudio"],"dg-publish":true,"permalink":"/Project/AndroidStudio/Android导入OpenCV模块/","dgPassFrontmatter":true}
---

## 步骤
1. 导入模块
	1. 路径选择 `opencv` 的 `sdk ` 模块
	2. 注意修改一下settings. gradle的模块
![Pasted image 20240417221311.png](/img/user/Project/AndroidStudio/%E5%9B%BE%E7%89%87/Pasted%20image%2020240417221311.png)
1. 在`Project Structure`中添加模块
	1. 选中app，点击添加模块依赖
![Pasted image 20240417221323.png](/img/user/Project/AndroidStudio/%E5%9B%BE%E7%89%87/Pasted%20image%2020240417221323.png)
1. 统一`gradle`的版本参数
	1. 注意opencv的gradle中没有`AndroidId`
![Pasted image 20240417221339.png](/img/user/Project/AndroidStudio/%E5%9B%BE%E7%89%87/Pasted%20image%2020240417221339.png)
1. 将 `sdk/native` 下的libs文件夹移动到 `AndroidStudio` 工程文件的 `app/src/main`，并改名为 `jniLibs`

[[Project/省中医APP开发/使用opencv进行人脸检测\|使用opencv进行人脸检测]]


# REF：
1. [【图文详解】Android Studio（新版本） 配置OpenCV库，解决出现的各种问题 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/666307982)
2. 官网下载地址：[Releases - OpenCV](https://opencv.org/releases/)