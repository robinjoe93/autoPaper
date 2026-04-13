---
type: concept
aliases: [Task Prompt, 任务提示]
category: 深度学习基础
---

# Task Prompt

## 定义

ORCA 提出的可学习 prompt，用于在扩散模型中注入任务特定的隐式描述。替代传统的文本条件，解决 domain gap 问题。

## 核心要点

1. **可学习**: 与下游任务一起优化
2. **共享**: 在单个任务中所有观测共享相同的 task prompt
3. **隐式**: 不使用显式文本，避免 grounding 错误
4. **数量**: 通常 4 个 tokens ($l_t=4$)

## 数学形式

Task prompt 作为条件：

$$
\mathcal{C}^* = \tau_\theta(p_t; p_v)
$$

**符号说明**:
- $p_t$: learnable task prompt tokens
- $\tau_\theta$: text encoder (CLIP 或 SD 的)
- $p_v$: visual prompt

## 工作原理

- 通过 cross-attention 注入 U-Net
- 隐式学习关注任务关键区域（如机械臂、按钮）
- Cross-attention maps 自动聚焦多个相关对象

## 与文本条件的对比

| 条件类型 | Grounding 质量 | 灵活性 |
|----------|---------------|--------|
| 文本 prompt | 受 domain gap 影响 | 固定文本 |
| Task prompt | 隐式学习准确 grounding | 完全灵活 |

## 代表工作

- [[ORCA]]: 提出 task prompt 作为核心创新
- Exploring Conditions for Diffusion Models in Robotic Control (Shin et al., 2026)

## 相关概念

- [[Visual Prompt]]
- [[Cross-Attention]]
- [[CoOp]]