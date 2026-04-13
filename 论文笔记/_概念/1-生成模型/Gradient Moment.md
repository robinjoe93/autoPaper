---
type: concept
aliases: [Gradient Moment, GM, GPT-2 GM, 梯度矩]
---

# Gradient Moment

## 定义
一种评估离散扩散模型样本质量的指标，通过参考 LLM 的损失梯度范数衡量生成样本与真实数据的分布距离。

## 数学形式

$$
\|\mathbb{E}_g[\nabla_\theta \log p_{\theta}^{\text{LLM}}(x)] - \mathbb{E}_q[\nabla_\theta \log p_{\theta}^{\text{LLM}}(x)]\|^2
$$

随机估计：
$$
(\nabla_\theta \log p(x_1^g) - \nabla_\theta \log p(x_1^q))^T (\nabla_\theta \log p(x_2^g) - \nabla_\theta \log p(x_2^q))
$$

## 核心要点
1. 当生成分布等于数据分布时，梯度矩为零
2. 比 generative perplexity 更真实反映样本质量
3. 低温度采样可降低 perplexity 但会增加 GM（偏离真实分布）
4. 常用 GPT-2 作为参考模型，称为 GPT-2 GM

## 代表工作
- [[D-MMD]]: 提出并使用此指标评估文本生成质量

## 相关概念
- [[Temperature Sampling]]
- [[Discrete Diffusion Models]]
- [[D-MMD]]