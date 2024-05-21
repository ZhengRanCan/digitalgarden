---
{"tags":["Project/AndroidStudio"],"dg-publish":true,"permalink":"/Project/AndroidStudio/intent/","dgPassFrontmatter":true}
---

# 意义：
起一个中间媒介的作用，在两个activity之间传递信息
# 作用
1. 利用 intent 在两个activity之间进行数据交换
	1. 发送方：
		1. 设置intent
2. 利用intent在两个activity之间进行数据包交换
3. 利用 [[Project/AndroidStudio/Activity Result API\|Activity Result API]] 获取返回数据
4. Intent（）的构造函数也接收一个参数，即action字符串，用于指定要执行的操作，
	1. MediaStore. ACTION_IMAGE_CAPTURE   //创建一个用于启动相机应用程序的Intent对象
	2. 判断是否又能够处理 Intent活动   .resolveActivity (getPackageManager ())
		1. getPackageManager是一个上下文方法，用于访问应用程序包的信息。
# 方法
1. 发送方
	1. 设置intent    Intent i = new Inent( MainActivity. this , 接收方.class);
	2. 发送数据 i.putExtra(name: "adsf", value: "asdf");  通过键值对的方式发送数据
	3. 发送数据包 通过Bundle类实现
		1. 定义Bundle类：Bundle bundle = new Bundle ();
		2. bundle. putstring (“”，“”); 同样是键值对方式
		3. i.putExtra(bundle);  
	4. 最后startActivity (i);
	5. 接收数据
		1. 使用[[Project/AndroidStudio/Activity Result API\|Activity Result API]]

1. 接收方
	1. 接收数据
		1. 设置Intent  Intent i = getIntent();
		2. i.getStringExtra ("key")；z这里的getxxExtra得看之前传的是什么
	2. 接收数据包
		1. 设置Bundle  Bundle xxx = i.getExtras ();
		2. assert xx!=null;
		3. noob. getstring
	3. 发送返回数据
		1. Intent resultIntent =new Intent ();
		2. resultIntent. putExtra (“key”，“value”);
		3. setResult (resultCode, resultIntent);
		4. finish (); 


