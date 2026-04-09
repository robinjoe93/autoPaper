---
title: "Best Object Selection"
tags: [vlm, object-generation, similarity-ranking]
created: 2026-04-03
---

# Best Object Selection

## 定义

从 VLM 生成的多个候选对象图中，选择与原视频最匹配的最佳参考图的过程。

## 核心公式

$$
o^{*}=\arg\max_{\hat{o}_{k}}s_{k}
$$

## 参数说明

- $o^{*}$: 最终选择的参考对象图
- $\hat{o}_{k}$: 第 $k$ 个 VLM 生成的候选
- $s_{k}$: DINO 相似度得分

## 选择流程

1. VLM 生成 $m$ 个候选对象图
2. 计算每个候选与原视频 $N$ 个采样帧的 DINO 相似度
3. 选择得分最高的候选

## 核心优势

- 避免 copy-and-paste 瑕疵
- 保持训练/推理输入一致性
- 生成干净的白背景对象图

## 应用场景

- [[ROSE++]] 构建: 选择最佳参考图
- [[InsertAnywhere]]: 数据集构建流程

## 关联概念

- [[DINO Similarity]]
- [[ROSE++]]
- [[VLM]]