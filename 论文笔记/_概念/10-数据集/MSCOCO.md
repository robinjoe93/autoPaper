---
type: concept
aliases: [MSCOCO, Microsoft COCO, COCO 数据集]
---

# MSCOCO

## 定义

大规模真实图像数据集，提供 2D 关键点、边界框、分割等多任务标注，是 2D 姿态估计的标准 benchmark。

## 核心要点

1. **标注类型**: 2D 关键点（17 个身体关键点）、边界框、分割
2. **规模**: 约 150K 人物实例
3. **场景**: 多样化真实场景
4. **用途**: 2D 姿态估计训练和评估
5. **局限**: 无 3D 标注，仅 2D

## 代表工作

- [[PoseDreamer]]: 作为真实数据对比基线
- Microsoft COCO: Common Objects in Context (Lin et al., 2014)

## 相关概念

- [[3DPW]]
- [[Object Keypoint Similarity]]
- [[2D Pose Estimation]]