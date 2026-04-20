---
title: "Action Chunking"
category: "方法/策略学习"
created: 2026-04-20
---

# Action Chunking

## 定义

Action Chunking 是机器人策略学习中的一种输出形式，模型预测一个动作序列（chunk）而非单步动作，提高执行连贯性和效率。

## 核心思想

- **序列预测**: 输出 $a_{t:t+p-1}$ 共 $p$ 步动作
- **平滑执行**: 避免 step-by-step 的不连贯
- **时序建模**: 捕捉动作间的时序依赖

## 常见设置

| 方法 | Chunk Length $p$ |
|------|--------------------|
| [[GigaWorld-Policy]] | 48 |
| [[Pi05]] | 16 |
| [[Motus]] | - |

## 相关概念

- [[Action Prediction]]
- [[Temporal Modeling]]
- [[World-Action Model]]