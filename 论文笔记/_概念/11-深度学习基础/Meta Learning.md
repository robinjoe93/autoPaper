---
type: concept
aliases: [元学习, Meta-learning, Learning to Learn]
---

# Meta Learning

## 定义

学习如何学习的技术范式，目标是让系统学会改进自身的学习过程。

## 核心要点

1. **双层优化**: 内层优化任务性能，外层优化学习过程
2. **泛化能力**: 学习到的方法应能泛化到新任务
3. **快速适应**: 能用少量样本快速适应新任务
4. **自引用**: 部分方法可学习改进学习算法本身

## 数学形式

双层优化框架：

$$
\min_{\theta} \sum_{i} \mathcal{L}_i(\phi_i), \quad \phi_i = \text{Learn}(\theta, \mathcal{D}_i)
$$

其中 $\theta$ 是元参数，$\phi_i$ 是任务特定参数，$\mathcal{D}_i$ 是任务数据。

## 代表工作

- [[Hyperagents]]: 通过 Meta Agent 实现元级修改
- Kirsch & Schmidhuber (2022): 自引用神经网络元学习
- MAML: Model-Agnostic Meta-Learning

## 相关概念

- [[Meta Agent]]: 执行元学习的智能体组件
- [[Recursive Self-improvement]]: 元学习的终极目标
- [[Self-referential Learning]]: 元学习的自引用形式