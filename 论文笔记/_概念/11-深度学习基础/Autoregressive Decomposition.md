---
type: concept
aliases: [Autoregressive, 自回归, 自回归分解]
---

# Autoregressive Decomposition

## 定义

自回归分解是将序列概率分解为条件概率乘积的方法，语言模型基于此逐 token 生成文本。

## 数学形式

$$
P(x_1, ..., x_n) = \prod_{i=1}^{n} P(x_i | x_1, ..., x_{i-1})
$$

## 核心要点

1. **顺序生成**: 每个 token 依赖之前所有 token
2. **KV Cache**: 缓存历史 K/V 避免重复计算
3. **内存瓶颈**: 生成过程内存持续增长

## 代表工作

- [[vLLM]]: 解决自回归生成中的内存问题
- [[GPT]]: 自回归语言模型

## 相关概念

- [[Transformer]]
- [[KV Cache]]
- [[LLM Serving]]