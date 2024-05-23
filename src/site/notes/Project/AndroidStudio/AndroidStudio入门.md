---
{"tags":["Project/AndroidStudio"],"dg-publish":true,"permalink":"/Project/AndroidStudio/AndroidStudio入门/","dgPassFrontmatter":true}
---

# 基础学习
## 安装
1. [下载android studio (wolai.com)](https://www.wolai.com/dyDs6YzzSJD96QTa648aGJ)
2. 好像联想应用商店也能下载的样子
3. 安装JDK
	1. 【保姆级Android Studio+JDK安装配置教程带字幕】 https://www.bilibili.com/video/BV1S44y1572j/?share_source=copy_web&vd_source=817ae913d5c2a3e04f92694955abe876
	2. AndroidStudio环境部署：[p03-安装JDK_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Su4y1w7hL/?p=3&spm_id_from=pageDriver&vd_source=ed636aea03b32e53457a090439165487)

## 项目基础设置
1. 新建项目
	1. 选择empty views activity
	2. 选择finish之后会出现两个文件：. xml和. java文件
	3. 选择模拟器，点击Device Manager
		1. 选个一个5.0尺寸（有点小？）
		2. 下载一个Android 11.0版本, 后面按需求选择就行了
		3. 启动！
2.  硬件加速？
3. 手机连接（失败）
 ![Pasted image 20240331211817.png](/img/user/Project/AndroidStudio/%E5%9B%BE%E7%89%87/Pasted%20image%2020240331211817.png)

## 工程目录结构认识
1.  工程目录：
	1. app模块
		1. manifests 只有一个xml文件
			1. android:allowBackup
			2. icon: 图标
			3. label：名称
			4. roundicon: 是否圆角
			5. theme：主题
		2. java子目录	
		3. Src文件夹：存放源码的文件夹
			1. res：存放资源的文件夹
				1. drawable：图片
				2. layout: 界面布局
				3. mip：应用程序的代用图标文件，启动图标
				4. value：常量定义文件，  字符串常量string. xml、像素常量dimens. xml、颜色常量colors. xml、样式风格定义styles. xml
	2. Gradle Script：工程的编译配置文件，自动化构建工具，依赖、打包、部署、发布
		1. bulid. gradle
			1. 工程
			2. 模块
		2. proguard-rules. pro：描述java的混淆规则？保护应用安全
		3. gradle. properties  配置命令行参数，一般不需要更改
		4. setting. gradle    所需编译的模块
		5. local. properties  项目的本地配置文件

## Git
1.  [[Project/AndroidStudio/git使用\|git使用]]
2.  [版本控制基础知识 |Android 工作室 |Android 开发人员 (google.cn)](https://developer.android.google.cn/studio/projects/version-control)（官网） 

---
# JAVA和View
## JAVA入门
1. [[Project/AndroidStudio/JAVA入门\|JAVA入门]]
## 前后端交互
1. activity_main文件 构建前端页面
	1. 组件通过设置ID可被后端容易找到。
2. MainActivity文件: 用于数据交互
	1. 前端界面组件通过setContentView（R.layout.activity_main）一个一个被识别成view，如果想找到某个view，可通过findViewByID（）;（当组件多了，挺麻烦的）
3. Activity: 一个应用程序组件，提供屏幕用于交互
## 创建新的APP页面
1. 第一种方法：
	1. layout目录下创建xml文件
		1. Textview直接使用text会出现： [[Project/AndroidStudio/硬编码警告\|硬编码警告]]
		2. constrain问题：
	2.  创建于xml文件对应的Java代码
	3. 在AndroidManifest.xml中注册页面配置
2. 第二种方法：
	1. 直接在java文件夹下添加一个new activity就行了
## 视图
1. 宽高的取值有三种：
	1. match_parent：与上级视图保持一定
	2. wrap_content：与内容自适应
		1. 为wrap_content时才能调用代码
			1. 调用控件对象的getLayoutParams方法，获取该控件的布局参数
			2. 布局参数的width表示宽度，heigh属性表示高度
			3. 调用控件对象的setlayoutParams方法，填入修改后的布局参数
	3. 以dp为单位的具体尺寸 ([[Project/AndroidStudio/px和dp转换\|px和dp转换]])
2. 内间距和外间距
	1. margin：当前视图与周围平级视图之间的距离
	2. padding：当前视图与内部下级视图之间的距离
3. 对齐方式
	1. layout_gravity: 当前视图相对于上级视图的对齐方式 子对父
	2. gravity：下级视图相对于当前视图的对齐方式  父对子
	3. 取值：left，top  left|top


## 布局
1. LinearLayout
	1. orientation
		1. vertical
		2. horizontal
		3. 默认水平
	2. Layout_weight 在线性布局的直接下级视图设置
		1. layout_width填入0dp时，这时候layout_weight才能使用，表示水平方向的宽度比例
2. RelativeLayout
	1. 位置依靠上级视图或者平级视图来决定
	2. 默认在上级视图的左上角
	3. 关键词
		1. toLeftOF: 指定视图的左边
		2. toRightOf：指定视图的右边
		3. above：指定视图的上边
		4. below：指定视图的下面
		5. align: 当前视图与指定视图的某个方向对齐
		6. alignParent：当前视图与上级视图的某个方向对齐
		7. 可重复设置
		8. 在指定视图时，引用对方的ID
3. GridLayout：多行多列
	1. columnCount属性，网格列数
	2. rowCount属性，网格行数
	3. 权重：
		1. ColumnWeight
		2. RowWeight
4. ScrollView：
	1. ScrollView：垂直方向
		1. layout_width属性值设置为match_parent
		2. layout_height属性值设置为wrap_content
	2. HorizontalScrollView：水平方向
		1. 与上面类似



## Activity数据交互
1. 利用[[Project/AndroidStudio/intent\|intent]]在两个activity之间进行数据交换
2. 利用[[Project/AndroidStudio/intent\|intent]]在两个activity之间进行数据包交换
3. 利用[[Project/AndroidStudio/intent\|intent]]获取返回数据

## 调试
 [[Project/AndroidStudio/日志打印\|日志打印]]
## 注解
[[Project/AndroidStudio/注解\|注解]]

---
# Kotlin
## 入门
1. [Kotlin 教程 | 菜鸟教程 (runoob.com)](https://www.runoob.com/kotlin/kotlin-tutorial.html)
2. [[Project/AndroidStudio/Kotlin代码学习\|Kotlin代码学习]]（自己写的）

## 注解
[[Project/AndroidStudio/注解\|注解]]
---

# UI设计
1. [[Project/AndroidStudio/读UI文章有感\|读UI文章有感]]
# REF:
1. [02-Android 发展历程_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV19U4y1R7zV/?p=2&spm_id_from=pageDriver&vd_source=ed636aea03b32e53457a090439165487) (入门)
