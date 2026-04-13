---
type: concept
aliases: [VLM, 视觉语言模型]
---

# Vision Language Model

## 定义

能够理解和生成跨视觉和语言模态内容的模型，结合图像理解和文本生成能力。

## 数学形式

$$
\text{output} = \text{VLM}(image, text)
$$

通常由视觉编码器 + 语言模型 + 跨模态对齐组成。

## 核心要点

1. 跨模态理解：图像 + 文本联合处理
2. 可用于图像描述、VQA、多模态推理
3. P2P 使用 VLM 离线生成文本注释

## 代表工作

- [[CLIP]]: 对比学习 VLM
- [[OpenVLA]]: 基于 VLM 的动作模型
- [[P2P]]: VLM 用于文本注释生成

## 相关概念

- [[Transformer]]
- [[OpenVLA]]
- [[CLIP]]