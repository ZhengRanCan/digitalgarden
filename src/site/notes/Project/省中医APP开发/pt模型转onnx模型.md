---
{"tags":["Project/code","Project/AndroidStudio/Bug"],"dg-publish":true,"permalink":"/Project/省中医APP开发/pt模型转onnx模型/","dgPassFrontmatter":true}
---

## 将 . pt文件转化成onnx文件格式
```python
import torch  

# 加载模型  
model = torch.load('yolox.pt')  
# 创建输入张量  
input_shape = (1, 3, 224, 224)  
input_tensor = torch.randn(input_shape)  
  
# 导出onnx模型  
output_path = 'yolo.onnx'  
torch.onnx.export(model, input_tensor, output_path)
```
1.  [[Project/省中医APP开发/yolox转onnx\|yolox转onnx]]
### 出现错误
![Pasted image 20240413145353.png](/img/user/Project/%E7%9C%81%E4%B8%AD%E5%8C%BBAPP%E5%BC%80%E5%8F%91/%E5%9B%BE%E7%89%87/Pasted%20image%2020240413145353.png)
导出的model变量并不是一个Pytorch模型实例，而是一个OrderedDict对象，这种情况出现在将模型权重文件直接当做模型对象来使用。
这可能是在保存模型的使用 `torch. save (model. state_dict (), model_path)`, 这样保存的是参数文件

```python
import torch
import torch.onnx
from your_model_definition import YourModel  # 这里需要您导入模型的定义

# 加载模型权重
model_weights = torch.load('path_to_your_model_weights.pt')
model = YourModel()  # 创建模型实例
model.load_state_dict(model_weights)  # 加载权重
# 如果在一个类中将模型写好了，直接
model = YourModel().net  #如果有错误，可能需要修改这个类网络对应函数

# 准备模型的输入数据
input_tensor = torch.randn(1, 3, 224, 224)  # 根据您模型的输入调整大小

# 设置输出的 ONNX 文件路径
output_path = 'yolo.onnx'

# 导出模型
torch.onnx.export(model, input_tensor, output_path)

```
![Pasted image 20240413155651.png](/img/user/Project/%E7%9C%81%E4%B8%AD%E5%8C%BBAPP%E5%BC%80%E5%8F%91/%E5%9B%BE%E7%89%87/Pasted%20image%2020240413155651.png)




