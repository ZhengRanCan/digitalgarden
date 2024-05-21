---
{"tags":["Project/省中医APP开发","gardenEntry","gardenEntry","gardenEntry"],"dg-publish":true,"dg-home":"true","permalink":"/Project/省中医APP开发/省中医APP开发/","dgPassFrontmatter":true}
---

# 功能开发
## V1.0：
1. 提供基础相机功能以拍摄病人舌头
	1. [[Project/省中医APP开发/相机开发\|相机开发]]
2. 当相机摄像内容不满足AI模型所需时，需要弹出提示
	1. 光照
	2. 角度
	3. 大小
3. 提供API接口，以供AI模型接入
## 平台：
1. 原生开发
	1. [[Project/AndroidStudio/AndroidStudio入门\|AndroidStudio入门]]
2. 混合开发
数据存储方面：先用SharedPreferences进行存储吧
## v2.0
1. 完善app界面，美化
2. 将yolov8或者其他yolo模型部署到手机上，或者通过rstp，tcp啥的协议做服务端生成，客户端通过API接受。（这一部分主要是用于检测是否有舌头）
	1. 检测舌头
3. 相机要有实时预览功能，预览同时用到检测模型，然后还要对环境光进行判断，对舌头离相机距离进行判断，并实时反馈到app上
	1. https://blog.csdn.net/bluewindtalker/article/details/79999172 （测量环境光）
	2. https://github.com/philiiiiiipp/Android-Screen-to-Face-Distance-Measurement （测量距离）
	3. https://www.bilibili.com/video/BV1du411577U/?spm_id_from=333.337.search-card.all.click&vd_source=bf07b512779fbffcb99bc03d1dae0115 （YOLO部署）
	4. https://zhuanlan.zhihu.com/p/45827914 （人脸识别项目）
## v3.0  2024.4
1. 图片检测，保存图片
	1. 检测标准：
	2. yolov8：检测到对应舌头
	3. 亮度检测：对环境光照进行判断
	4. 舌头大小粗略判断
2. 对图片进行本地处理
	1. 分割舌头
	2. 分析舌象特征
		1. 选取特征项：舌色，苔色，舌形
		2. 通过分离通道结果图分析舌苔轮廓，舌形
		3. 通过处理亮度，颜色分析舌头的舌色

![Pasted image 20240412210332.png](/img/user/Project/%E7%9C%81%E4%B8%AD%E5%8C%BBAPP%E5%BC%80%E5%8F%91/%E5%9B%BE%E7%89%87/Pasted%20image%2020240412210332.png)


## v4.0   2024.4.16
以拍照界面作为起始点开始分析，逐步将功能分模块。
1. 拍照界面：
	1. 检测亮度
	2. 动态检测舌头，需要注意判断舌头的大小，完整性
	3. 假设不能进行动态检测舌头，就先实行相机功能。当相机拍到图片时，对图片检测，如果图片中的舌头满足要求，则将舌头图片进行保存用来后续图片的分析，如果不能满足要求，则进行重新拍照。
2. 图片分析：对拍照界面返回的舌头图片进行处理，以分析出舌象特征
	1. 图片分割，将舌头照片的舌头进行分割
		1. 小昂师兄在做
		2. 通过飞浆平台做
		3. 没听清楚，忘了
	2. 对舌头进行颜色分析
		1. 颜色标准化，规范化
			1. 等医生做一个调查问卷——对舌头图片打标签
			2. 对已打好标签的舌头照片进行训练（半监督学习）
			3. 训练指标：红，白，紫，湿度检测
	3. 对舌头进行厚度检测
		1. 还没相关思路
		2. 硬件？
	4. 对舌头进行亮度检测
		1. 通过舌头的反射程度，进行分析（待补充）
		2. 通过舌头的湿度进行判断
3. 后台接入
	1. 在登入界面中选择角色（医生，病人，开发者）
	2. 医生
		1. 在登录界面需要选择医院
		2. 可以查看自己病人的相关信息，
		3. 同一医院的不同医生可以相互查看病人信息，不同医院的医生只能查看自己医院的病人
		4. 隐私方面：医生在查看病人信息时，需要注意隐藏信息（比如名字郑 * 轩）
	3. 病人
		1. 可以查看自己的问诊记录
		2. 如果在不同医院会诊，应能同步不同医院的问诊记录
	4. 开发者
		1. 可以查看全部信息
4. App整合
	1. zxx和zpm的软件目前是分离的，现在需要尽快将两个app的相关功能合并到一起
	2. 同时，需要设置一套项目规范，以便于后续开发的同步问题。
5. 美观
	1. 听说后面有专业团队，先放着

---
# APP模块

```components
componentId: 0f1f525e-1087-4ffe-b598-3aa053689f8b

```

## APP分为四个模块 ：
1. [[Project/省中医APP开发/APP的初始界面\|APP的初始界面]]
2. [[Project/省中医APP开发/APP的舌诊界面\|APP的舌诊界面]]
3. [[Project/省中医APP开发/APP的拍照界面\|APP的拍照界面]]
4. [[Project/省中医APP开发/APP舌诊报告界面\|APP舌诊报告界面]]

## 其他功能模块或注意点
1. [[Project/省中医APP开发/APP项目规范\|APP项目规范]]
2. [[Project/省中医APP开发/UI/使用Figma转换代码\|使用Figma转换代码]]
3. [[Project/AndroidStudio/数据库数据调用\|数据库数据调用]]


---
# REF :
1. 支付宝人脸认证
	1. [【C语言/C++】一小时学会支付宝人脸识别系统_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1io4y127yC/?spm_id_from=333.337.search-card.all.click&vd_source=ed636aea03b32e53457a090439165487)
2. APP开发
	1. QT？
		1. [利用Qt开发跨平台APP（一）（Android）_安卓ui可以用qt开发-CSDN博客](https://blog.csdn.net/wikichan/article/details/77679783)
	2. [如何从零开始写一个 Android 安卓 App ？ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/442096241)
	3. Android Studio (比较主流？)
		1. [2022 最新 Android 基础教程，从开发入门到项目实战，看它就够了，更新中_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV19U4y1R7zV/?spm_id_from=333.337.search-card.all.click&vd_source=ed636aea03b32e53457a090439165487)
		2. 
