---
type: concept
aliases: [SFT, 监督微调]
---

# Supervised Finetuning

## 定义
Supervised Finetuning (SFT) 是在预训练模型基础上使用标注数据进行训练的过程，使模型适应特定任务或领域。

## 数学形式

$$
\mathcal{L}_{SFT} = -\sum_{(x,y) \in \mathcal{D}} \log p_\theta(y \mid x)
$$

最小化负对数似然：最大化模型在给定输入下生成目标输出的概率。

## 核心要点
1. **预训练基础**: 在大规模预训练模型上继续训练
2. **任务适应**: 使模型适应特定下游任务
3. **数据质量**: 高质量标注数据对 SFT 效果至关重要
4. **计算效率**: 相比从头训练，SFT 需要更少数据和计算

## 代表工作
- [[HoneyBee]]: 使用 SFT 训练 VL 推理模型
- Instruction Tuning: 使用指令数据 SFT

## 相关概念
- [[Chain-of-Thought]]: 使用 CoT 数据进行 SFT
- [[Knowledge Distillation]]: SFT 作为 distillation 的方式
- [[Direct Preference Optimization]]: SFT 后的进一步优化方法