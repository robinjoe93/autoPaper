---
type: concept
aliases: [VGGHeads, VGG 头部检测]
---

# VGGHeads

## 定义

头部姿态估计模型，从 RGB 图像预测 3D 头部姿态（roll, pitch, yaw）和头部边界框。

## 核心要点

1. **输出**: 3D 头部姿态（欧拉角）+ 2D 头部边界框
2. **架构**: CNN backbone + 姿态回归头
3. **应用**: 头部姿态估计、注意力预测
4. **鲁棒性**: 对遮挡、光照变化有一定鲁棒性

## 代表工作

- [[PoseDreamer]]: 使用 VGGHeads 评估生成图像的头部姿态一致性
- VGGHeads (Kumar et al., 2021)

## 相关概念

- [[Head Pose Estimation]]
- [[SMPL-X]]
- [[Object Keypoint Similarity]]