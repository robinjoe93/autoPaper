---
type: concept
aliases: [推理时扩展, TTS, Inference-Time Compute]
---

# Test-Time Scaling

## 定义
Test-Time Scaling (TTS) 是在推理阶段增加计算资源（如生成更多 token、尝试多次）以提升模型性能的策略。

## 数学形式

$$
\text{Performance}(n) \propto f(n)
$$

其中 $n$ 是推理时计算量（如生成次数、token 数量）。

常见的 scaling 策略：
- 生成多个解法后投票
- 生成更长的推理链
- 多次尝试直到满意

## 核心要点
1. **推理时资源**: 用更多推理计算换取更高准确率
2. **无需训练**: 不改变模型参数
3. **灵活可控**: 可根据需求调整计算量
4. **代价权衡**: 计算成本与性能的权衡

## 代表工作
- [[HoneyBee]]: Shared Caption Decoding 减少 73% 推理成本
- Snell et al. (2024): Scaling Laws for Inference-Time Compute

## 相关概念
- [[Chain-of-Thought]]: TTS 通过更长 CoT 实现
- [[Self-Consistency]]: TTS 通过多次生成实现
- [[HoneyBee]]: 提出 Shared Caption 高效 TTS