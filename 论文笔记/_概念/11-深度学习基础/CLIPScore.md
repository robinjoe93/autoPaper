---
type: concept
aliases: [CLIP Score, 图文相似度分数]
---

# CLIPScore

## 定义

基于 CLIP 模型计算的图文相似度分数，广泛用于 image-text caption 数据质量评估。

## 数学形式

$$
\text{CLIPScore}(I, T) = \cos(\text{CLIP}_I(I), \text{CLIP}_T(T)) \times 100
$$

## 核心要点

1. 仅能处理单图文对，无法评估 interleaved 文档
2. 基于图文语义相似度判断质量
3. 是 LAION 等大规模数据集的核心过滤方法

## 代表工作

- [[LAION]]: CLIPScore 过滤构建 400M/5B 数据集
- [[UniFilter]]: 对比基线，突破其单图文限制

## 相关概念

- [[DFN]]
- [[MLLM]]