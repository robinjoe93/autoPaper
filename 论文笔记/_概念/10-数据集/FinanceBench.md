---
type: concept
aliases: [金融问答基准]
---

# FinanceBench

## 定义

FinanceBench 是一个金融领域问答基准，基于公司财务报告文档，用于评估模型在金融领域的知识学习和问答能力。

## 数据集统计

| 项目 | 数值 |
|------|------|
| 文档数量 | 1367 |
| 平均 tokens/doc | 16K |
| 评估问题数 | 150 |
| 评估类型 | Free-form（开放问答） |
| 领域 | 金融 |

## 核心要点

1. **最多文档**: 1367 个 PDF 文档
2. **最长文档**: 平均 16K tokens
3. **开放问答**: 非 MCQA，需要生成完整回答
4. **PDF 处理**: 使用 olmOCR-2-7B-1025 转换为 Markdown

## 代表工作

- Islam et al. (2023): 提出 FinanceBench 基准
- [[SMT]]: 泛化验证基准之一

## 用途

- 金融领域知识学习测试
- PDF 文档理解评估
- 开放式生成评估

## 评估方式

使用 Qwen3-14B 作为 judge，对比模型生成答案与 gold answer。

## 相关概念

- [[QuaLITY]]
- [[LongHealth]]
- [[Synthetic Mixed Training]]