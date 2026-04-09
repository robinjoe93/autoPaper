---
type: concept
aliases: [OKS, 关键点相似度]
---

# Object Keypoint Similarity

## 定义

衡量检测的关键点与 ground truth 关键点之间相似度的指标，常用于 2D 姿态估计评估，考虑了关键点位置误差和目标尺度。

## 数学形式

$$
\text{OKS} = \frac{\sum_{i} \exp\left(-\frac{d_i^2}{2s^2\kappa_i^2}\right) \delta(v_i > 0)}{\sum_{i} \delta(v_i > 0)}
$$

**符号说明**:
- $d_i$: 检测关键点与 ground truth 的欧氏距离
- $s$: 目标尺度因子（通常是边界框面积的平方根）
- $\kappa_i$: 关键点特定的标准差（控制不同关键点的容忍度）
- $v_i$: 关键点可见性标志（$v_i > 0$ 表示可见）
- $\delta(v_i > 0)$: 只对可见关键点计算

## 核心要点

1. **尺度归一化**: 通过 $s$ 归一化，使不同大小的目标可比较
2. **关键点特异性**: $\kappa_i$ 反映不同关键点的标注难度（如肩膀比手腕更稳定）
3. **范围**: OKS ∈ [0, 1]，1 表示完美匹配
4. **阈值**: 通常 OKS > 0.5 为正确检测，> 0.8 为高质量

## 代表工作

- [[PoseDreamer]]: 使用 OKS 评估生成图像的姿态一致性和过滤低质量样本
- COCO keypoint evaluation metric

## 相关概念

- [[2D Pose Estimation]]
- [[SMPL-X]]
- [[Keypoint Detection]]