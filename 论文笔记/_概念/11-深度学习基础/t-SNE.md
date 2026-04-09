---
type: concept
aliases: [t-SNE, t-Distributed Stochastic Neighbor Embedding]
---

# t-SNE

## 定义

非线性降维算法，使用 t 分布衡量高维和低维空间中点对的相似度，优化低维嵌入保持局部邻域结构。

## 数学形式

$$
p_{ij} = \frac{\exp(-\|x_i - x_j\|^2 / 2\sigma_i^2)}{\sum_{k \neq l} \exp(-\|x_k - x_l\|^2 / 2\sigma_k^2)} \quad \text{(高维相似度)}
$$

$$
q_{ij} = \frac{(1 + \|y_i - y_j\|^2)^{-1}}{\sum_{k \neq l} (1 + \|y_k - y_l\|^2)^{-1}} \quad \text{(低维相似度)}
$$

优化目标：

$$
\mathcal{L} = \sum_{i,j} p_{ij} \log \frac{p_{ij}}{q_{ij}} \quad \text{(KL 散度)}
$$

## 核心要点

1. **特点**: 侧重保持局部邻域结构
2. **缺点**: 计算慢，全局结构可能失真
3. **参数**:
   - perplexity: 控制邻域大小
   - learning_rate: 优化步长
4. **应用**: 高维数据可视化、聚类分析

## 代表工作

- [[PoseDreamer]]: 使用 t-SNE 投影 DINO/CLIP 特征，分析生成图像分布

## 相关概念

- [[UMAP]]
- [[DINO]]
- [[CLIP]]