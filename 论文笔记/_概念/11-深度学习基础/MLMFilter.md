---
type: concept
aliases: [MLM-based Filter, MLLM Quality Filter]
---

# MLMFilter

## 定义

基于 MLLM 的图文质量过滤器，使用 GPT-4V 生成 100-way quality scores 训练质量评估模型。

## 核心要点

1. 提出 4 种质量指标: ITM, ODF, CTQ, SU
2. 需要昂贵的 GPT-4V 标注成本
3. 仅支持 caption 数据，不支持 interleaved

## 代表工作

- [[UniFilter]]: 对比方法，UniFilter 使用更少质量等级但支持 interleaved

## 相关概念

- [[DFN]]
- [[CLIPScore]]