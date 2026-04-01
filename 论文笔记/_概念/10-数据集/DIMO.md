---
type: concept
aliases: [Dataset of Industrial Metal Objects]
---

# DIMO

## 定义

高分辨率工业金属物体数据集，用于评估合成数据对目标检测模型泛化能力的影响。

## 数据规模

- 合成数据：553,800图像 / 42,600场景
- 使用Unity引擎生成
- 包含真实-合成配对数据

## 核心要点

1. 模拟真实光照、物体姿态、纹理
2. 纹理使用合成算法模拟真实效果（如划痕）
3. SDQM中用于特征提取器评估

## 代表工作

- [[SDQM]]: 特征提取模型评估数据集

## 相关概念

- [[Synthetic Data]]
- [[GroundingDINO]]
- [[RarePlanes]]