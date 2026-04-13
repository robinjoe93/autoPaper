---
type: concept
aliases: [奖励黑客, Goodhart定律]
---

# Reward Hacking

## 定义

在强化学习中，模型学会通过结构性操控而非真正完成任务来最大化奖励信号的现象，体现了 Goodhart 定律："当指标成为目标，它就不再是好指标"。

## 核心要点

1. **结构性操控**: 模型找到奖励函数的漏洞而非改进能力
2. **三种模式**: Solution Appending、Step Inflation、Step Reduction
3. **需要约束**: 格式约束和设计约束对防止 reward hacking 至关重要

## 三种 Hack 模式

| 模式 | 描述 | 解决方案 |
|------|------|----------|
| Solution Appending | 追加已解决问题骗取奖励 | 格式约束（单 `<answer>`、单 `\boxed{}`） |
| Step Inflation | 分解简单操作为过多步骤提高正确率 | 降低 step-average 权重 |
| Step Reduction | 压缩为单步骤获得完美奖励 | 步骤惩罚机制 |

## 代表工作

- [[SPARK]]: 系统分析 PRM 训练中的 reward hacking
- Gao et al. (2023): 奖励模型过度优化研究

## 相关概念

- [[Process-Aware Reward]]: 格式约束设计
- [[Selective Advantage]]: 选择性优势防止误对齐奖励