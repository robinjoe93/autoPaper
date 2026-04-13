---
type: concept
aliases: [Uniform Diffusion, 均匀扩散]
---

# Uniform Diffusion

## 定义
一种离散扩散过程，通过逐步将数据 token 随机替换为词汇表中任意 token（均匀分布）来实现噪声化。

## 数学形式

扩散过程：$z_t \sim \text{Cat}(\alpha_t x + (1-\alpha_t) \pi_{\text{uniform}})$

其中 $\pi_{\text{uniform}}$ 是词汇表上的均匀分布

## 核心要点
1. 不像 Masked Diffusion 有吸收态，每个 token 可以被任意其他 token 替换
2. 采样时从均匀噪声逐步恢复数据分布
3. 对于某些参数化，$\mathbb{E}_q[x | z_t]$ 可能不是最优解，需要特殊处理
4. 噪声状态本身包含足够噪声，蒸馏时无需额外噪声输入

## 代表工作
- [[D-MMD]]: Uniform D-MMD 在 CIFAR10 上用 32 步达到 FID 3.7
- Hoogeboom et al. (2021): Argmax flows and multinomial diffusion

## 相关概念
- [[Discrete Diffusion Models]]
- [[Masked Diffusion]]
- [[Teacher Model]]