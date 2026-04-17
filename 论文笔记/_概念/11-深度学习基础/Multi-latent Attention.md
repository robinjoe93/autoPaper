---
type: concept
aliases: [MLA, Multi-Latent-Attention, 多潜层注意力]
---

# Multi-latent Attention

## 定义

一种注意力机制变体，通过将 KV cache 压缩到低维 latent space 来减少内存占用，同时保持注意力计算的有效性。

## 数学形式

$$
K_{\text{latent}} = W_{UK} \cdot c_K, \quad V_{\text{latent}} = W_{UV} \cdot c_V
$$

其中 $c_K, c_V$ 为压缩后的 latent vectors（如 576 维），替代传统的高维 KV（如 2048 维）。

## 核心要点

1. **KV 压缩**: 使用低维 latent representation 减少 GPU 内存
2. **训练兼容**: Training 时 MHA-style，inference 时 latent 解码
3. **Muon Split**: 对每个 head 独立应用矩阵正交化的优化技巧
4. **头维度调整**: 增加头维度减少解码计算量

## 代表工作

- [[GLM-5]]: MLA-256 配置，576 维 latent KV
- [[DeepSeek-V2/V3]]: MLA 的原始提出者

## 相关概念

- [[DeepSeek Sparse Attention]]: 与 MLA 结合使用
- [[Grouped Query Attention]]: MLA 的替代方案
- [[PagedAttention]]: KV cache 管理技术