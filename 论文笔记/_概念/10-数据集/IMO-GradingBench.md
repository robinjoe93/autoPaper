---
type: concept
aliases: [IMO-GradingBench, 奥数评分数据集]
---

# IMO-GradingBench

## 定义

Luong et al. (2025) 构建的奥林匹克数学评分benchmark，包含IMO级问题和专家评分标注。

## 核心要点

1. **评分标签**: {0, 1, 6, 7} 对应 {incorrect, partial, almost, correct}
2. **完整信息**: 包含问题、候选解、参考解和评分指南
3. **专家标注**: 由专业数学评分员提供评分
4. **迁移评估**: 用于评估跨域迁移的元级改进效果

## 代表工作

- [[Hyperagents]]: 用于数学评分领域的验证
- Luong et al. (2025): IMO-GradingBench 原始论文

## 相关概念

- [[Task Agent]]: 评分智能体
- [[BetterGrader]]: DGM-H 发现的改进评分器