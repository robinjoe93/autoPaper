---
type: concept
aliases: [Async Agent RL, Asynchronous RL for Agents]
---

# Asynchronous Agent RL

## 定义

一种 RL 训练框架，将训练引擎与推理引擎解耦，异步生成 trajectory 并训练，解决长 horizon agent 任务训练效率问题。

## 数学形式

优化目标（token-level clipping）:

$$
L(\theta) = \mathbb{E}_t \left[ f(r_t(\theta), \epsilon_\ell, \epsilon_h) \hat{A}_t \log \pi_\theta(a_t|s_t) \right]
$$

Importance sampling ratio:

$$
r_t(\theta) = \exp(\log \pi_\theta(a_t|s_t) - \log \pi_{rollout}(a_t|s_t))
$$

## 核心要点

1. **训练-推理解耦**: 不同 GPU 设备上运行
2. **异步生成**: Rollout engine 持续生成 trajectory
3. **Periodic Sync**: 定期同步权重到 rollout engine
4. **Off-policy 处理**: 使用 DDIS 和样本过滤

## 代表工作

- [[GLM-5]]: Agentic RL stage 使用异步框架
- [[slime]]: GLM-5 的统一 post-training 基础设施

## 相关概念

- [[TITO]]: Token-in-Token-out 保证一致性
- [[DDIS]]: Direct Double-sided Importance Sampling
- [[Multi-Task Rollout Orchestrator]]: 多任务管理组件