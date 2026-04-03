---
title: Evolutionary Data Generation
tags: [synthetic-data, evolutionary-algorithm, data-generation]
created: 2026-04-03
---

# Evolutionary Data Generation

## 定义

基于进化算法的合成数据生成方法，通过迭代变异、交叉、选择操作优化数据样本。

## 工作流程

1. **初始化**: 从种子数据开始
2. **变异**: 对现有样本进行修改
3. **交叉**: 组合多个样本特征
4. **选择**: 保留高质量样本
5. **迭代**: 重复直到满足条件

## 局限性

- **随机性**: 缺乏可解释的生成过程
- **审计困难**: 无法追溯样本来源
- **种子依赖**: 需要目标分布的种子数据
- **可控性差**: 难以精确控制生成方向

## 对比 Simula

[[Simula]] 采用推理驱动方法，提供：
- 可解释的分类法追溯
- 无种子数据依赖
- 精细的生成控制

## 代表工作

- Evol-Instruct
- [[Simula]] (对比方法)

## 相关概念

- [[Evolutionary Algorithm]]
- [[Simula]]
- [[Synthetic Data]]