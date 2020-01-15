# 基于Yolov3的无瞄点的One-Stage目标检测算法

## 1、目标

- 抛弃特征金字塔思想，在多个特征图上做预测
- 抛弃基于Anchor的目标检测算法
- 抛弃Two-Stage目标检测流程，只做One-Stage目标检测算法

## 2、技术难点

- 解决物体遮挡难题
- 提升对小物体定位的准确度

## 3、传统做法

1、基于Anchor方法，在边界框位置回归时，学习边界框和Anchor的一个函数映射，从而能够快速使边界框收敛。

2、在一个位置提出多个Anchor，那么就可以很好解决物体位置重复现象。

3、基于特征金子塔思想，将物体基于大小在不同的层上做预测，降低每一层学习的复杂度，同时能够有效解决小目标物体识别难题。

## 4、本文做法

抛弃Anchor Based目标检测方法，以及抛弃特征金子塔思想，回归原始网络，在特征提取网络产生的特征图上，直接进行物体类别预测和边界框回归。

method one：通过特征提取特征图和图像原题之间的位置映射关系，首先求得原图中出现的物体真实框在特征图上的位置映射，然后在这些映射的特征图上的每一个位置来进行物体的类别预测和边界框回归。

为了解决物体遮挡的难题，提出当两物体边界框有重叠时，基于视觉的从左到右原则，将重叠区域划分到距离右侧边界框最近的物体类别中。

method one：考虑有效感受野问题，在特征提取网络中，越靠近物体中心点的特征对物体预测的影响越重要，因此，在构建真实特征图标注时，物体的有效感受野框和其真实框通过一个非线性映射函数来生成。

method three：Yolo 特征金子塔Anchor Base的分支和Anchor Free分支结合同时做预测

## 5、实验结果

###### 在Pascal Voc2017数据集中，训练集和验证集打乱放在一起训练，并从中重新随机抽取1/5的数据量作为验证集，来对比实验效果

1、Yolov3架构，Anchor Free分支在3个特征图上都做预测，没有考虑有效感受野问题

`best_model_Epoch_29_step_48179_mAP_0.4998_loss_2114.4957_lr_3e-05`

2、Yolov3架构，Anchor Free分支在3个特征图上都做预测，考虑有效感受野问题

`best_model_Epoch_45_step_73875_mAP_0.5588_loss_735.3984_lr_0.0001`

3、Yolov3架构，Anchor Free分支+Yolov3分支在三个特征图上都做预测，考虑有效感受野问题

`best_model_Epoch_64_step_104389_mAP_0.5619_loss_838.2706_lr_0.0001`

4、Yolov3架构，AnchorFree分支仅在Yolov3的[26*26]的特征图上做预测，考虑有效感受野问题

`best_model_Epoch_26_step_43361_mAP_0.6013_loss_24.5113_lr_3e-05`

###### MS-COCO2017数据集，设置batch-szie为8，训练集为MS-COCO train2017,验证集为 val2017，正在训练中

------



### Credits:

主要借鉴代码来源:

[YunYang1994/tensorflow-yolov3](https://github.com/YunYang1994/tensorflow-yolov3)

[YOLOv3_TensorFlow](https://github.com/wizyoung/YOLOv3_TensorFlow)