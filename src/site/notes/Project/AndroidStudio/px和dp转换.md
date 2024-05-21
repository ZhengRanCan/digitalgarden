---
{"tags":["Project/AndroidStudio"],"dg-publish":true,"permalink":"/Project/AndroidStudio/px和dp转换/","dgPassFrontmatter":true}
---

常用名词
1. in, pt, px, dpi, dip/dp, sp:
	1. in: inches的缩写，屏幕的物理长度单位，Android手机常用的尺寸有5寸，5.5寸，6寸等，这里的长度是手机对角线的长度
	2. pt：points，一个点等于1/72英寸
	3. px：pixels，抽象的取样，没有实际固定长度. 屏幕分辨率一般描述为“宽向像素数*纵向像素数”
	4. dpi：dots per inch  像素密度 
		5. PPI: pixel per inch 
	5. dp: density-independent pixel密度无关像素
	6. sp: scale-indepentdent pixel 独立比例像素。用作字体的单位
2. 为什么要引用dp？
	1. 为了让设计图能够适配不同像素密度的屏幕。
3. DPI和PPI的区别：
	1. DPI多用于印刷中对图片输出的叫法
	2. PPI多用于图片显示生成的叫法
	3. 理论上可以互相应用


$$dp*\frac{PPI}{160} = px$$	$$PPI=(\frac{\sqrt{长度像素数^2+宽度像素数^2}}{屏幕对教线英寸数})$$


# REF :
1. [终极指导：怎么理解px和dp，看这一篇就够了 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/34831282)