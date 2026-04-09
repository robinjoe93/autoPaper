---
type: concept
aliases: [扩散策略, Diffusion Policy]
---

# Diffusion Policy

## 定义

Diffusion Policy 是一种将扩散模型应用于机器人模仿学习的方法，通过去噪过程生成动作序列，而非直接预测动作。

## 数学形式

动作生成过程：

$$
a_{t:t+K} = \mathcal{D}_\theta(\mathcal{E}(a_{t:t+K}), \mathbf{o}_t)
$$

其中 $\mathcal{D}_\theta$ 是去噪网络，$\mathcal{E}$ 是编码器，$\mathbf{o}_t$ 是当前观测。

## 核心要点

1. **动作序列生成**: 生成未来 K 步动作块，而非单步动作
2. **多模态分布**: 扩散模型天然适合处理多模态动作分布
3. **平滑轨迹**: 去噪过程产生平滑、连贯的动作序列
4. **训练稳定性**: 相比 GMM 等方法，训练更稳定

## 代表工作

- Chi et al. (2023): "Diffusion Policy: Visuomotor Policy Learning via Action Diffusion"
- [[SoftMimicGen]]: 使用 BC-RNN-GMM 作为策略，可替换为 Diffusion Policy

## 相关概念

- [[Imitation Learning]]: 模仿学习
- 生成模型 → 1-生成模型/Diffusion Model