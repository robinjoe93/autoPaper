---
type: concept
aliases: [Temperature Sampling, Temperature, 温度采样]
---

# Temperature Sampling

## 定义
一种调整语言模型采样分布的方法，通过调节温度参数控制样本的多样性。

## 数学形式

调整后的 logits：$s_\theta = \frac{1}{\tau} \log \hat{x}_\theta$

- $\tau < 1$: 更集中在高概率 token（更确定）
- $\tau > 1$: 分布更均匀（更随机）

## 核心要点
1. 温度降低使样本更集中在分布的 mode
2. 可与 top-p 采样结合使用
3. 低温度可提高 generative perplexity 但会偏离真实分布
4. [[D-MMD]] 支持蒸馏带温度的教师模型

## 代表工作
- [[D-MMD]]: 支持温度蒸馏，教师 logits 为 $s_\theta = \frac{1}{\tau} \log \hat{x}_\theta$
- [[Gradient Moment]]: 显示低温度虽改善 perplexity 但增加 GM

## 相关概念
- [[Gradient Moment]]
- [[Teacher Model]]
- [[D-MMD]]