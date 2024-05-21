---
{"tags":["Project/code","#Project/AndroidStudio"],"dg-publish":true,"permalink":"/Project/省中医APP开发/Pytorch格式 .pt .pth .bin .onnx 详解/","dgPassFrontmatter":true}
---

## 场景
使用Pytorch构建模型并且训练之后，下一步就是将这个模型应用到实际场景或者给别人使用，这时候需要思考，提供哪些模型信息，能让对方复现我们的模型

## 结构
1. 模型代码
	1. 定义模型的结构，例如：模型包括了多少层，每一层包含了多少神经元
	2. 定义训练过程，例如：迭代次数，模型大小
	3. 定义如何加载数据和使用
	4. 如何测试评估模型
2. 模型参数
	1. 提供了模型代码，对方能够复现模型，但是参数需要重新训练，他们希望能够将参数保存下来。
		1. model. state_dict ()  模型每一层可学习的节点参数，例如：权重，残差
		2. optimizer. state_dict () 模型优化器的参数
		3. epoch/batch_size/loss
3. 数据集
4. .md文件

##  `Pytorch` 保存模型的几种格式：
1. `.pt`
2. `.pth`
3. `.bin`
4. `.onnx`
`torch. load ()` 可以读取上面的四种模型格式，只要文件是正确的就行。
![Pasted image 20240413105741.png](/img/user/Project/%E7%9C%81%E4%B8%AD%E5%8C%BBAPP%E5%BC%80%E5%8F%91/%E5%9B%BE%E7%89%87/Pasted%20image%2020240413105741.png)
### .pt .pth格式
一个完整的Pytorch模型文件，包含了如下参数：
1. model_state_dict：模型参数
2. optimizer_state_dict：优化器的状态
3. epoch：当前的训练轮数
4. loss：当前的损失值
5. 保存实例
```python
import torch
import torch.nn as nn

# 定义一个简单的模型
class Net(nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.fc1 = nn.Linear(10, 5)
        self.fc2 = nn.Linear(5, 1)

    def forward(self, x):
        x = self.fc1(x)
        x = self.fc2(x)
        return x

model = Net()

# 保存模型
torch.save({
            'epoch': 10,
            'model_state_dict': model.state_dict(),
            'optimizer_state_dict': optimizer.state_dict(),
            'loss': loss,
            }, PATH)
```
6. 加载代码
```python
import torch
import torch.nn as nn

# 定义同样的模型结构
class Net(nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.fc1 = nn.Linear(10, 5)
        self.fc2 = nn.Linear(5, 1)

    def forward(self, x):
        x = self.fc1(x)
        x = self.fc2(x)
        return x

# 加载模型
model = Net()
optimizer = optim.SGD(model.parameters(), lr=0.001, momentum=0.9)
checkpoint = torch.load(PATH)
model.load_state_dict(checkpoint['model_state_dict']) # 模型参数
optimizer.load_state_dict(checkpoint['optimizer_state_dict']) # 优化器参数
epoch = checkpoint['epoch']
loss = checkpoint['loss']
model.eval()
```

### .bin格式
.bin文件是一个二进制文件，可以保存Pytorch模型的参数和持久化缓存。.bin文件的大小较小，加载速度较快，因此在生产环境中使用较多。
1. 保存模型
```python
import torch
import torch.nn as nn

# 定义一个简单的模型
class Net(nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.fc1 = nn.Linear(10, 5)
        self.fc2 = nn.Linear(5, 1)

    def forward(self, x):
        x = self.fc1(x)
        x = self.fc2(x)
        return x

model = Net()
# 保存参数到.bin文件
torch.save(model.state_dict(), PATH)
```
2. 加载
```python
# 加载.bin文件
model = Net()
model.load_state_dict(torch.load(PATH))
model.eval()
```
### .onnx格式
上述保存的文件可以通过Pytorch提供的torch. onnx. export函数转化为onnx格式,这样可以在其他深度学习框架中使用PyTorch训练的模型。
- [ ] 感觉可能有点问题？
```python
import torch
import torch.onnx

# 将模型保存为.bin文件
model = torch.nn.Linear(3, 1)
torch.save(model.state_dict(), "model.bin")
# torch.save(model.state_dict(), "model.pt")
# torch.save(model.state_dict(), "model.pth")

# 将上面三种文件转化为ONNX格式
model.load_state_dict(torch.load("model.bin"))
# model.load_state_dict(torch.load("model.pt"))
# model.load_state_dict(torch.load("model.pth"))
example_input = torch.randn(1, 3)
torch.onnx.export(model, example_input, "model.onnx", input_names=["input"], output_names=["output"])

# 加载
import onnx
import onnxruntime

# 加载ONNX文件
onnx_model = onnx.load("model.onnx")

# 将ONNX文件转化为ORT格式
ort_session = onnxruntime.InferenceSession("model.onnx")

# 输入数据
input_data = np.random.random(size=(1, 3)).astype(np.float32)

# 运行模型
outputs = ort_session.run(None, {"input": input_data})

# 输出结果
print(outputs)
```




## [[Project/省中医APP开发/pt模型转onnx模型\|pt模型转onnx模型]]
将. pt或者. pth文件转换成为. onnx文件

# REF:
1. [Pytorch格式 .pt .pth .bin .onnx 详解 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/620688513)
2. [Save and Load the Model — PyTorch Tutorials 2.2.2+cu121 documentation](https://pytorch.org/tutorials/beginner/basics/saveloadrun_tutorial.html)