# Paddle-AoA-Captioning

[简体中文](./README_ch.md) | English
   
## 1 Introduction
Paper：《Attention on Attention for Image Captioning》[paper](https://arxiv.org/abs/1908.06954v2)

The attentional mechanism is widely used in the current image description encoder-decoder framework, in which the weighted average of the image encoding vector is generated at each time step to guide the generation process of the decoder.  However, the decoder knows little about the relationship or degree of correlation between correlation vectors and a given attention query, which may cause the decoder to give wrong results.  In this paper, the authors propose "Attention on Attention" (AoA) module, which extends the conventional attentional mechanism to determine the correlation between attentional results and query results.  AoA first uses the results of attention and the current context to generate an "information vector" and an "attention gate", and then multiplies them element by element to add additional attention, finally obtaining the "attention information", which is the expected useful knowledge.  In this paper, AoA is applied to encoder and decoder of image description model, and it is named AoA Network (AoANet). Experiments show that the performance of AoANet is superior to all previously published methods. 

[Online AI Studio](https://aistudio.baidu.com/aistudio/projectdetail/2879640)

[Refer to the project address link](https://github.com/husthuaan/AoANet)

## 2 Accuracy
The code is trained on COCO2014 data set, and the reproducibility accuracy is as follows:

Cross-entropy Training

|Bleu_1|Bleu_2|Bleu_3|Bleu_4|METEOR|ROUGE_L|CIDEr|SPICE|
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | 
|0.778|0.623|0.485|0.377|0.284|0.578|1.187|0.215|

SCST(Self-critical Sequence Training)

|Bleu_1|Bleu_2|Bleu_3|Bleu_4|METEOR|ROUGE_L|CIDEr|SPICE|
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | 
|0.810|0.658|0.511|0.391|0.286|0.589|1.283|0.220|

## 3 Dataset
coco2014 image captions [paper](https://link.springer.com/chapter/10.1007/978-3-319-10602-1_48), “Karpathy” data split [论文](https://arxiv.org/pdf/1412.2306v2.pdf)

[coco2014 Dataset download](https://aistudio.baidu.com/aistudio/datasetdetail/28191)

- dataset size：123287
  - train：113287
  - val：5000
  - test：5000
- label：dataset_coco.json

## 4 Environment
paddlepaddle-gpu==2.1.2  cuda 10.2

opencv-python==4.5.3.56

yacs==0.1.7

yaml==0.2.5

## 5 Quick start

### step1: load data

Load preprocessed data files are all placed under the data/repo

[“Karpathy” data split json](https://aistudio.baidu.com/aistudio/datasetdetail/107078)

Bottom-up original feature file extracted by Faster R-CNN model. [link](https://github.com/ruotianluo/ImageCaptioning.pytorch/blob/master/data/README.md)

Generate cocotalk label.h5 and cocoktalk.json files
```bash
python scripts/prepro_labels.py
```

Generate cocobu_att, pool cocobu_fc, and border cocobu_box 
```bash
python scripts/make_bu_data.py
```

[load the above pretraining data](https://aistudio.baidu.com/aistudio/datasetdetail/107198). cocobu_att is uploaded into cocobu_att_train and cocobu_att_val. After the loading is complete, cocobu_att should be merged into cocobu_att.

**Install dependencies**
```bash
pip install -r requestments.txt
```

### step2: train

The training process is divided into two steps：Cross-entropy Training和SCST(Self-critical Sequence Training)

Cross-entropy Training：

```bash
python3 train.py --cfg configs/aoa.yml  
```

SCST(Self-critical Sequence Training)：

```bash
python3 train.py --cfg configs/aoa_rl.yml
```

The trained model data and logs are placed under the log/repo

### step3: eval

```bash
python eval.py
```

During the test, the program will load the training model data saved in the log/repo.  

[Download the trained model data](https://aistudio.baidu.com/aistudio/datasetdetail/118052), into the log/repo, then perform validation instructions directly.  

## 6 Code structure

### 6.1 structure

```
├─config                        
├─models                          
├─misc                            
├─modules                         
├─data                            
├─scripts                         
│  eval.py                       
│  dataloader.py                  
│  README.md                      
│  requirements.txt               
│  train.py                       
```
### 6.2 Parameter description

Training and evaluation parameters can be set in the config file.

## 7 Model information

For other information about the model, please refer to the following table:

| information | description |
| --- | --- |
| Author | Lieber |
| Date | 2021.12 |
| Framework version | Paddle 2.1.2 |
| Application scenarios | multi-modal |
| Support hardware | GPU、CPU |
| Download link | [best model](https://aistudio.baidu.com/aistudio/datasetdetail/118052)|
|Online operation| [notebook](https://aistudio.baidu.com/aistudio/projectdetail/2879640)|
