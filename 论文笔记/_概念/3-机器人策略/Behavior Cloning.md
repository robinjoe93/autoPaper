---
type: concept
aliases: [Behavior Cloning, BC, 行为克隆]
category: 机器人策略
---

# Behavior Cloning

## 定义

模仿学习的一种方法，通过从专家演示数据中学习策略，直接复制专家的行为。是最简单、最常用的模仿学习范式。

## 数学形式

BC 损失函数：

$$
\mathcal{L}_{BC} = \sum_{i=1}^{N} \sum_{o} \| \pi_\phi(s_o^i) - a_o^i \|^2
$$

**符号说明**:
- $\pi_\phi$: 策略网络，参数为 $\phi$
- $s_o^i$: 第 $i$ 条轨迹第 $o$ 步的状态/观测
- $a_o^i$: 对应的专家动作
- $N$: 轨迹数量

## 核心要点

1. **数据来源**: 专家演示（人类演示或最优策略）
2. **学习方式**: 监督学习，直接回归动作
3. **局限性**: 误差累积、分布偏移问题
4. **优势**: 简单、稳定、无需环境交互

## 变体与改进

- **DAgger**: 通过专家在线纠正解决分布偏移
- **Action Chunking**: 预测动作序列而非单步动作
- **Diffusion Policy**: 用扩散模型建模动作分布

## 代表工作

- [[ORCA]]: 使用 BC 损失学习 task/visual prompts
- [[Diffusion Policy]]: BC + Diffusion Model
- [[VC-1]]: 使用预训练视觉表征的 BC baseline

## 相关概念

- [[Imitation Learning]]
- [[Diffusion Policy]]
- [[DAgger]]