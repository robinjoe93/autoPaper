---
title: "Causal Attention"
category: "架构/注意力机制"
created: 2026-04-20
---

# Causal Attention

## 定义

Causal Attention 是一种注意力掩码机制，限制 token 只能 attend 到序列中位于其之前的 token，防止未来信息泄露。

## 在 GigaWorld-Policy 中的应用

关键设计：不同 token 类型的因果约束：

- $T_s$（状态）和 $T_o$（观测）可互相 attend
- $T_a$（动作）可 attend $T_s$ 和 $T_o$，但不能 attend $T_f$（未来视频）
- $T_f$ 可 attend $T_s$, $T_o$ 和 $T_a$

这使得**推理时可跳过未来视频生成**，直接输出动作。

## 核心优势

- **信息流控制**: 防止未来信息影响当前预测
- **推理效率**: 可选择性跳过部分计算
- **物理一致性**: 保持因果依赖关系

## 相关概念

- [[Self-Attention]]
- [[Transformer]]
- [[Autoregressive Model]]
- [[World-Action Model]]