---
type: concept
aliases: [Pickapic preference score]
---

# PickScore

## 定义

基于 Pickapic 数据集训练的人类偏好评分模型，用于评估文本到图像生成结果与人类审美偏好的匹配程度。

## 评分形式

$$
\text{PickScore} \in [0, 100]
$$

越高表示人类偏好匹配度越高。

## 核心要点

1. **偏好学习**: 从人类标注的偏好对学习
2. **审美维度**: 评估图像整体审美质量
3. **模型评分**: 使用神经网络而非规则评估
4. **OOD 评估**: 常在 DrawBench 等外部数据集上评估

## 代表工作

- [[SOAR]]: PickScore 从 22.71 提升至 22.86（无奖励模型）
- Kirstein et al. (2022): Pickapic 数据集和 PickScore 原始论文

## 相关概念

- [[HPS]]
- [[ImageReward]]
- [[Aesthetic Score]]
- [[Human Preference]]