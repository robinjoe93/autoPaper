---
type: concept
aliases: [仿真到真实迁移, Sim-to-Real, Zero-shot Transfer]
---

# Sim-to-Real Transfer

## 定义

仿真到真实迁移是指将仿真环境中训练的策略部署到真实机器人系统，解决仿真与真实世界之间的差距问题。

## 主要挑战

1. **视觉域差距**: 仿真图像与真实图像的外观差异
2. **物理域差距**: 物理参数、摩擦、惯性的差异
3. **传感器噪声**: 真实传感器的不确定性和噪声
4. **动作执行**: 真实执行器的延迟和误差

## 主要方法

1. **Domain Randomization**: 在仿真中随机化参数增加鲁棒性
2. **Domain Adaptation**: 学习域不变的表示
3. **Sim-Real Co-training**: 联合使用仿真和真实数据
4. **Point Bridge**: 使用点云表示弥合域差距

## 数学形式

Sim-Real Co-training 目标：

$$
\mathcal{L} = \mathcal{L}_{\text{sim}}(\pi; \mathcal{D}_{\text{sim}}) + \lambda \mathcal{L}_{\text{real}}(\pi; \mathcal{D}_{\text{real}})
$$

## 核心要点

1. **Zero-shot**: 仅用仿真数据，直接部署
2. **Co-training**: 结合少量真实数据，提升性能
3. **表示选择**: 选择域不变的观测表示
4. **数据生成**: 大规模仿真数据补充真实数据不足

## 代表工作

- [[SoftMimicGen]]: 生成大规模仿真数据用于迁移
- [[Point Bridge]]: 点云表示用于迁移
- Domain Randomization: 经典方法

## 相关概念

- [[Soft Body Simulation]]: 仿真环境
- [[Imitation Learning]]: 学习框架
- [[Point Bridge]]: 表示方法