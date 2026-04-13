---
type: concept
aliases: [自我一致性, 多路径投票]
---

# Self-Consistency

## 定义

一种推理时缩放方法，通过生成多个独立的推理路径并对结果进行多数投票来提高 LLM 推理准确性。

## 核心要点

1. **并行缩放**: 生成 N 个独立验证/解决方案
2. **多数投票**: 通过投票确定共识判断
3. **无需 Ground Truth**: 不依赖外部参考答案
4. **两种变体**: Outcome-level Consistency（最终 verdict 投票）和 Step-level Consistency（步骤级投票）

## 数学形式

Outcome-level 聚合：
$$
y^* = \arg\max_{y \in \{Yes, No\}} \sum_{j=1}^N \mathbb{1}[y^{(j)} = y]
$$

Step-level 聚合：
$$
v_i^* = \arg\max_{v \in \{correct, incorrect\}} \sum_{j=1}^N \mathbb{1}[v_i^{(j)} = v]
$$

## 代表工作

- [[SPARK]]: Step-level Consistency 达到最佳 PRM 训练效果
- Wang et al. (2023): 提出 Self-Consistency 改进 CoT 推理

## 相关概念

- [[Inference-Time Scaling]]: Self-Consistency 属于并行缩放方法
- [[Process Reward Model|PRM]]: 用于生成 PRM 训练数据
- [[Meta-Critique]]: 顺序缩放方法的对比