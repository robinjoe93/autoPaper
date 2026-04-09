---
type: concept
aliases: [DPO, 直接偏好优化]
---

# Direct Preference Optimization

## 定义

一种直接从人类偏好数据训练语言模型或生成模型的方法，无需显式训练奖励模型，直接优化模型使其生成更符合人类偏好的输出。

## 数学形式

DPO 损失函数：

$$
\mathcal{L}_{\text{DPO}}(\pi_\theta; \pi_{\text{ref}}) = -\mathbb{E}_{(x, y_w, y_l) \sim \mathcal{D}} \left[ \log \sigma\left( \beta \log \frac{\pi_\theta(y_w|x)}{\pi_{\text{ref}}(y_w|x)} - \beta \log \frac{\pi_\theta(y_l|x)}{\pi_{\text{ref}}(y_l|x)} \right) \right]
$$

**符号说明**:
- $\pi_\theta$: 待优化策略/模型
- $\pi_{\text{ref}}$: 参考模型（通常是 SFT 后的模型）
- $y_w, y_l$: winner 和 loser 的输出
- $\beta$: 温度参数，控制偏好强度

## 核心要点

1. **无需奖励模型**: 直接从偏好数据优化，避免 RLHF 中的奖励模型训练步骤
2. **稳定性更好**: 相比 PPO 等 RL 方法，训练更稳定
3. **数据需求**: 需要成对的偏好数据 $(x, y_w, y_l)$，其中 $y_w \succ y_l$

## 代表工作

- [[PoseDreamer]]: 使用 DPO 对齐可控生成模型，提高 3D 姿态控制精度
- DPO: Direct Preference Optimization for Language Model Alignment (Rafailov et al., 2023)

## 相关概念

- [[RLHF]]
- [[Controllable Generation]]
- [[Diffusion Model]]