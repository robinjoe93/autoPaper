---
title: On-Policy Distillation
aliases: [OPD, 在线策略蒸馏]
tags: [knowledge-distillation, llm-training, policy-learning]
created: 2026-04-20
---

# On-Policy Distillation

## 定义

[[On-Policy-Distillation]] 是一种知识蒸馏方法，学生模型先采样自身响应，教师在相同前缀上提供监督。相比传统离线蒸馏，该方法减少了训练-推理分布差距。

## 核心公式

$$
\mathcal{L}_{\mathrm{OPD}}=\mathbb{E}_{x,\,y\sim\pi_{s}(\cdot\mid x)}\left[\frac{1}{|y|}\sum_{t=1}^{|y|}\mathrm{KL}\!\left(\pi_{t}(\cdot\mid x,y_{<t})\,\|\,\pi_{s}(\cdot\mid x,y_{<t})\right)\right]
$$

## 与离线蒸馏的对比

| 特性 | 离线蒸馏 | 在线策略蒸馏 |
|------|----------|--------------|
| 训练数据 | 教师生成 | 学生采样 |
| 分布匹配 | 仅教师轨迹 | 学生自身状态 |
| 监督时机 | 全序列 | Token 级 |

## 核心优势

1. **分布一致性**: 学生在自身解码状态下学习，而非教师轨迹
2. **错误修正**: 教师监督学生实际出错的状态
3. **推理能力迁移**: 推理过程的能力分布在整条链上，而非仅最终答案

## 在 HY-Embodied-0.5 中的应用

- 大模型 (MoE-A32B) 通过 RL + RFT 获得强大推理能力
- 小模型 (MoT-2B) 通过 OPD 迁移大模型的推理行为
- 目标不仅是压缩，更是保留教师的思维风格

## 适用场景

- 长链推理任务（CoT）
- 能力分布在过程而非结果的任务
- 学生-教师能力差距较大的场景

## 相关概念

- [[Knowledge-Distillation]]: 通用蒸馏方法
- [[Chain-of-Thought]]: 需要过程迁移的推理范式
- [[GRPO]]: OPD 之前的 RL 能力发现阶段