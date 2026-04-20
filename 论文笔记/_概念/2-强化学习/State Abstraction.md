---
type: concept
aliases: [状态抽象, State Abstraction Theory]
---

# State Abstraction

## 定义

状态抽象是将高维状态空间映射到低维抽象状态空间的过程，通过合并或丢弃部分状态信息来简化问题。

## 数学形式

抽象函数 $\phi: \mathcal{S}^t \rightarrow \mathcal{S}^s$ 将真实状态映射到抽象状态：

$$
s^s = \phi(s^t)
$$

抽象 MDP：$\mathcal{M}_s = (\mathcal{S}^s, \mathcal{A}, P_s, r_s, \gamma)$

## 核心要点

1. **状态缩减**: $\mathcal{S}^s$ 的维度低于 $\mathcal{S}^t$
2. **信息丢失**: 抽象合并或丢弃部分状态细节
3. **部分可观测**: 抽象诱导部分可观测性，Markov 性质可能失效
4. **保价值性**: 好的抽象应保持最优价值函数近似

## 抽象类型

1. **离散化抽象**: 将连续状态映射到有限离散集合
2. **子空间抽象**: 仅保留部分状态维度
3. **特征抽象**: 使用特征提取函数降维

## 抽象 Sim2Real 问题

当模拟器使用抽象状态空间时：
- 抽象状态无法区分真实世界中的不同配置
- 需要历史信息来恢复部分可观测性
- 标准模拟器修正方法失效

$$
\Pr(s_{i+1}^s|s_i^s,a_i) \neq \Pr(s_{i+1}^s|s_i^s,a_i,s_{i-1}^s,a_{i-1})
$$

## 代表工作

- [[ASTRA]]: 抽象 sim2real 的形式化与解决方案
- Abel (2022): 抽象理论综述
- Allen et al. (2021): 学习 Markov 状态抽象

## 相关概念

- [[Approximate Information State]]: 解决抽象诱导的部分可观测性
- [[Partial Observability]]: 抽象的核心后果
- [[Sim-to-Real Transfer]]: 应用场景