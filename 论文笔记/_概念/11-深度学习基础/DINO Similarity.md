---
title: "DINO Similarity"
tags: [feature-extraction, similarity-metric, self-supervised]
created: 2026-04-03
---

# DINO Similarity

## 定义

基于 DINO 自监督视觉特征计算的图像相似度度量，用于评估视觉一致性。

## 核心公式

$$s_{k}=\frac{1}{N}\sum_{j=1}^{N}\mathrm{DINO}(\hat{o}_{k},f_{j})$$

## 特点

- 无需标签的自监督特征
- 语义级别相似度
- 对外观变化敏感

## 应用场景

- [[InsertAnywhere]]: 选择最佳 VLM 生成的对象图
- [[ROSE++]] 构建: 候选对象图排序
- 视频一致性评估 (DINO-I metric)

## 评估指标

DINO-I: 插入对象与参考图的 DINO 特征相似度

## 关联概念

- [[CLIP-I]] (另一相似度指标)
- [[Self-Supervised Learning]]
- [[Feature Extraction]]