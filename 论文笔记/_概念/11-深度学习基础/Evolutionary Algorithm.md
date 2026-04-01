---
type: concept
aliases: [进化算法, Genetic Algorithm, GA]
---

# Evolutionary Algorithm

## 定义

基于自然选择和遗传机制的优化算法，通过种群进化搜索最优解。

## 核心组件

1. **选择**: 根据适应度选择个体
2. **交叉**: 组合两个个体产生后代
3. **变异**: 随机修改个体特征

## SDQM中的应用

Algorithm 1使用进化算法选择数据子集：
- 种群大小$p$
- 交叉概率$P_c$
- 变异概率$P_m$
- 早停条件：适应度<0.005

## 数学形式

适应度函数：
$$
f(D^i, m, m_j) = |m_j - m(D^i)| + \frac{\text{dist}(k_l, k_u, |D_1^i|) + \text{dist}(k_l, k_u, |D_2^i|)}{\max(k_u - k_l, 1)} \cdot \text{Range}(m)
$$

## 代表工作

- [[SDQM]]: 数据子集选择方法

## 相关概念

- [[Genetic Algorithm]]
- [[Optimization]]
- [[Selection]]