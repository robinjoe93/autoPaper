---
type: concept
aliases: [DINO, Self-supervised Vision Transformer]
---

# DINO

## 定义

Meta 提出的自监督视觉 Transformer，通过自蒸馏学习强大的视觉特征，无需标注数据。

## 数学形式

自蒸馏损失：

$$
\mathcal{L} = -\sum_{i} p_s^{(i)} \log p_t^{(i)}
$$

**符号说明**:
- $p_s$: 学生网络的 softmax 输出
- $p_t$: 教师网络的 softmax 输出（ sharpened + centering）

## 核心要点

1. **架构**: ViT backbone + 自蒸馏训练
2. **特点**: 无需标签，学习语义特征
3. **能力**: 强语义特征，适合图像检索、聚类
4. **应用**: 图像质量评估、分布分析、特征提取

## 代表工作

- [[PoseDreamer]]: 使用 DINO 特征分析生成图像与真实图像的分布相似度
- Emerging Properties in Self-Supervised Vision Transformers (Caron et al., 2021)

## 相关概念

- [[CLIP]]
- [[UMAP]]
- [[ViT]]