---
type: concept
aliases: [空间替换, Spatial-domain Replacement]
---

# Spatial Replacement

## 定义
在 spatial domain（occupancy grid）而非 latent space 直接替换观测区域的几何信息。

## 数学形式

$$
\boldsymbol{S}'_{0|t} = \boldsymbol{S}_{p} \odot \boldsymbol{M} + \boldsymbol{S}_{0|t} \odot (1 - \boldsymbol{M})
$$

## 核心要点
1. **为什么在 Spatial Domain**: Latent encoding 存在 [[Latent-Spatial Gap]]，直接 latent replacement 不可靠
2. **流程**: Decode 到 spatial → 替换观测区域 → Encode 回 latent
3. **保真度**: 确保生成结果在观测区域与 partial input 一致

## 与 Latent Replacement 的对比

| 方面 | Spatial Replacement | Latent Replacement |
|------|---------------------|-------------------|
| 可靠性 | 高（几何直接注入） | 低（latent gap） |
| 计算开销 | 需要 decode/encode | 直接操作 latent |

## 代表工作
- [[LaS-Comp]]: ERS 使用 spatial replacement

## 相关概念
- [[ERS]]
- [[Latent-Spatial Gap]]
- [[Occupancy Grid]]
- [[Mask]]