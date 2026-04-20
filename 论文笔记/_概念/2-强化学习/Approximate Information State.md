---
type: concept
aliases: [AIS, 近似信息状态]
---

# Approximate Information State

## 定义

近似信息状态 (AIS) 是部分可观测系统中用于决策的一种隐状态表示，它包含足够信息以预测未来奖励和自身演化。

## 数学形式

隐状态 $z_i$ 构成 AIS 需满足两个条件：

**奖励预测条件**：
$$
\mathbb{E}[r_i \mid h_i, a_i] \approx R_{\text{pred}}(z_i, a_i)
$$

**自预测条件**：
$$
P(z_{i+1} \mid h_i, a_i) \approx P(z_{i+1} \mid z_i, a_i)
$$

其中 $h_i$ 是历史序列，$z_i$ 是编码器输出的隐状态。

## 核心要点

1. **Markov 隐空间**: AIS 使隐状态转移在隐空间内近似 Markov
2. **任务相关性**: 隐状态必须包含足够预测奖励的信息
3. **历史压缩**: 将任意长度历史压缩为固定维度隐状态
4. **决策充分性**: AIS 是最优决策的充分统计量

## 与 Sim2Real 的关系

在抽象 sim2real 问题中，状态抽象诱导部分可观测性。基于 AIS 的隐状态设计可以：
- 隐式捕获被抽象掉的动力学信息
- 提供任务相关的历史表征
- 支持历史依赖的模拟器修正

## 代表工作

- [[ASTRA]]: 使用 AIS 进行抽象模拟器修正
- Subramanian et al. (2022): AIS 用于部分可观测系统规划

## 相关概念

- [[Self-Predictive Representation]]: 相关的表征学习方法
- [[State Abstraction]]: 抽象诱导部分可观测性
- [[Partial Observability]]: AIS 解决的核心问题
- [[POMDP]]: AIS 的理论背景