---
{"tags":["Project/code","Project/AndroidStudio"],"dg-publish":true,"permalink":"/Project/省中医APP开发/yolox转onnx/","dgPassFrontmatter":true}
---

## 步骤
1. 下载yolox模型：[YOLOX/tools/visualize_assign.py at main · Megvii-BaseDetection/YOLOX (github.com)](https://github.com/Megvii-BaseDetection/YOLOX/blob/main/tools/visualize_assign.py)
2. 进入到tools文件夹中的export_onnx. py文件进行操作。
3.  配置环境：
```python
git clone git@github.com:Megvii-BaseDetection/YOLOX.git
cd YOLOX
pip3 install -U pip && pip3 install -r requirements.txt
pip3 install -v -e .  # or  python3 setup.py develop
git clone https://github.com/NVIDIA/apex
cd apex
pip3 install -v --disable-pip-version-check --no-cache-dir --global-option="--cpp_ext" --global-option="--cuda_ext" ./
```
4. 运行
```cmd  

python tools/export_onnx.py --output-name my_yolox_s.onnx -f exps/example/yolox_voc/yolox_voc_s.py -c yolox.pth

```

## BUG:
1. yolox环境部署出错：[[Project/省中医APP开发/ERROR_Command errored out with exit status报错\|ERROR_Command errored out with exit status报错]]
2. 模型数量出现错误，对yolox_voc中的类别进行改写，因为就一个tongue进行检测。 ![Pasted image 20240415140108.png](/img/user/Project/%E7%9C%81%E4%B8%AD%E5%8C%BBAPP%E5%BC%80%E5%8F%91/%E5%9B%BE%E7%89%87/Pasted%20image%2020240415140108.png)

# REF:
1. [实践教程｜YOLOX目标检测ncnn实现-CSDN博客](https://blog.csdn.net/qq_29462849/article/details/119067484)
2. [实践教程｜YOLOX目标检测ncnn实现-CSDN博客](https://blog.csdn.net/qq_29462849/article/details/119067484)