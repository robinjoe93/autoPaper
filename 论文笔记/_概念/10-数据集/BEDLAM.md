---
type: concept
aliases: [BEDLAM 数据集]
---

# BEDLAM

## 定义

大规模合成人体数据集，通过渲染引擎生成，包含精确的 3D SMPL 标注，用于人体姿态估计和 mesh recovery 训练。

## 核心要点

1. **生成方式**: Blender 渲染引擎，3D 场景合成
2. **标注**: 精确 SMPL 参数 + 2D 关键点投影
3. **规模**: 约 950K 训练样本
4. **场景**: In-the-wild 多场景
5. **局限**: 渲染图像真实感有限，多样性受限

## 代表工作

- [[PoseDreamer]]: 与 BEDLAM 对比，证明 diffusion 生成数据图像质量更高（FID 1.72 vs 4.55）
- BEDLAM: A Synthetic Dataset of Bodies, Equipped with Lights, Action, and More (Black et al., 2023)

## 相关概念

- [[AGORA]]
- [[SynBody]]
- [[SMPL]]
- [[Human Mesh Recovery]]