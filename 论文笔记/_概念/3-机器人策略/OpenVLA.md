---
type: concept
aliases: [Open Vision-Language-Action]
---

# OpenVLA

## 定义

开源的 Vision-Language-Action 模型，基于预训练 VLM 微调生成机器人动作。

## 数学形式

输入：图像 + 文本指令
$$
a = \text{VLM}_{\text{finetuned}}(o, \text{text})
$$

输出：动作 token 序列

## 核心要点

1. 基于 Prismatic VLM 微调
2. 支持 7B 参数开源模型
3. 推理速度较慢，难以实时控制

## 代表工作

- Kim et al. (2024): OpenVLA 论文

## 相关概念

- [[Vision Language Model]]
- [[Behavior Cloning]]
- [[P2P]]