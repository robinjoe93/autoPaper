---
type: concept
aliases: [DexGraspNet数据集]
---

# DexGraspNet

## 定义

DexGraspNet是一个大规模灵巧手抓取数据集，包含多种物体的抓取姿态，主要用于训练和评估灵巧抓取算法。

## 核心要点

1. **数据规模**: 包含大量物体的抓取姿态标注
2. **物体来源**: 提供多样化的物体几何形状
3. **生成方法**: 基于可微分力闭合估计的优化方法
4. **应用场景**: 单手灵巧抓取，不支持双手机器人

## 代表工作

- [[UltraDexGrasp]]: 从DexGraspNet选取1000个物体构建双手机器人数据集
- [[DexGraspNet (方法)]]: 作为baseline对比，仅支持单手抓取

## 相关概念

- [[Force Closure]]
- [[Grasp Synthesis]]
- [[Dexterous Grasping]]