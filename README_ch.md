# Paddle-AoA-Captioning

[English](./README.md) | 简体中文

## 一、简介
参考论文：《Attention on Attention for Image Captioning》[论文链接](https://arxiv.org/abs/1908.06954v2)

注意力机制在当前的图像描述编解器框架中广泛使用，其中在每个时间步生成图像编码向量的加权平均值以指导解码器的生成过程。 
但是，解码器几乎不了解相关向量和给定注意力查询之间的关系或关联程度如何，这可能会使解码器给出错误的结果。
在本论文中，作者提出了一个“Attention on Attention”(AoA)模块，该模块扩展了常规的注意力机制，以确定注意力结果和查询结果的相关性。
AoA首先使用注意力的结果和当前的上下文生成一个“信息向量”和一个“注意力门”，然后通过对它们进行逐元素乘法来增加额外的注意力，最终获得“关注信息”，即预期的有用知识。
论文将AoA应用于图像描述模型的编码器和解码器中，将其命名为AoA Network（AoANet）。实验表明，AoANet的性能优于以前发布的所有方法。

[在线 AI Studio项目地址](https://aistudio.baidu.com/aistudio/projectdetail/2879640)

[参考项目地址](https://github.com/husthuaan/AoANet)

## 二、复现精度
代码在coco2014数据集上训练，复现精度：

Cross-entropy Training

|Bleu_1|Bleu_2|Bleu_3|Bleu_4|METEOR|ROUGE_L|CIDEr|SPICE|
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | 
|0.778|0.623|0.485|0.377|0.284|0.578|1.187|0.215|

SCST(Self-critical Sequence Training)

|Bleu_1|Bleu_2|Bleu_3|Bleu_4|METEOR|ROUGE_L|CIDEr|SPICE|
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | 
|0.810|0.658|0.511|0.391|0.286|0.589|1.283|0.220|

## 三、数据集
coco2014 image captions [论文](https://link.springer.com/chapter/10.1007/978-3-319-10602-1_48), 采用“Karpathy” data split [论文](https://arxiv.org/pdf/1412.2306v2.pdf)

[coco2014数据集下载](https://aistudio.baidu.com/aistudio/datasetdetail/28191)

- 数据集总大小：123287张
  - 训练集：113287张
  - 验证集：5000张
  - 测试集：5000张
- 标签文件：dataset_coco.json

## 四、环境依赖
paddlepaddle-gpu==2.1.2  cuda 10.2

opencv-python==4.5.3.56

yacs==0.1.7

yaml==0.2.5

## 五、快速开始

### step1: 加载数据
加载预处理数据文件全放在本repo的data/下 

[“Karpathy” data split json](https://aistudio.baidu.com/aistudio/datasetdetail/107078)

通过Faster R-CNN模型提取的Bottom-up 原始特征文件[链接](https://github.com/ruotianluo/ImageCaptioning.pytorch/blob/master/data/README.md)

生成cocotalk_label.h5和cocoktalk.json文件
```bash
python scripts/prepro_labels.py
```

生成cocobu_att、池化特征cocobu_fc、边框特征cocobu_box
```bash
python scripts/make_bu_data.py
```

可以直接[加载上述预训练数据](https://aistudio.baidu.com/aistudio/datasetdetail/107198)
其中cocobu_att分成cocobu_att_train和cocobu_att_val上传，加载完成后，要合并成cocobu_att

**Install dependencies**
```bash
pip install -r requestments.txt
```

### step2: 训练
训练过程过程分为两步：Cross-entropy Training和SCST(Self-critical Sequence Training)

第一步Cross-entropy Training：

```bash
python3 train.py --cfg configs/aoa.yml  
```

第二步SCST(Self-critical Sequence Training)：

```bash
python3 train.py --cfg configs/aoa_rl.yml
```

训练的模型数据和日志会放在本repo的log/下

### step3: 验证评估

验证模型
```bash
python eval.py
```

测试时程序会加载本repo的log/下保存的训练模型数据，我们最终验证评估的是使用SCST优化的模型。

可以[下载训练好的模型数据](https://aistudio.baidu.com/aistudio/datasetdetail/118052), 放到本repo的log/下，然后直接执行验证指令。

## 六、代码结构与参数说明

### 6.1 代码结构

```
├─config                          # 配置
├─models                          # 模型
├─misc                            # 工具以及测试代码
├─modules                         # 损失函数模块
├─data                            # 训练数据目录
├─scripts                         # 预处理文件
│  eval.py                        # 评估
│  dataloader.py                  # 加载器
│  README.md                      # readme
│  requirements.txt               # 依赖
│  train.py                       # 训练
```
### 6.2 参数说明

可以在config文件中设置训练与评估相关参数

## 七、模型信息

关于模型的其他信息，可以参考下表：

| 信息 | 说明 |
| --- | --- |
| 发布者 | Lieber |
| 时间 | 2021.12 |
| 框架版本 | Paddle 2.1.2 |
| 应用场景 | 多模态 |
| 支持硬件 | GPU、CPU |
| 下载链接 | [训练好的模型](https://aistudio.baidu.com/aistudio/datasetdetail/118052)|
| 在线运行 | [notebook](https://aistudio.baidu.com/aistudio/projectdetail/2879640)|
