---
type: concept
aliases: [认知库, 先验知识库, 人类知识注入]
---

# Cognition Base

## 定义

ASI-Evolve框架的核心组件，编码任务相关的启发式、已知陷阱、设计原则等人类先验知识，通过语义检索注入每轮探索以加速收敛。

## 数学形式

认知检索：

$$
R_t = \text{Retrieve}(C; S_t) = \text{TopK}_{sim}(C, \text{Embed}(S_t))
$$

**符号说明**:
- $C$: 认知库（文本条目+嵌入索引）
- $S_t$: 采样节点的分析或动机
- $\text{Embed}$: 语义嵌入函数
- $sim$: 相似度度量（如cosine）

## 核心要点

1. **内容来源**: 领域论文、已知失败模式、设计原则
2. **检索方式**: 基于节点信息的语义相似搜索
3. **关键作用**: 加速冷启动，不限制长期探索
4. **演化洞察**: 初期51.7%依赖认知库，后期44.8%依赖自身经验

## 代表工作

- [[ASI-Evolve]]: 核心创新组件，消融证明其重要性

## 相关概念

- [[Evolutionary Agent]]
- [[Evolutionary Search]]
- [[Human Prior]]