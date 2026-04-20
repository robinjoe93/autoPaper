---
type: concept
aliases: [MMD, 最大均值差异]
---

# Maximum Mean Discrepancy

## 定义

最大均值差异 (MMD) 是一种衡量两个概率分布之间差异的非参数度量方法，常用于分布匹配和生成模型评估。

## 数学形式

$$
\text{MMD}^2(P, Q) = \mathbb{E}_{x,x' \in P}[k(x,x')] - 2\mathbb{E}_{x \in P, y \in Q}[k(x,y)] + \mathbb{E}_{y,y' \in Q}[k(y,y')]
$$

其中 $k$ 是核函数（常用 Gaussian 核）：

$$
k(x, y) = \exp\left(-\frac{\|x - y\|^2}{2\sigma^2}\right)
$$

## 核心要点

1. **非参数**: 不需假设分布的具体形式
2. **核方法**: 通过核函数将分布嵌入 RKHS
3. **样本估计**: 可从有限样本中有效估计
4. **统一度量**: 可比较任意两个分布

## 在 ASTRA 中的应用

用于对齐源编码器和目标编码器的潜态分布：

$$
\mathcal{L}_{\text{align}} = \text{MMD}(p_i^s, p_i^t)
$$

其中：
- $p_i^s = P(z_{i+1}^s|z_i^s, a_i)$: 源编码器潜态转移
- $p_i^t = P(z_{i+1}^t|z_i^t, a_i)$: 目标编码器潜态转移

## 相关应用场景

1. [[ASTRA]]: 编码器分布对齐
2. 生成模型评估: 衡量生成分布与真实分布差异
3. 域适应: 源域与目标域特征分布匹配
4. 两样本检验: 统计检验两个样本是否来自相同分布

## 代表工作

- Gretton et al. (2012): MMD 两样本检验
- [[ASTRA]]: 编码器对齐中的应用

## 相关概念

- [[KL Divergence]]: 另一种分布差异度量
- [[Encoder Alignment]]: ASTRA 中的对齐损失
- [[Kernel Method]]: MMD 的理论基础