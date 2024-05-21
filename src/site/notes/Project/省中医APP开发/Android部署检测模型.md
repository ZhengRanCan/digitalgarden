---
{"tags":["Project/省中医APP开发"],"dg-publish":true,"permalink":"/Project/省中医APP开发/Android部署检测模型/","dgPassFrontmatter":true}
---

# YOLO模型
## 应用到Android端
1. [yolov8部署Android安卓ncnn 全流程 一镜到底，一定行_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1du411577U/?spm_id_from=333.337.search-card.all.click&vd_source=bf07b512779fbffcb99bc03d1dae0115) （粗略部署）
2. 部署自己的模型
	1. 拿一个预训练模型出来训练，训练完之后需要将模型进行转换
		1. 将pt模型用代码转化成onnx，再通过网站[一键转换 Caffe, ONNX, TensorFlow 到 NCNN, MNN, Tengine (convertmodel.com)](https://convertmodel.com/) 转换成ncnn。
		2. 出现[[Project/省中医APP开发/pt模型转onnx模型\|pt模型转onnx模型]]问题,
		3. 出现 [[Project/省中医APP开发/onnx转ncnn模型\|onnx转ncnn模型]] 问题
		4. 检测模型检测不到
	2. 在Android里进去对应的修改
		1. 布局文件，yolov8ncnn, yolo共三个文件需要进行修改。
			1. 布局文件：修改strings.xml中的第一个spiner
			2. 修改yolo. cpp：修改143行的num_class
			3. 
			4. 连接的JNI文件（yolov8ncnn. cpp）: 
				1. 修改modeltypes
				2. 修改连接的函数
	3. 


