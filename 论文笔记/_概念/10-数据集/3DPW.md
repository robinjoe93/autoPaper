---
type: concept
aliases: [3DPW, 3D Poses in the Wild]
---

# 3DPW

## 定义

真实世界 3D 人体姿态数据集，使用 IMU 设备在户外场景采集，提供视频序列和 3D SMPL 标注。

## 核心要点

1. **采集方式**: 移动设备 + IMU 传感器
2. **标注**: 3D SMPL 参数（通过 IMU 优化）
3. **规模**: 约 51 个视频序列，户外场景
4. **用途**: 常作为 3D 姿态估计 benchmark
5. **特点**: 真实场景，动态序列

## 代表工作

- [[PoseDreamer]]: 作为下游模型测试 benchmark
- 3D Poses in the Wild (von Marcard et al., 2018)

## 相关概念

- [[MSCOCO]]
- [[SMPL]]
- [[Human Mesh Recovery]]