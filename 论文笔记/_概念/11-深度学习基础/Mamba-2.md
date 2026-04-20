---
title: "Mamba-2"
created: 2026-04-20
tags: [architecture, sequence-modeling, state-space-model]
category: 11-深度学习基础
---

# Mamba-2

## 定义

[[Mamba-2]] 是一种高效的序列建模架构，基于状态空间模型（SSM），具有线性计算复杂度和优秀的长序列建模能力。

## 核心特点

- **线性复杂度**: 相比Transformer的二次复杂度，Mamba具有线性复杂度
- **选择性状态空间**: 可选择性保留或遗忘信息
- **硬件友好**: 针对GPU优化的并行扫描算法
- **长序列建模**: 高效处理长序列数据

## 与Transformer对比

| 特性 | Transformer | Mamba-2 |
|------|-------------|---------|
| 序列长度复杂度 | $O(n^2)$ | $O(n)$ |
| 并行性 | 高 | 高（扫描算法） |
| 长序列能力 | 受限 | 优秀 |
| 内存效率 | 需KV Cache | 更高效 |

## 在Lyra中的应用

[[Lyra]] 使用Mamba-2处理：
- **多视角特征融合**: 融合来自多个视角的特征
- **序列建模**: 处理相机轨迹对应的特征序列
- **动态场景**: 处理时序动态信息

消融实验表明去除Mamba-2后：
- PSNR: 24.77 → 24.58（轻微下降）
- 仍能运行，但性能有所损失

## 数学表示

状态空间模型：
$$
\begin{aligned}
\mathbf{h}_t &= \mathbf{A}\mathbf{h}_{t-1} + \mathbf{B}\mathbf{x}_t \\
\mathbf{y}_t &= \mathbf{C}\mathbf{h}_t
\end{aligned}
$$

选择性机制使得 $\mathbf{A}, \mathbf{B}, \mathbf{C}$ 可根据输入动态调整

## 优势

1. **高效推理**: 线性复杂度，推理速度快
2. **长序列处理**: 不受序列长度限制
3. **内存友好**: 无需存储完整KV Cache
4. **质量保证**: 在多种任务上达到Transformer水平

## 相关概念

- [[Transformer]]: 传统的序列建模架构
- [[State Space Model]]: Mamba的基础理论
- [[Lyra]]: 使用Mamba-2的具体应用
- [[Multi-view Fusion]]: Mamba-2在Lyra中的应用场景

## 参考文献

Gu & Dao, "Mamba: Linear-Time Sequence Modeling with Selective State Spaces" (2023)
Dao & Gu, "Transformers are SSMs: Generalized Models and Efficient Algorithms" (2024)