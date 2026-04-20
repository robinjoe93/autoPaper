---
title: Vision-Language-Action (VLA)
aliases: [VLA, Vision-Language-Action Model]
tags: [embodied-ai, robot-control, multimodal]
created: 2026-04-20
---

# Vision-Language-Action (VLA)

## 定义

[[Vision-Language-Action]] 是一种将视觉感知、语言理解和机器人动作输出统一建模的多模态模型范式。VLA 模型以视觉观测和语言指令为输入，直接输出机器人控制信号。

## 核心架构

```
视觉观测 → 视觉编码器 → 视觉 Token
                              ↓
语言指令 → LLM → 跨模态融合 → Action Token → 动作输出
```

## 关键设计

- **Action Token**: 将机器人动作（位姿、速度等）编码为 Token 序列
- [[Action Chunking]]: 输出动作块而非单步动作，提升时序一致性
- **模态扩展**: 在 VLM 基础上添加 Action Expert/Head

## 代表工作

| 模型 | 机构 | 特点 |
|------|------|------|
| [[RT-1]] / [[RT-2]] | Google | Transformer + Action Tokenization |
| [[Pi0]] | Physical Intelligence | Flow Matching + VLA |
| [[Pi05]] | Physical Intelligence | 改进版具身基础模型 |
| [[HY-Embodied-0.5]] | Tencent | MoT 架构 + VLA 扩展 |

## 与传统方法的对比

| 方法类型 | 输入 | 输出 | 特点 |
|----------|------|------|------|
| 传统控制 | 状态 | 动作 | 需精确状态估计 |
| VLA | 视觉+语言 | 动作 | 端到端，泛化性强 |
| 分层规划 | 视觉 | 任务 | 需下游控制器 |

## 挑战与趋势

1. **数据稀缺**: 高质量机器人数据获取成本高
2. **多具身泛化**: 不同机器人形态的统一建模
3. **实时推理**: 边缘部署的响应速度要求
4. **安全约束**: 物理世界的安全操作边界

## 相关概念

- [[Embodied-AI]]: 具身智能总览
- [[Action Chunking]]: 动作块输出
- [[Mixture-of-Transformers]]: MoT 可扩展为三模态