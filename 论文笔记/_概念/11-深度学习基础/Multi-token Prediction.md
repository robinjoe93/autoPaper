---
type: concept
aliases: [MTP, Multi-Token-Prediction, 多 token 预测]
---

# Multi-token Prediction

## 定义

一种训练和推理技术，模型同时预测多个未来 token，用于提升基础模型性能和作为 speculative decoding 的 draft model。

## 数学形式

$$
L_{MTP} = -\sum_{t=1}^{n} \log P(y_{t+1}, y_{t+2}, \ldots, y_{t+n} | y_{\leq t})
$$

训练时共享 n 个 MTP 层参数：

$$
W_{MTP}^{(1)} = W_{MTP}^{(2)} = W_{MTP}^{(n)}
$$

## 核心要点

1. **训练增益**: 强迫模型规划未来 token，提升推理能力
2. **Speculative Decoding**: 作为 draft model 加速推理
3. **参数共享**: 减少内存开销，提高 accept rate
4. **Training-Inference 贴合**: 共享参数减少 discrepancy

## 代表工作

- [[GLM-5]]: 共享 3 个 MTP 层参数，accept length 2.76
- [[DeepSeek-V3]]: MTP 的原始应用

## 相关概念

- [[Speculative Decoding]]: MTP 的推理加速应用
- [[Draft Model]]: speculative decoding 的辅助模型
- [[Accept Length]]: speculative decoding 的效率指标