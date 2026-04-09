---
type: concept
aliases: [预训练数据整理, 数据清洗, 数据质量控制]
---

# Pretraining Data Curation

## 定义

对大规模预训练语料进行系统化清洗、筛选、质量控制的流程，旨在提升数据质量从而改善模型训练效果。

## 数学形式

数据质量评估：

$$
Q(D) = \sum_{c \in C} w_c \cdot \text{Score}(S_c; D_c)
$$

**符号说明**:
- $C$: 数据类别集合
- $w_c$: 类别权重
- $S_c$: 类别 $c$ 的整理策略
- $D_c$: 类别 $c$ 的数据

## 核心要点

1. **清洗策略**: HTML标签去除、去重、PII过滤、格式归一化
2. **领域感知**: 不同类别采用特定策略（数学、医学、代码等）
3. **质量控制**: 困惑度筛选、教育质量评分、多样性维护
4. **自动化挑战**: 策略空间离散、效果映射不清、需迭代验证

## 代表工作

- [[ASI-Evolve]]: 进化策略使MMLU提升+18.64分
- Nemotron-CC: 大规模高质量预训练数据
- DCLM, FineWeb-Edu: 公开数据整理基准

## 相关概念

- [[Synthetic Data]]
- [[Data Curation]]
- [[Evolutionary Agent]]