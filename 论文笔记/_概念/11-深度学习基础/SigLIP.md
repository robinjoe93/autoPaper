---
type: concept
aliases: [Sigmoid Loss for Language Image Pre-training]
---

# SigLIP

## 定义

Google 提出的视觉-语言预训练方法，使用 sigmoid loss 替代 contrastive loss，在 CLIP 架构上取得更好性能。

## 数学形式

$$
\mathcal{L} = -\frac{1}{|P|} \sum_{(i,j) \in P} \log \sigma(z_i \cdot z_j) - \frac{1}{|N|} \sum_{(i,j) \in N} \log \sigma(-z_i \cdot z_j)
$$

## 核心要点

1. Sigmoid loss 避免 contrastive loss 的 batch size 依赖
2. SO-400M 版本在 400M 参数规模下表现优异
3. 常作为 MLLM 的 vision encoder backbone

## 代表工作

- [[UniFilter]]: 使用 SigLIP-SO-400M 作为 vision encoder

## 相关概念

- [[MLLM]]
- [[CLIPScore]]