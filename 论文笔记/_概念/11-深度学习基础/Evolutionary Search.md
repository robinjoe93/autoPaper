---
type: concept
aliases: [进化搜索, 程序进化, 进化优化]
---

# Evolutionary Search

## 定义

基于生物进化原理的搜索范式，通过迭代生成、评估、选择、变异候选解来探索复杂的程序或解决方案空间。

## 数学形式

迭代过程：

$$
p_{t+1} = \text{Select}(\text{Mutate}(p_t), \text{Score}(p_t))
$$

或更一般的：

$$
p_t \sim P(p | S_t, R_t)
$$

**符号说明**:
- $p_t$: 第 $t$ 代的候选程序
- $S_t$: 采样历史节点
- $R_t$: 检索的相关知识

## 核心要点

1. **核心循环**: 生成→评估→选择→变异→下一代
2. **选择策略**: fitness-based, diversity-preserving, UCB
3. **变异方式**: 全代码重写、diff增量编辑
4. **知识注入**: 认知库提供先验，加速冷启动

## 代表工作

- [[ASI-Evolve]]: learn–design–experiment–analyze闭环
- [[AlphaEvolve]]: 改进Strassen算法、数学问题
- [[FunSearch]]: 发现组合优化算法
- [[OpenEvolve]]: 开源进化框架

## 相关概念

- [[Evolutionary Agent]]
- [[MAP-Elites]]
- [[UCB1 Sampling]]
- [[Cognition Base]]