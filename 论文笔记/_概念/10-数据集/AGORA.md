---
type: concept
aliases: [AGORA 数据集]
---

# AGORA

## 定义

大规模合成人体数据集，在真实背景中渲染多人 3D 场景，提供 SMPL 和 SMPL-X 标注，用于多人人体姿态估计。

## 核心要点

1. **生成方式**: Unreal Engine 渲染
2. **特点**: 多人场景，真实背景
3. **标注**: SMPL + SMPL-X 参数
4. **规模**: 约 107K 训练样本
5. **场景**: In-the-wild 多场景，多人互动

## 代表工作

- [[PoseDreamer]]: 作为 benchmark 对比基线
- AGORA: Avatar Guidance for Outdoor Reconstruction (Varol et al., 2021)

## 相关概念

- [[BEDLAM]]
- [[SMPL-X]]
- [[Human Mesh Recovery]]