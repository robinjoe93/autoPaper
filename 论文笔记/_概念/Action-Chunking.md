---
title: Action Chunking
aliases: [动作块, Action Block, Temporal Action Prediction]
tags: [robot-control, action-prediction, temporal-modeling]
created: 2026-04-20
---

# Action Chunking

## 定义

[[Action Chunking]] 是机器人控制中的一种输出策略，模型一次输出多个未来时间步的动作序列，而非仅输出单步动作。这种设计提升时序一致性，减少动作抖动。

## 核心思想

```
传统方法: t → a_t (单步)
Action Chunking: t → a_{t:t+k} (k步动作块)
```

## 优势

1. **时序平滑**: 预规划多步动作，减少相邻帧动作跳变
2. **计算效率**: 减少 PLL (Policy Latency Loop) 问题
3. **任务连贯**: 长视野规划，动作更符合任务逻辑

## 参数设计

| 参数 | 含义 | 典型值 |
|------|------|--------|
| $k$ | 动作块长度 | 8-50 步 |
| 执行策略 | 实际执行多少步 | 1步 (re-plan) 或 k步 |

## 代表工作

| 模型 | Action Chunking 设计 |
|------|----------------------|
| [[ACT]] | CVAE + Temporal Ensemble |
| [[Pi0]] | Flow Matching + Action Sequence |
| [[HY-Embodied-0.5]] VLA | MoT + Action Expert |

## 执行策略

- **Re-plan**: 每步重新预测，执行第一个动作
- **Full execution**: 执行完整动作块后再重新预测
- **Temporal ensemble**: 对多次预测的动作加权平均

## 挑战

1. **长程预测**: 预测远未来动作的准确性下降
2. **动态响应**: 环境变化时需中断动作块重新预测
3. **动作维度**: 高维动作空间的块表示效率

## 相关概念

- [[Vision-Language-Action]]: 使用 Action Chunking 的典型任务范式
- [[ACT]]: Action Chunking with Transformers