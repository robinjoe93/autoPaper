---
type: concept
aliases: [目标检测, Object Localization]
---

# Object Detection

## 定义

识别图像中所有感兴趣物体的类别和位置的任务，输出边界框和类别标签。

## 数学形式

$$
\mathcal{D}(I) = \{(b_i, c_i, s_i)\}_{i=1}^{N}
$$

其中$b_i$是边界框，$c_i$是类别，$s_i$是置信度。

## 核心要点

1. 兼顾分类和定位两个子任务
2. 常用评估指标：mAP、AP50、AP75
3. 代表模型：YOLO系列、Faster R-CNN

## 代表工作

- [[YOLO]]: 实时目标检测系列
- [[SDQM]]: 针对目标检测的数据质量指标
- [[GroundingDINO]]: 开放集目标检测

## 相关概念

- [[Instance Segmentation]]
- [[Image Classification]]
- [[Bounding Box]]