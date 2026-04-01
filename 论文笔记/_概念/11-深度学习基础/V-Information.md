---
type: concept
aliases: [V-Information, V-Usable Information]
---

# V-Information

## 定义

量化数据集对特定模型族$\mathcal{V}$提供的可用信息量，用于评估数据集难度和有用性。

## 数学形式

$$
I_V(X \rightarrow Y) = H_V(Y) - H_V(Y|X)
$$

其中条件熵和预测熵定义为：
$$
H_{\mathcal{V}}(Y \mid X) = \inf_{f \in \mathcal{V}} \mathbb{E}\left[-\log_2 f[X](Y)\right]
$$
$$
H_{\mathcal{V}}(Y) = \inf_{f \in \mathcal{V}} \mathbb{E}\left[-\log_2 f[\emptyset](Y)\right]
$$

## 核心要点

1. 扩展传统信息论概念，考虑模型族约束
2. 高V-Information表示数据集更容易被模型学习
3. 可用于评估数据集对特定任务的适用性

## 代表工作

- [[SDQM]]: V-Information是相关性最高的子指标(>0.62)

## 相关概念

- [[Entropy]]
- [[Information Theory]]
- [[Dataset Quality]]