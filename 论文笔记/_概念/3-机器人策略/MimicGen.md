---
type: concept
aliases: [MimicGen]
---

# MimicGen

## 定义

MimicGen 是 NVIDIA 提出的自动化数据生成系统，通过从少量人类演示中提取物体中心的操作模式，自动生成大规模机器人操作数据集。

## 核心机制

1. **物体中心轨迹**: 将演示分解为以物体为参考的轨迹段
2. **场景重放**: 在新场景中重用物体中心的操作模式
3. **源演示选择**: 基于物体位姿距离智能选择最匹配的源演示
4. **轨迹适应**: 将源轨迹变换到新物体位置

## 关键假设

- 存在静态物体参考帧（适用于刚体）
- 任务可分解为固定的子任务序列

## 局限性

- 仅支持刚体操作，不适用于可变形物体
- 依赖固定参考帧，限制了应用范围

## 代表工作

- Mandlekar et al. (2023): "MimicGen: A Data Generation System for Scalable Robot Learning"
- [[SoftMimicGen]]: 扩展到可变形物体操作

## 相关概念

- [[Imitation Learning]]: 下游学习方法
- [[SoftMimicGen]]: 可变形物体版本
- [[Deformable Object Manipulation]]: SoftMimicGen 解决的问题