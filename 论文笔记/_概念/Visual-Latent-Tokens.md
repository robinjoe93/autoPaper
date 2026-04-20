---
title: Visual Latent Tokens
aliases: [视觉潜在Token, Latent Vision Tokens]
tags: [vision-language-model, cross-modal, token-design]
created: 2026-04-20
---

# Visual Latent Tokens

## 定义

[[Visual-Latent-Tokens]] 是 HY-Embodied-0.5 中引入的可学习 Token，附加在每个视觉元素（图像/视频帧）末尾，用于连接视觉和语言模态，提升小 VLM 的感知表达能力。

## 设计动机

受 [[Latent Thinking]] 和 [[Vision-Registers]] 启发：
- 小 VLM 参数有限，需要额外机制连接模态
- Vision Registers 证明额外 Token 可聚合全局信息
- Latent Thinking 展示隐藏计算可增强推理

## 具体实现

1. **位置**: 每个视觉元素（图像/视频帧）末尾添加 1 个 Latent Token
2. **可学习**: Token 参数随机初始化，随模型训练
3. **监督**: 教师 ViT 的全局 CLS 特征监督其输出

## 损失函数

$$
\mathcal{L}_{\text{global}}=-\frac{f_{\text{latent}}^{\top}f_{\text{teacher}}}{\|f_{\text{latent}}\|\|f_{\text{teacher}}\|}
$$

## 功能验证

注意力可视化显示：
- **视觉注意力**: 精确定位显著目标和关键区域
- **语言注意力**: 聚焦语义实体、状态、动作指令
- **连接作用**: 有效聚合视觉信息并与语言对齐

## 适用场景

- 小参数 VLM（2B 级别）
- 视觉-语言跨模态任务
- 具身理解任务（需精确定位 + 语义关联）

## 相关概念

- [[Vision-Registers]]: 设计灵感来源
- [[Latent Thinking]]: 隐藏计算范式
- [[HY-Embodied-0.5]]: 使用该设计的代表工作