---
{"tags":["Project/AndroidStudio"],"dg-publish":true,"permalink":"/Project/AndroidStudio/读UI文章有感/","dgPassFrontmatter":true}
---

# 第一篇文章
## 色彩的概念
通过HSV理论：将色彩分解成三种基本的元素：Hue（色相）、Saturation（纯度）、Value（明度）
### 色相 ：
色相是指颜色在色谱中的位置，以红黄蓝作为三原色（这里的三原色是指色彩的三原色，而不是光学上的三原色），在HSV模型中，以红黄蓝作为三原色，通过将这些基本色相互混合形成的颜色构成了色相环。色相环包括了12种基本的色相，如红、橙、黄、绿、青、蓝等。每种颜色在色相环中都有其特定的位置，反映了其在光谱中的位置。![Pasted image 20240510103634.png](/img/user/Project/%E7%9C%81%E4%B8%AD%E5%8C%BBAPP%E5%BC%80%E5%8F%91/UI/%E5%9B%BE%E7%89%87/Pasted%20image%2020240510103634.png)
### 纯度：
可将其理解为在原本的色相中混合进去不同程度黑白两色或者灰度，从而使颜色呈现鲜艳或浑浊的效果
### 明度 ：
色彩从亮到暗的明暗程度

### 关系：
1. 纯度和明度：高纯度色相加入白或黑，会提高或降低明度，但都会降低色相的纯度。
![Pasted image 20240510104118.png](/img/user/Project/%E7%9C%81%E4%B8%AD%E5%8C%BBAPP%E5%BC%80%E5%8F%91/UI/%E5%9B%BE%E7%89%87/Pasted%20image%2020240510104118.png)

## 应用
### 单色
选取色轮上的一个颜色，然后改变纯度和明度，好处是能保证相互匹配
### 相似色
使用色轮中彼此相邻的色彩，大概就是30°左右去过度的样子
### 互补色
色轮中彼此相对的色彩
互补色可以延伸出来分裂互补色
### 三元配色方案
三种均匀分布的颜色，在色轮上形成一个完美的三角形
通过分配主色和辅色，效果挺好
### 四元配色方案
在色轮上形成一个矩形，选择一个颜色作为矩形

### 搭配
1. 如果色彩搭配特别刺眼，选择一种颜色调整它的纯度或者明度
2. 不要过多的使用颜色，中性颜色可以用来平衡颜色

## APP
APP所用的配色方案基本由背景色、主题色、辅助色、点睛色四种色调组成。
1. 背景色：背景色通常是整个应用界面的基础色调。它应该与应用的整体风格和主题相匹配，并提供良好的可读性和视觉舒适度。通常情况下，浅色背景用于传达轻松、明亮的氛围，而深色背景则更适合传达沉稳、专业的感觉。
2. 主题色：主题色是应用中最突出的颜色，也是用户与应用界面互动的重要元素。它可以用于突出重点内容、按钮、链接等。
3. 辅助色：辅助色用于补充主题色，丰富应用界面的细节和交互元素。辅助色可以用于突出次要按钮、分隔线、标签等，以增强用户体验和视觉吸引力。
4. 点睛色：点睛色是用来吸引用户注意力的颜色，通常用于强调特定内容或功能。它可以用于突出关键信息、交互反馈或视觉引导。
5. 前进色和后退色：浅色底上，深色，鲜艳的颜色为前进色，深色底上，浅色，鲜艳的颜色为前进色。
### 凸显APP页面UI元素和文字的多种方法
1. 文字：色相、粗细、透明度、尺寸、下划线
2. UI元素：尺寸、高低、 间距、角度、形状、密度、透明、颜色
### 流行的配色方案
1. 单色渐变，双色渐变![Pasted image 20240510142654.png](/img/user/Project/%E7%9C%81%E4%B8%AD%E5%8C%BBAPP%E5%BC%80%E5%8F%91/UI/%E5%9B%BE%E7%89%87/Pasted%20image%2020240510142654.png)
2. APP类型配色
	1. 电商类APP：红色、粉色暖色为主要配色
	2. 医疗、科技、旅游以绿色、蓝色为主要配色
	3. 音乐类：紫红、红白、绿白
	4. 理财类：橘色、紫蓝、土豪金
	5. 美食类：米黄色、咖啡色、粉红色
# 第二篇文章
第二篇文章前面的颜色搭配与第一篇文章类似，但对于APP的配色方案多了一点介绍

