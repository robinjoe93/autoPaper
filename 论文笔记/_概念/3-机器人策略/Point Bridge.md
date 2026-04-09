---
type: concept
aliases: [Point Bridge]
---

# Point Bridge

## 定义

Point Bridge 是一种用于仿真到真实迁移的表示方法，使用统一的点云表示来弥合仿真与真实世界之间的域差距。

## 核心机制

1. **点云表示**: 使用任务相关物体点作为统一观测
2. **仿真端**: 从 ground-truth mask 和深度图提取点云
3. **真实端**: 使用 VLM-guided pipeline 从 RGB-D 相机提取点云
4. **域无关**: 点云表示减少域差异

## 数学形式

观测表示：

$$
\mathbf{o} = \{(\mathbf{p}_i, \mathbf{c}_i)\}_{i=1}^N
$$

其中 $\mathbf{p}_i$ 是 3D 点位置，$\mathbf{c}_i$ 是颜色/特征。

## 核心要点

1. **几何中心**: 聚焦物体几何而非外观
2. **VLM 引导**: 使用视觉语言模型识别任务相关点
3. **跨域一致**: 仿真与真实使用相同表示
4. **无需在线配准**: 策略直接从点云输出动作

## 代表工作

- Haldar et al. (2026): "Point Bridge: 3D Representations for Cross Domain Policy Learning"
- [[SoftMimicGen]]: 用于真实世界部署

## 相关概念

- [[Sim-to-Real Transfer]]: 应用场景
- [[SoftMimicGen]]: 配合使用
- [[Imitation Learning]]: 学习框架