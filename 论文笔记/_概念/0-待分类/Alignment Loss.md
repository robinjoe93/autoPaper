---
type: concept
aliases: [对齐损失, Geometry Alignment Loss]
---

# Alignment Loss

## 定义
用于对齐生成结果与 partial input 在观测区域的几何一致性损失。

## 数学形式

$$
\mathcal{L}_{\text{align}} = \text{BCE}(\boldsymbol{S}_{0|t} \odot \boldsymbol{M}, \boldsymbol{S}_{p} \odot \boldsymbol{M})
$$

## 核心要点
1. **仅观测区域**: 通过 mask $\boldsymbol{M}$ 限定损失计算范围
2. **Binary Cross Entropy**: 适合 occupancy grid 表示
3. **梯度优化**: 用于 IAS 的 latent refinement

## 作用
通过梯度下降优化 latent，使 boundary 连贯：

$$
\boldsymbol{x}^{\text{aligned}}_{0|t} = \hat{\boldsymbol{x}}_{0|t} - \eta \cdot \nabla_{\hat{\boldsymbol{x}}_{0|t}} \mathcal{L}_{\text{align}}
$$

## 代表工作
- [[LaS-Comp]]: IAS 使用 alignment loss

## 相关概念
- [[IAS]]
- [[BCE Loss]]
- [[ERS]]
- [[Mask]]