---
type: concept
aliases: [动作块, 动作序列预测]
---

# Action Chunking

## 定义

Action Chunking 是一种动作表示方法，将机器人动作序列预测为连续的"块"（chunk），而非单步动作，提高轨迹的平滑性和任务完成效率。

## 核心要点

1. **序列预测**: 同时预测多个时间步的动作，而非逐步预测
2. **平滑轨迹**: 避免单步预测的抖动和不连续
3. **时序一致性**: 保持动作序列的时序连贯性
4. **效率提升**: 减少推理次数，提高执行效率

## 数学形式

动作块 $\mathbf{a}_{t:t+H}$ 表示从时间 $t$ 到 $t+H$ 的动作序列：

$$
\mathbf{a}_{t:t+H} = [a_t, a_{t+1}, ..., a_{t+H-1}]
$$

其中 $H$ 为块长度（horizon）。

## 代表工作

- [[Diffusion Policy]]: 使用扩散模型生成动作块
- [[Pi07]]: 使用流匹配生成动作序列
- [[ACT]]: Action Chunking with Transformer

## 相关概念

- [[Imitation Learning]]: 学习范式
- [[Temporal Consistency]]: 时序一致性
- [[Trajectory Generation]]: 轨迹生成