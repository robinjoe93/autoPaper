---
type: concept
aliases: [Vision-Language Model, 视觉语言模型, 多模态模型]
---

# VLM

## 定义
Vision-Language Model (VLM) 是能够同时理解和处理视觉（图像）与语言（文本）信息的神经网络模型，实现跨模态理解和生成。

## 数学形式

$$
p_\theta(C \mid I, Q) = p_\theta(C \mid \text{Enc}_V(I), \text{Enc}_T(Q))
$$

条件生成：以视觉编码和文本编码为条件生成输出。

$$
\mathcal{L} = -\sum_{t} \log p_\theta(y_t \mid I, Q, y_{<t})
$$

## 核心要点
1. **视觉编码器**: 通常使用预训练的视觉模型（如 ViT、CLIP）
2. **语言模型**: 基于 Transformer 的语言模型作为主体
3. **跨模态融合**: 通过 cross-attention 或其他机制融合视觉和文本信息
4. **统一任务**: 单一模型可处理 VQA、caption、检索等多种任务

## 代表工作
- [[CLIP]]: 对比学习实现视觉-文本对齐
- [[HoneyBee]]: VLM 推理数据构建
- InternVL: 高性能 VLM 系列

## 相关概念
- [[CLIP]]: 视觉-文本对齐基础
- [[Cross-Attention]]: 跨模态融合机制
- [[Chain-of-Thought]]: VLM 推理能力
- [[Supervised Finetuning]]: VLM 训练方法