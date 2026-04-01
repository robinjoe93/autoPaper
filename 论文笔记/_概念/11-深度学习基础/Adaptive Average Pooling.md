---
type: concept
aliases: [自适应平均池化, Adaptive Pooling]
---

# Adaptive Average Pooling

## 定义

自适应平均池化操作，将任意尺寸的输入压缩到固定尺寸的输出，常用于 MLLM 的 visual projector。

## 数学形式

$$
\text{Output}(h', w') = \frac{1}{k_h \times k_w} \sum_{i,j \in \text{kernel}} \text{Input}(i, j)
$$

其中 kernel 大小自适应计算以匹配目标输出尺寸。

## 核心要点

1. 输出尺寸固定，输入尺寸任意
2. 常用于压缩视觉 token 数量
3. 比 Q-Former、C-Abstractor 等压缩方法更高效

## 代表工作

- [[UniFilter]]: 使用 2D AvgPool 压缩图像到 144 tokens
- [[DECO]]: 提出 AvgPool+MLP projector

## 相关概念

- [[MLLM]]
- [[SigLIP]]