---
{"tags":["Project/AndroidStudio"],"dg-publish":true,"permalink":"/Project/AndroidStudio/Context/","dgPassFrontmatter":true}
---

# ContextCompat
1. 用于执行与上下文（Context）相关的操作，例如权限请求，资源获取等。
2. 常用方法：
	1. checkSelfPermission（）：用于检查应用是否被授予权限
		1. 接受两个参数：
			1. 第一个上下文参数Context，通常是Activity或者Fragment的实例
			2. 第二个参数是要检查的权限，通过Manifest. permission类获取
	2. requestPermission（）：用于请求权限
		1. 接收三个参数
			1. 上下文对象，通常是Activity的实例
			2. 字符串数组，包含需要申请的权限
			3. 请求码requestCode,以便在 `onRequestPermissionsResult()` 方法中进行识别和处理
	3. getColor () 和getDrawable () ：用于获取与上下文相关的的颜色和资源
	4. startActivity () 和registerForActivityResult（）：用于启动新的Activity
