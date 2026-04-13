---
type: concept
aliases: [ADAS, Automated Design of Agent Systems]
---

# ADAS

## 定义

Hu et al. (2025) 提出的自动智能体设计系统，使用固定的元智能体优化任务智能体。

## 核心要点

1. **固定元级**: Meta Agent不可自我修改，限制改进潜力
2. **任务优化**: 专注于优化Task Agent的任务执行能力
3. **无开端探索**: 不使用档案积累，可能过早收敛
4. **DGM-H基线**: 作为 DGM-H without self-improve 的对比

## 代表工作

- Hu et al. (2025): ADAS 原始论文
- [[Hyperagents]]: 扩展ADAS支持元认知自我修改

## 相关概念

- [[Meta Agent]]: ADAS中的固定元级组件
- [[Darwin Godel Machine]]: 与ADAS相似的自我改进方法
- [[Open-endedness]]: ADAS缺少的开端探索能力