---
type: concept
aliases: [Masked Diffusion, Absorbing State Diffusion, 掩码扩散]
---

# Masked Diffusion

## 定义
一种离散扩散过程，通过逐步将数据 token 转化为特殊的 MASK token 来实现噪声化，最终完全被 MASK。

## 数学形式

扩散过程：$z_t$ 中每个 token 以概率 $(1-\alpha_t)$ 被 MASK

后验分布：
$$
q(z_s | z_t, x) = \text{Cat}\left(z_s \middle| \frac{\alpha_{t|s} z_t + (1-\alpha_{t|s}) \mathbf{1}_{\text{MASK}} \odot [\alpha_s x + (1-\alpha_s) \pi_{\text{MASK}}]}{\alpha_t z_t^\top x + (1-\alpha_t)}\right)
$$

## 核心要点
1. MASK token 是特殊吸收态，一旦到达无法离开
2. 模型学习预测哪些位置应被恢复为真实 token
3. 采样时从全 MASK 状态逐步恢复
4. 对条件期望优化时 $\hat{x}_\theta(z_t) = \mathbb{E}_q[x | z_t]$ 是最优解

## 代表工作
- [[D-MMD]]: Masked D-MMD 在 CIFAR10 上用 64 步达到 FID 3.5
- Austin et al. (2021): 提出 structured denoising diffusion in discrete state-spaces

## 相关概念
- [[Discrete Diffusion Models]]
- [[Uniform Diffusion]]
- [[Teacher Model]]