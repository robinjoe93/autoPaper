---
type: concept
aliases: [软体仿真, Soft Body Simulation, Deformable Simulation]
---

# Soft Body Simulation

## 定义

软体仿真是指模拟可变形物体（如布料、绳子、软组织等）的物理行为，包括弹性变形、碰撞、摩擦等。

## 主要方法

1. **有限元法 (FEM)**: 将物体离散为有限元，精确但计算量大
2. **质点-弹簧系统**: 简化模型，计算高效
3. **Position-Based Dynamics (PBD)**: 约束驱动，实时性好
4. **物质点法 (MPM)**: 适合大变形和流体耦合

## 核心要点

1. **实时性要求**: 机器人仿真需要实时或超实时速度
2. **物理精度**: 需要足够精确以反映真实物理
3. **节点表示**: 物体状态由节点/粒子位置描述
4. **Ground-truth 信息**: 仿真提供精确的节点位置和速度

## 在 SoftMimicGen 中的应用

提供 ground-truth 节点信息用于非刚性配准，实现精确的场景匹配。

## 代表工作

- Isaac Gym/Gymnasium: NVIDIA 的机器人仿真平台
- MuJoCo: 支持软体仿真
- [[SoftMimicGen]]: 使用高保真软体仿真环境

## 相关概念

- [[Deformable Object Manipulation]]: 应用领域
- [[Non-Rigid Registration]]: 利用仿真节点信息
- [[SoftMimicGen]]: 仿真环境基础