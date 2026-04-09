---
type: concept
aliases: [SynBody 数据集]
---

# SynBody

## 定义

合成人体数据集，通过渲染引擎生成，提供 3D 姿态标注，用于人体姿态估计训练。

## 核心要点

1. **生成方式**: 渲染引擎合成
2. **规模**: 约 633K 训练样本
3. **标注**: 3D 姿态参数
4. **场景**: In-the-wild 场景
5. **局限**: 与其他渲染数据集类似，真实感有限

## 代表工作

- [[PoseDreamer]]: 图像质量对比中，PoseDreamer IS 9.78 vs SynBody 5.39

## 相关概念

- [[BEDLAM]]
- [[AGORA]]
- [[Human Mesh Recovery]]