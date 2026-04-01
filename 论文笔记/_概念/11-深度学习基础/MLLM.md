---
type: concept
aliases: [多模态大语言模型, Multimodal LLM]
---

# MLLM

## 定义

多模态大语言模型（Multimodal Large Language Model），能够处理和理解图像、文本等多种模态输入的大语言模型。

## 数学形式

$$
\text{output} = \text{LLM}([\text{VisionEncoder}(I), \text{TextEmbedding}(T)])
$$

## 核心要点

1. 结合视觉编码器与语言模型处理图文输入
2. 支持 zero-shot 和 few-shot 多模态推理
3. 通常采用 modality fusion 架构（Vision Encoder + Projector + LLM）

## 代表工作

- [[LLaVA]]: Visual instruction tuning
- [[BLIP-2]]: Bootstrapping with frozen encoders
- [[Qwen-VL]]: Frontier VLM with versatile abilities
- [[UniFilter]]: 用于多模态数据质量分类

## 相关概念

- [[SigLIP]]
- [[CLIPScore]]
- [[In-Context Learning]]