---
title: "ROSE Dataset"
tags: [dataset, object-removal, synthetic]
created: 2026-04-03
---

# ROSE Dataset

## 定义

一个合成的视频物体移除数据集，专门设计用于训练物体移除模型。

## 数据组成

每个样本包含：
1. Object-present video (原始视频)
2. Object mask (对象区域)
3. Object-removed video (移除后视频，无阴影/反射)

## 特点

- 合成数据，精确控制
- 移除光学副作用（阴影、反射）
- 无显式对象图像（仅设计为移除任务）

## 扩展

[[ROSE++]] 将其扩展为 triplet 格式，添加 VLM 生成的参考图，用于插入任务。

## 关联概念

- [[ROSE++]] (扩展版本)
- [[Video Object Removal]]
- [[Video Object Insertion]]