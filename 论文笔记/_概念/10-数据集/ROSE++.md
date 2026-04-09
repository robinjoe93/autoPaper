---
title: "ROSE++"
tags: [dataset, video-object-insertion, illumination-aware]
created: 2026-04-03
---

# ROSE++ Dataset

## 定义

一个用于视频对象插入任务的 illumination-aware 合成数据集，通过将 ROSE 物体移除数据集转换为 triplet 格式构建。

## 数据格式

每个样本包含 triplet：
1. **Object-removed video**: 移除对象后的视频（无阴影/反射）
2. **Object-present video**: 原始包含对象的视频
3. **Reference object image**: VLM 生成的对象参考图

## 构建流程

1. 从 ROSE 数据集获取移除/原始视频 pair
2. 使用 VLM ([[GPT-4V]] 等) 从多视角裁剪生成对象图
3. 通过 DINO 相似度评分选择最佳候选

## 核心创新

- **任务反转**: 将 removal 转为 insertion
- **光照监督**: 提供阴影/反射监督信号
- **VLM 生成**: 避免 copy-and-paste 瑕疵

## 应用场景

- [[InsertAnywhere]]: 监督训练数据
- 视频对象插入模型训练
- 光照感知视频编辑

## 关联概念

- [[ROSE Dataset]] (原始版本)
- [[Video Object Insertion]]
- [[DINO Similarity]]