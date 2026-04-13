---
type: concept
aliases: [高效网络]
---

# EfficientNet

## 定义

通过复合缩放同时缩放深度、宽度和分辨率的高效 CNN 架构，在图像分类等任务上取得优异效率。

## 数学形式

复合缩放：
$$
\text{depth} = \alpha^\phi, \quad \text{width} = \beta^\phi, \quad \text{resolution} = \gamma^\phi
$$

其中 $\alpha \cdot \beta^2 \cdot \gamma^2 \approx 2$，$\phi$ 是缩放系数。

## 核心要点

1. 平衡缩放深度、宽度、分辨率
2. 参数效率和计算效率高
3. P2P 使用前 6 层作为图像编码器

## 代表工作

- Tan & Le (2019): EfficientNet 论文
- [[P2P]]: 使用 EfficientNet-B0 前 6 层

## 相关概念

- [[Transformer]]
- [[P2P]]