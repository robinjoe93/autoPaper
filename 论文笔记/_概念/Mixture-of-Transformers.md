---
title: Mixture-of-Transformers (MoT)
aliases: [MoT, Modality-Adaptive Transformer]
tags: [architecture, multimodal, vision-language-model]
created: 2026-04-20
---

# Mixture-of-Transformers (MoT)

## 定义

[[Mixture-of-Transformers]] 是一种多模态架构设计，通过为不同模态（视觉、语言）分配独立的计算参数，实现模态自适应计算。该架构解决了"大量视觉训练损害语言能力"的问题。

## 核心思想

- **模态解耦**: 复制 FFN 和 QKV 参数用于视觉分支，文本分支保持原始参数
- **注意力差异**: 视觉 Token 使用双向注意力，文本 Token 保持因果注意力
- **参数效率**: 实际参数翻倍，但训练/推理开销增加极小

## 与 MoE 的区别

| 特性 | MoE (Mixture-of-Experts) | MoT (Mixture-of-Transformers) |
|------|--------------------------|-------------------------------|
| 激活机制 | Token 级专家选择 | 模态级分支选择 |
| 专家类型 | 同构（同一功能） | 异构（不同模态） |
| 计算分配 | 动态路由 | 固定映射 |

## 代表工作

- **HY-Embodied-0.5**: 腾讯具身 VLM，MoT-2B 架构
- **Mixture-of-Transformers (Liang et al., 2024)**: 原始 MoT 论文

## 优势

1. 保护语言能力不被视觉训练损害
2. 增强视觉建模能力（独立的双向注意力）
3. 训练收敛更快
4. 推理开销增加极小（解码阶段主导）

## 相关概念

- [[Mixture-of-Experts]]: 稀疏激活的专家混合架构
- [[Vision-Language-Action]]: 三模态扩展方向
- [[NaViT]]: 原生分辨率视觉编码