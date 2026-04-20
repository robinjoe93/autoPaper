---
title: Vision Registers
aliases: [Vision Registers, ViT Registers, Register Tokens]
tags: [vit, representation-learning, vision-transformer]
created: 2026-04-20
---

# Vision Registers

## 定义

[[Vision-Registers]] 是 Vision Transformer 中添加的可学习 Token，用于聚合全局信息并消除注意力图中的 artifacts。该设计由 Darcet et al. (2023) 提出。

## 核心问题

ViT 的注意力图存在两类问题：
1. **Artifacts**: 部分位置被异常关注（非语义相关）
2. **信息丢失**: CLS Token 信息聚合不完整

## 解决方案

在 patch Token 之外添加 $n$ 个 Register Token：
- 这些 Token 参与注意力计算
- 作为信息中转站，聚合全局信息
- 输出时忽略 Register Token，仅使用 patch Token

## 设计细节

| 参数 | 含义 | 典型值 |
|------|------|--------|
| $n$ | Register 数量 | 1-4 |
| 初始化 | 随机可学习 | - |
| 使用 | 仅中间计算 | 输出时忽略 |

## 与 HY-Embodied-0.5 的联系

HY-Embodied-0.5 的 [[Visual Latent Tokens]] 受 Vision Registers 启发：
- 在每个视觉元素末尾添加 1 个 Latent Token
- 用于连接视觉和语言模态
- 受教师 ViT 的全局特征监督

## 优势

1. **信息聚合**: 更完整的全局信息收集
2. **注意力清理**: 消除 artifacts，注意力更聚焦语义
3. **下游性能**: 提升 dense prediction 任务表现

## 相关概念

- [[Vision-Transformer]]: 基础架构
- [[Visual-Latent-Tokens]]: Vision Registers 在具身 VLM 中的应用