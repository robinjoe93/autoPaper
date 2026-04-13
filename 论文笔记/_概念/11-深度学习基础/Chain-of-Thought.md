---
type: concept
aliases: [CoT, 思维链, 链式思维]
---

# Chain-of-Thought

## 定义
Chain-of-Thought (CoT) 是一种推理范式，模型在给出最终答案前先生成一系列中间推理步骤，允许模型利用额外的推理时计算。

## 数学形式

$$
C = [S; P]
$$

其中 $C$ 是完整的 CoT，$S$ 是推理步骤序列，$P$ 是预测答案。

$$
\mathbb{E}_{(I,Q,C) \in \mathcal{D}}[\log p_\theta(C \mid I, Q)]
$$

训练目标：最大化给定 context 下生成 CoT 的概率。

## 核心要点
1. **推理时计算**: 通过生成中间步骤，模型可以使用更多计算资源
2. **可验证性**: 中间步骤使得推理过程可被人类检查
3. **错误纠正**: 允许模型在推理过程中自我反思和纠正
4. **泛化能力**: 相比直接输出答案，CoT 能更好地泛化到新问题

## 代表工作
- [[STaR]]: Self-Taught Reasoner，自我生成 CoT 并过滤
- [[HoneyBee]]: VL 推理 CoT 数据构建
- Wei et al. (2022): Chain-of-Thought Prompting Elicits Reasoning in Large Language Models

## 相关概念
- [[Self-Consistency]]: 多个 CoT 解法聚合
- [[Test-Time Scaling]]: 推理时增加计算
- [[Supervised Finetuning]]: 使用 CoT 数据训练模型