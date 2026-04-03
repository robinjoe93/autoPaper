---
title: LoRA
tags: [fine-tuning, parameter-efficient, llm]
created: 2026-04-03
---

# LoRA

## 定义

LoRA (Low-Rank Adaptation) 是一种参数高效的微调方法，通过低秩分解减少可训练参数量。

## 核心公式

权重更新矩阵 $\Delta W$ 分解为：

$$
\Delta W = BA
$$

其中 $B \in \mathbb{R}^{d \times r}$, $A \in \mathbb{R}^{r \times k}$, $r \ll d, k$

## 优势

- **参数效率**: 可训练参数量大幅减少（可达 0.1%）
- **存储效率**: 多个 LoRA 适配器可共享基础模型
- **无推理延迟**: 适配器权重可合并到基础模型

## 在 Simula 中的应用

[[Simula]] 下游实验使用 LoRA 微调：
- 学生模型：Gemma 2 2B IT
- LoRA Rank: 8-16
- 在合成数据上微调

## 代表工作

- [[Simula]]: LoRA 微调验证合成数据有效性

## 相关概念

- [[Simula]]
- [[Parameter-Efficient Fine-Tuning]]