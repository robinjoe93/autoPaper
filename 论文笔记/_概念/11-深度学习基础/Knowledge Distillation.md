---
type: concept
aliases: [蒸馏, 知识迁移]
---

# Knowledge Distillation

## 定义
Knowledge Distillation 是一种训练方法，使用强大的教师模型的输出或行为来指导较弱的学生模型训练。

## 数学形式

$$
\mathcal{L}_{KD} = \alpha \mathcal{L}_{hard} + (1-\alpha) \mathcal{L}_{soft}
$$

其中 $\mathcal{L}_{hard}$ 是硬标签损失，$\mathcal{L}_{soft}$ 是软标签损失：

$$
\mathcal{L}_{soft} = KL(p_{teacher}^\tau \| p_{student}^\tau)
$$

温度 $\tau$ 控制软化程度。

## 核心要点
1. **教师-学生架构**: 强模型指导弱模型学习
2. **软标签**: 使用教师模型的概率分布而非硬标签
3. **压缩效果**: 学生模型可更小更快但保持接近教师性能
4. **跨模态应用**: VLM 中常用 generator-to-student 范式

## 代表工作
- [[HoneyBee]]: 使用 Llama4-Scout 作为 generator 教 PLM
- DistilBERT: BERT 的蒸馏版本

## 相关概念
- [[Supervised Finetuning]]: KD 作为特殊形式的 SFT
- [[Chain-of-Thought]]: CoT 数据作为蒸馏内容
- [[VLM]]: Generator-to-Student VL 训练