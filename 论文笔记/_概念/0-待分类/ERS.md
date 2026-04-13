---
type: concept
aliases: [Explicit Replacement Stage, 显式替换阶段]
---

# ERS (Explicit Replacement Stage)

## 定义
LaS-Comp 框架的第一阶段，通过 spatial-domain replacement 显式注入 partial geometry 到 latent，确保输入保真度。

## 双分支设计

- **Clean Branch**: 估计 noise-free latent，decode 到 spatial，用 mask 替换观测区域，re-encode
- **Noisy Branch**: 估计 noisy latent，通过 [[PNS]] 调制 stochasticity

## 数学形式

$$
\boldsymbol{S}'_{0|t} = \boldsymbol{S}_{p} \odot \boldsymbol{M} + \boldsymbol{S}_{0|t} \odot (1 - \boldsymbol{M})
$$

$$
\boldsymbol{x}^*_{0|t} = \mathcal{E}(\boldsymbol{S}'_{0|t})
$$

$$
\boldsymbol{x}^*_t = (1 - t) \cdot \boldsymbol{x}^*_{0|t} + t \cdot \boldsymbol{x}^*_{1|t}
$$

## 核心要点
1. **解决 Latent-Spatial Gap**: Spatial domain replacement 比 latent replacement 更可靠
2. **关键贡献**: 消融实验中移除 ERS 导致最大性能下降（CD 从 1.42 升至 3.42）
3. **灵感来源**: [[FlowDPS]] 的 latent decomposition 思想

## 代表工作
- [[LaS-Comp]]: 提出并使用 ERS

## 相关概念
- [[IAS]]
- [[PNS]]
- [[Latent-Spatial Gap]]
- [[Spatial Replacement]]
- [[FlowDPS]]