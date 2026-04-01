---
type: concept
aliases: [You Only Look Once, YOLOv11]
---

# YOLO

## 定义

单阶段实时目标检测模型系列，通过单次前向传播同时完成物体定位和分类。

## 核心要点

1. 速度快：实时检测能力
2. 单阶段架构：无需候选区域生成
3. YOLOv11n：nano版本，平衡速度和精度

## 版本演进

- YOLOv1-v8: 持续改进架构
- YOLOv11: 最新版本，在SDQM中用于验证

## SDQM中的应用

使用YOLOv11n：
- 训练10 epochs评估数据质量
- mAP50范围：训练0.31-0.79，评估0.06-0.40
- 作为V-Information计算的基础模型

## 代表工作

- Redmon et al. (2016): YOLOv1
- [[SDQM]]: 验证模型

## 相关概念

- [[Object Detection]]
- [[COCO]]
- [[mAP]]