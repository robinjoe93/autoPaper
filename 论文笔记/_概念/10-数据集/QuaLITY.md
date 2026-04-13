---
type: concept
aliases: [质量问答基准, 长文档问答]
---

# QuaLITY

## 定义

QuaLITY 是一个长文档阅读理解基准，用于评估模型在需要获取新知识的场景下的问答能力。

## 数据集统计

| 项目 | 数值 |
|------|------|
| 文档数量 | 265 |
| 平均 tokens/doc | 6K |
| 评估问题数 | 4609 |
| 评估类型 | MCQA（多选题） |
| 领域 | 虚构故事 |

## 核心要点

1. **长文档挑战**: 平均 6K tokens，需要模型处理长上下文
2. **新知识测试**: 文档为虚构内容，模型需从头学习
3. **知识学习基准**: 用于验证合成数据训练的知识获取效果

## 代表工作

- Pang et al. (2022): 提出 QuaLITY 基准
- [[SMT]]: 主要实验基准，超越 RAG 4.4%

## 用途

- 验证 continued pretraining 效果
- 评估模型知识学习能力
- 对比 RAG 与参数化知识

## 相关概念

- [[RAG]]
- [[Synthetic Mixed Training]]
- [[LongHealth]]
- [[FinanceBench]]