## 主色调
### 界面偏向的颜色 ：
1. 企业VI确定UI界面的主题色
2. 根据用户人群确定UI界面的主题色
3. 行业特征确定主题色
## 主题色
1. 界面比较强调的地方，比如标题，价格，按钮，导航，主色用量基本占据全部用色的25%
## 辅助色
背景色，占据75%的全部用色，起辅助主色的作用，
### 颜色选择：
1. 选择主题色的邻近色作为辅助色，让界面有丰富的变化，产生活泼感
2. 用主题色的对比色作为辅助色可以让主色更为突出
3. 白色或灰色
## 点缀色
全部色彩方案的5%左右，主要起提醒作用，应用子鼠标悬停，选中，强调部分，点缀色与界面整体色调相区别，明度与饱和度较高。
# REF：
1. https://www.uisdc.com/ui-color-matching-skills
2. https://pixso.cn/designskills/color-matching-in-ui-design/#:~:text=UI%20%E8%AE%BE%E8%AE%A1%E4%B8%AD%E7%9A%84%E8%89%B2%E5%BD%A9%E6%90%AD%E9%85%8D%201%201.%20UI%E8%AE%BE%E8%AE%A1%E5%8D%95%E8%89%B2%E6%90%AD%E9%85%8D%202%202.%20%E7%9B%B8%E9%82%BB%E9%A2%9C%E8%89%B2%E6%90%AD%E9%85%8D,5.%20%E4%B8%89%E8%89%B2%E6%90%AD%E9%85%8D%206%206.%20%E5%9B%9B%E5%88%86%E8%89%B2%E6%90%AD%E9%85%8D%207%207.%20%E4%B8%BB%E8%89%B2%E8%B0%83%E3%80%81%E8%BE%85%E5%8A%A9%E8%89%B2%E3%80%81%E7%82%B9%E7%BC%80%E8%89%B2%E6%90%AD%E9%85%8D
3. https://www.uisdc.com/ui-color-design-guideline
4. https://zhuanlan.zhihu.com/p/99065208
5. [至今看过最棒的关于基础色彩理论的视频_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1t7411o79q/?spm_id_from=333.788.recommend_more_video.-1&vd_source=ed636aea03b32e53457a090439165487)
6. https://color.adobe.com/zh/explore （Adobe的官方配色网站）

看到别人的的评论推荐了解一手：
1. [【20集】豆瓣9.4分《啊 设计》，设计里的奇思妙想_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Hk4y157g1/?spm_id_from=333.337.search-card.all.click&vd_source=ed636aea03b32e53457a090439165487)
2. [像乌鸦一样思考第1集-纪录片-全集-高清正版在线观看-bilibili-哔哩哔哩](https://www.bilibili.com/bangumi/play/ep120875)
3. [★【室内设计】BBC室内设计规则 1-6 超清（中文字幕）_哔哩哔哩_bilibili](https://www.bilibili.com/video/av10491421/)
4. [BBC室内设计规则 第一集知识点_哔哩哔哩_bilibili](https://www.bilibili.com/video/av10533719/)
5. [BBC之设计生命第一集-纪录片-完整版视频在线观看-爱奇艺 (iqiyi.com)](https://www.iqiyi.com/v_19rrjyxi8g.html?vfrm=pcw_album_auto)
6. [【记录/人文】设计天赋（中英双字）_哔哩哔哩_bilibili](https://www.bilibili.com/video/av20362507/)
7. https://www.bilibili.com/video/av11288776
8. [【抽象：设计的艺术】双语字幕 全8集_哔哩哔哩_bilibili](https://www.bilibili.com/video/av11019747/)
9. https://www.bilibili.com/video/av15718261
10. [★公开课1《底层逻辑：设计是什么？》【AI字幕】——东海大学姚仁禄_哔哩哔哩_bilibili](https://www.bilibili.com/video/av9815990/)
11. [★公开课2、《设计、品牌、行销》【AI字幕】——东海大学姚仁禄_哔哩哔哩_bilibili](https://www.bilibili.com/video/av9813965/)
12. [★公开课3、《人文、文化、设计思考》【AI字幕】——东海大学姚仁禄_哔哩哔哩_bilibili](https://www.bilibili.com/video/av9813993/)
13. [★公开课4、《设计思考：挑战不可能》【AI字幕】——东海大学姚仁禄_哔哩哔哩_bilibili](https://www.bilibili.com/video/av9814059/)
14. [★公开课5、《美学与创意》【AI字幕】——东海大学姚仁禄_哔哩哔哩_bilibili](https://www.bilibili.com/video/av9860626/)
15. [【室内设计】实用灯光与色彩-简体中文字幕内嵌版_哔哩哔哩_bilibili](https://www.bilibili.com/video/av10276594/)
16. http://www.jianshu.com/p/9de23da6ece5