---
title: "World-Action Model"
category: "架构/世界模型"
created: 2026-04-20
---

# World-Action Model (WAM)

## 定义

World-Action Model (WAM) 是一种将世界模型（视频生成）与动作预测统一建模的范式，通过预测未来视觉动态和对应动作来学习机器人策略。

## 核心思想

- **统一建模**: 将视频生成和动作预测放在同一框架中
- **密集监督**: 视频生成提供时间密集的视觉监督
- **物理先验**: 利用大规模视频数据学习物理动态

## 代表工作

- [[GigaWorld-Policy]]: action-centered 设计，推理时视频生成可选
- [[Motus]]: Mixture-of-Transformer 架构
- [[VideoVLA]]: 多模态 Diffusion Transformer
- [[LingBot-VA]]: 自回归视频-动作建模

## 关键挑战

1. **推理延迟**: 联合预测需要迭代采样
2. **误差累积**: 视频预测错误影响动作质量
3. **数据需求**: 需要大规模具身数据预训练

## 相关概念

- [[World Model]]
- [[Vision-Language-Action Model]]
- [[Diffusion Transformer]]
- [[Action Chunking]]