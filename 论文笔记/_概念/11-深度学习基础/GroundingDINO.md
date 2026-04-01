---
type: concept
aliases: [Grounding DINO, GroundingDINO-tiny]
---

# GroundingDINO

## 定义

结合文本提示与图像输入的开放集目标检测模型，能够根据自然语言描述定位图像中的物体。

## 核心要点

1. 使用Transformer架构融合文本和视觉特征
2. 支持zero-shot目标检测
3. tiny版本适合作为特征提取器

## 特征提取应用

在[[SDQM]]中，GroundingDINO-tiny encoder作为最佳特征提取器：
- Cosine Similarity Sum: 7707.24
- Mean Cosine Similarity: 0.9898
- Euclidean Distance Sum: 760.09

## 代表工作

- Kirillov et al. (2023): Segment Anything
- [[SDQM]]: 特征提取器评估

## 相关概念

- [[CLIP]]
- [[Object Detection]]
- [[Vision-Language Model]]
- [[DINO]]