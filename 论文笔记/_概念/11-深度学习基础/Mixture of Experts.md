---
type: concept
aliases: [MoE, Mixture-of-Experts, 专家混合模型]
---

# Mixture of Experts

## 定义

一种神经网络架构，通过动态路由机制选择性地激活部分"专家"模块，在保持大模型容量的同时显著降低计算成本。

## 数学形式

$$
y = \sum_{i=1}^{n} g_i(x) \cdot E_i(x)
$$

其中路由函数 $g_i(x)$ 通常为 top-k softmax：

$$
g_i(x) = \frac{\exp(H(x)_i)}{\sum_{j \in \text{top-k}} \exp(H(x)_j)} \cdot \mathbb{I}[i \in \text{top-k}]
$$

## 核心要点

1. **动态路由**: 每个 token 根据其内容被路由到最相关的 k 个专家
2. **条件计算**: 只激活部分专家，降低训练和推理成本
3. **负载均衡**: 需要辅助损失防止专家负载不均
4. **容量扩展**: 可大幅增加模型参数而不增加激活参数

## 代表工作

- [[GLM-5]]: 744B 总参数，40B 激活参数，256 专家
- [[DeepSeek-V3]]: 671B 总参数，37B 激活参数
- [[Switch Transformer]]: 首个大规模 MoE 语言模型

## 相关概念

- [[DeepSeek Sparse Attention]]: MoE 思想在注意力层的应用
- [[Router Replay]]: 保持训练-推理路由一致性
- [[Expert Parallelism]]: MoE 的分布式训练策略