---
type: concept
aliases: [DiMO, Di[m]o]
---

# DiMO

## 定义
一种将 Masked Diffusion 模型蒸馏为单步生成器的方法，通过 straight-through softmax sampling 实现。

## 核心要点
1. 仅支持单步蒸馏，不支持少步生成
2. 使用 straight-through softmax sampling 处理离散采样
3. 生成的算法与 D-MMD 单步版本等价
4. 局限于图像 token 生成，未扩展到文本领域

## 代表工作
- Zhu et al. (2025): Di[m]o: Distilling masked diffusion models into one-step generator
- [[D-MMD]]: 扩展了 DiMO 的思想，支持少步和多类型扩散

## 相关概念
- [[D-MMD]]
- [[Masked Diffusion]]
- [[Discrete Diffusion Models]]