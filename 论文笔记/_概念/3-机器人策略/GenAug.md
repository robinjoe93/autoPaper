---
type: concept
aliases: [GenAug, 生成式增强]
---

# GenAug

## 定义

GenAug 是一种生成式数据增强方法，通过生成模型将机器人行为迁移到未见过的场景中。

## 核心机制

1. **场景生成**: 使用生成模型创建多样化的场景变体
2. **行为重定向**: 将源演示适应到新场景
3. **视觉增强**: 改变背景、光照、纹理等视觉要素
4. **物体变体**: 生成物体外观和位置的变体

## 核心要点

1. **泛化能力**: 扩展策略对未见场景的泛化
2. **生成式方法**: 使用生成模型而非简单变换
3. **数据效率**: 减少真实数据收集需求
4. **局限**: 主要针对视觉域差距

## 代表工作

- Chen et al. (2023): "GenAug: Retargeting Behaviors to Unseen Situations via Generative Augmentation"
- 与 [[SoftMimicGen]] 对比: SoftMimicGen 生成行为轨迹，GenAug 增强视觉场景

## 相关概念

- [[MimicGen]]: 数据生成系统
- [[SoftMimicGen]]: 可变形物体数据生成
- [[Sim-to-Real Transfer]]: 应用场景