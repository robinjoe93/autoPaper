---
type: concept
aliases: [Implicit Alignment Stage, 隐式对齐阶段]
---

# IAS (Implicit Alignment Stage)

## 定义
LaS-Comp 框架的第二阶段，通过梯度优化 refine latent，确保观测区域与生成区域的边界连贯性。

## 流程

1. Decode $\boldsymbol{x}^*_t$ 得到 $\boldsymbol{S}_{0|t}$
2. 计算 [[Alignment Loss]]（BCE）对齐观测区域
3. 梯度优化一步
4. 加噪到下一 timestep

## 数学形式

$$
\mathcal{L}_{\text{align}} = \text{BCE}(\boldsymbol{S}_{0|t} \odot \boldsymbol{M}, \boldsymbol{S}_{p} \odot \boldsymbol{M})
$$

$$
\boldsymbol{x}^{\text{aligned}}_{0|t} = \hat{\boldsymbol{x}}_{0|t} - \eta \cdot \nabla_{\hat{\boldsymbol{x}}_{0|t}} \mathcal{L}_{\text{align}}
$$

## 核心要点
1. **Boundary Coherence**: 仅 ERS 不够，需要 IAS 确保 boundary 无 holes
2. **一步优化足够**: 实验表明 1 步 vs 10 步无明显差异
3. **梯度优化**: 在 latent space 直接优化，避免迭代采样

## 代表工作
- [[LaS-Comp]]: 提出并使用 IAS

## 相关概念
- [[ERS]]
- [[Alignment Loss]]
- [[BCE Loss]]
- [[Latent-Spatial Gap]]