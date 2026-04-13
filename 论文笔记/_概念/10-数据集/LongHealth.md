---
type: concept
aliases: [长医疗文档问答]
---

# LongHealth

## 定义

LongHealth 是一个基于长临床文档的医疗问答基准，用于评估模型在医疗领域的知识学习和推理能力。

## 数据集统计

| 项目 | 数值 |
|------|------|
| 文档数量 | 400 |
| 平均 tokens/doc | 12K |
| 评估问题数 | 400 |
| 评估类型 | MCQA（多选题） |
| 领域 | 医疗 |

## 核心要点

1. **更长文档**: 平均 12K tokens，比 QuaLITY 更长
2. **医疗专业**: 需要模型学习医疗领域知识
3. **泛化验证**: 用于验证 SMT 的跨域泛化能力

## 代表工作

- Adams et al. (2025): 提出 LongHealth 基准
- [[SMT]]: 泛化验证基准之一

## 用途

- 医疗领域知识学习测试
- 长文档理解评估
- 跨域泛化验证

## 相关概念

- [[QuaLITY]]
- [[FinanceBench]]
- [[Synthetic Mixed Training]]