---
type: concept
aliases: [Cross-Attention, 交叉注意力]
category: 深度学习基础
---

# Cross-Attention

## 定义

Transformer 中的注意力机制，用于将一个序列的信息注入另一个序列。在扩散模型中用于将条件信息（如文本）注入图像生成过程。

## 数学形式

Cross-Attention 计算：

$$
\text{CrossAttn}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right) V
$$

**符号说明**:
- $Q$: 来自图像特征的 query
- $K, V$: 来自条件的 key 和 value
- $d_k$: key 的维度

## 在扩散模型中的应用

Stable Diffusion U-Net 中的 cross-attention：
- Query: 来自图像 latent 特征
- Key/Value: 来自 CLIP text encoder 的文本嵌入
- 位置: 每个 U-Net block 中的 Transformer 层

## 可视化与分析

- **Attention Maps**: 可可视化每个 token 对应的空间关注区域
- **Grounding 质量**: 用于分析文本条件是否正确关联图像区域
- [[ORCA]] 使用 cross-attention maps 分析文本条件的有效性

## 代表工作

- [[ORCA]]: 分析 cross-attention maps 发现文本条件在控制任务中 grounding 不准确
- [[Stable Diffusion]]: 使用 cross-attention 注入文本条件
- Attention is All You Need (Vaswani et al., 2017)

## 相关概念

- [[Transformer]]
- [[Self-Attention]]
- [[Stable Diffusion]]
- [[Task Prompt]]