---
type: concept
aliases: [Rotary Position Embedding, 旋转位置编码]
---

# RoPE

## 定义

旋转位置编码（Rotary Position Embedding），一种将位置信息编码为旋转矩阵的位置编码方法，应用于 Transformer 每一层。

## 数学形式

$$
\text{RoPE}(x_m, m) = \begin{pmatrix} x_m^{(1)} \\ x_m^{(2)} \\ \vdots \\ x_m^{(d-1)} \\ x_m^{(d)} \end{pmatrix} \otimes \begin{pmatrix} \cos(m\theta_1) \\ \sin(m\theta_1) \\ \vdots \\ \cos(m\theta_{d/2}) \\ \sin(m\theta_{d/2}) \end{pmatrix}
$$

其中 $m$ 是位置索引，$\theta_i$ 是预定义的频率。

## 核心要点

1. 相对位置感知：两个 token 的注意力分数仅取决于相对位置差
2. 远程衰减：远距离 token 的注意力自然衰减
3. 线性复杂度：可直接应用于注意力计算

## 代表工作

- RoFormer: 提出该方法
- [[P2P]]: 使用 RoPE 进行位置编码

## 相关概念

- [[Transformer]]
- [[Attention Score]]