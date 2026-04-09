---
type: concept
aliases: [困难样本挖掘, 课程学习, Hard Example Mining]
---

# Hard Sample Mining

## 定义

主动选择模型预测困难或错误样本进行训练的策略，通过优先学习困难样本提高模型性能和数据效率。

## 核心要点

1. **动机**: 简单样本冗余，困难样本信息量大
2. **方法**:
   - 模型反馈：根据模型在验证集上的错误分布采样
   - 课程学习：从简单到困难的渐进训练
   - 损失加权：高损失样本权重更大

3. **两阶段流程**:
   - Stage 1: 随机采样生成基线数据集
   - Stage 2: 根据模型性能优先采样困难类别

## 代表工作

- [[PoseDreamer]]: 课程式困难样本挖掘，优先生成模型表现差的姿态类别

## 相关概念

- [[Curriculum Learning]]
- [[Active Learning]]
- [[Data Augmentation]]