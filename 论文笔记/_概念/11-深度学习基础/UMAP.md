---
type: concept
aliases: [UMAP, Uniform Manifold Approximation and Projection]
---

# UMAP

## 定义

非线性降维算法，基于拓扑数据分析，将高维数据投影到低维空间保持局部和全局结构。

## 数学形式

$$
\text{UMAP}: X \subset \mathbb{R}^n \rightarrow Y \subset \mathbb{R}^d, \quad d < n
$$

**核心步骤**:
1. 构建模糊拓扑表示（fuzzy simplicial set）
2. 优化低维嵌入匹配高维拓扑结构

## 核心要点

1. **优点**: 比 t-SNE 更快，保持全局结构更好
2. **参数**:
   - n_neighbors: 控制局部邻域大小
   - min_dist: 控制嵌入点的最小距离
3. **应用**: 可视化、聚类、特征分析
4. **对比**: t-SNE 侧重局部结构，UMAP 平衡全局和局部

## 代表工作

- [[PoseDreamer]]: 使用 UMAP 投影 DINO/CLIP 特征，分析生成图像分布

## 相关概念

- [[t-SNE]]
- [[DINO]]
- [[CLIP]]