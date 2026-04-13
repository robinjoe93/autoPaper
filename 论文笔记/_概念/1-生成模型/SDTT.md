---
type: concept
aliases: [SDTT, Self-Distillation Through Time]
---

# SDTT

## 定义
一种应用于离散扩散的渐进蒸馏方法，类似连续扩散中的 progressive distillation。

## 核心要点
1. 通过逐步减少采样步数进行蒸馏
2. 因子化输出假设导致无法处理相关性 token
3. 多轮蒸馏后性能下降，容易丢失模式
4. 对于完全相关的例子（如两个相关硬币）无法正确建模

## 性能表现
- GPT-2 GM: 4 步达到 0.820（远高于 D-MMD 的 0.236）
- Generative PPL: 72.1（D-MMD 为 17.2）
- 存在明显的模式丢失问题

## 代表工作
- Deschenaux & Gulcehre (2025): Beyond autoregression: Fast LLMs via self-distillation through time
- [[D-MMD]]: 对比实验中作为 baseline

## 相关概念
- [[D-MMD]]
- [[Di4C]]
- [[Discrete Diffusion Models]]