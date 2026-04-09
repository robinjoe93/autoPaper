---
type: concept
aliases: [CLIP, Contrastive Language-Image Pre-training]
---

# CLIP

## 定义

OpenAI 提出的视觉-语言预训练模型，通过图像-文本对比学习实现零样本分类和跨模态理解。

## 数学形式

对比学习目标：

$$
\mathcal{L} = -\frac{1}{N} \sum_{i=1}^{N} \left[ \log \frac{\exp(\text{sim}(I_i, T_i) / \tau)}{\sum_{j=1}^{N} \exp(\text{sim}(I_i, T_j) / \tau)} + \log \frac{\exp(\text{sim}(T_i, I_i) / \tau)}{\sum_{j=1}^{N} \exp(\text{sim}(T_i, I_j) / \tau)} \right]
$$

**符号说明**:
- $\text{sim}(I, T)$: 图像和文本特征的余弦相似度
- $\tau$: 温度参数
- $N$: batch size

## 核心要点

1. **架构**: 图像编码器（ViT/ResNet）+ 文本编码器（Transformer）
2. **训练**: 400M 图像-文本对的对比学习
3. **能力**: 零样本分类、图像-文本检索、特征提取
4. **应用**: 生成模型条件、图像质量评估、多模态理解

## 代表工作

- [[PoseDreamer]]: 使用 CLIP 特征分析生成图像与真实图像的分布相似度
- Learning Transferable Visual Models From Natural Language Supervision (Radford et al., 2021)

## 相关概念

- [[DINO]]
- [[UMAP]]
- [[LAION]]