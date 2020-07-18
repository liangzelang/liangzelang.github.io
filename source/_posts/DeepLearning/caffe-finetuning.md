---
title: Caffe ResNet50微调
date: 2017-08-19 22:04:25
categories:
- 技术
tags:
- 图像处理
- 深度学习
- Caffe
- ResNet
comments: true
---

## 背景介绍
最近在学习Caffe，但是周围没有人交流，很是吃力，故分享之，如果有错误大家一定要指出，也希望对其他人有一定帮助。
ResNet模型是在2015年KaiMingHe提出来的，一举夺得了2015的ILSVRC ImageNet分类、识别、 定位的冠军（太腻害）。这个网络结构的特点就是没有最深只有更深……，在作者的[github](https://github.com/KaimingHe)上，有相应的代码，大家如果感兴趣可以看看。
我介绍一下我的数据，最近参加一个百度的比赛，100类宠物狗的识别。数据集一共18000张左右，但是数据质量一般，大小不定。最开始直接使用数据train from scratch，采用AlexNet错误率63%。我也是深度学习的初学者(水平就是看了一本书，看了一遍代码，跑了一遍例程)，所以决定go deeper，实验室服务器GPU是Tesla T40m可以跑一个稍微大一点的网络，所以选择ResNet50,顾名思义就是只有50层的残差网络。具体的网络结构图[如图所示](http://ethereon.github.io/netscope/#/gist/db945b393d40bfa26006)。使用ImageNet的pretrain模型进行finetune。我之前不太懂，直接拿18000张数据丢到网络中，过拟合严重，原因就是数据对于这种大型网络实在太少。（这其实也是技术交流的重要性，你自己搞一周，高手一句话可能就够了^_^）

## Caffe ResNet50 Finetune
我使用的是KaiMing He大神GitHub中的模型和相关配置
这是作者ResNet的repo，里面有预训练模型、网络定义等. [ResNet](https://github.com/KaimingHe/deep-residual-networks)
其中有一个50层的ResNet caffemodel，一个deploy.prototxt。需要从one drivedown下来，非常慢，也可以在[百度云](https://pan.baidu.com/s/1cgjfuY)下载到caffemodel。
因此需要自己做的工作有：
1. 写一个train_val.prototxt配置文件，从deploy.prototxt改一下就OK
2. 修改train_val.prototxt一些参数
3. 准备数据集，最好转换为LMDB，速度能提高很多

## 具体实现步骤

### train_val.prototxt
直接复制deploy.prototxt,然后修改。
1. 加入数据层
2. 根据自己的任务，修改最后全连接层的输出类别数量，我的为100. `num_output: 100`
3. 在训练阶段把BN层中 'use_global_flag: false' 全部设置为false，测试阶段全部设置为 true，所以这个实现需要分开写，然后用参数'include{ phase: train/test}'指明是啥阶段，我觉得这样有点麻烦，所以我是train.prototxt和test.prototxt分开写的，具体的见[我的github](https://github.com/liangzelang)
4. 修改最后全连接层的**名字、学习率**，因为要重新根据我们自己的任务训练最后的全连接层，故不能使用pretrained model对应的名字，这样这层参数将被随机初始化，重新训练；学习率提高可以较快训练这一层。
这样训练和测试的网络结构文件就搞定了。

### solver.prototxt
这个主要配置一些训练参数，例如学习率什么的，也是根据自己的任务、数据来动态调节的
```
train_net: "examples/fusion/f1/train.prototxt"                                  
test_net: "examples/fusion/f1/test.prototxt"
# iter_size: 2
test_iter: 3000
test_interval: 365
# ture: run from lastest snapshot, false: run from iteration 0
# test_initialization: false
display: 365
base_lr: 0.001
lr_policy: "multistep"
# 5th epoch  0.0001
stepvalue: 2920
# 10th epoch 0.00001
stepvalue: 9125
# 20th epoch 0.000001
stepvalue: 21900
# 40th epoch 0.0000001
stepvalue: 31025
# 80th epoch 0.00000001
# stepvalue: 36640
gamma: 0.1
max_iter: 36500
momentum: 0.9
weight_decay: 0.0001
snapshot: 1000
snapshot_prefix: "examples/fusion/f1/f1"
solver_mode: GPU         
```

### deploy.prototxt
这个是最后使用训练得到的模型来进行分类的网络结构，其实跟test.prototxt差不多，我需要分类的是图片，所以就把test.prototxt的数据层改一下，loss和acc层改一下就可以啦。

### 训练

```
###################                                                             
# author:liangzelang
# resnet50 train shell scripts
####################
#!/usr/bin/env sh
./build/tools/caffe train \
    --solver=examples/fusion/f1/solver.prototxt -weights examples/fusion/f1/resnet50.caffemodel
```

# END
