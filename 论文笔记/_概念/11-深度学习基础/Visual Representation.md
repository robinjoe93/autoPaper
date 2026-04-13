---
type: concept
aliases: [Visual Representation, 视觉表征]
category: 深度学习基础
---

# Visual Representation

## 定义

从图像中提取的特征表示，用于下游任务（分类、检测、分割、机器人控制等）。通常由预训练视觉模型提供。

## 类型

1. **Global Representation**: 整张图像的单个向量（如 CLIP image embedding）
2. **Dense Representation**: 空间密集特征图（如 DINOv2 patch features）
3. **Task-adaptive vs Task-agnostic**: 是否适应下游任务

## 预训练来源

- **CLIP**: 语言-图像对比学习，适合检索、零样本分类
- **VC-1**: MAE 预训练于 Ego4D 等数据，适合机器人控制
- **DINOv2**: 自监督学习，密集特征强
- **Stable Diffusion**: 扩散模型中间特征，语义丰富

## 在机器人控制中的挑战

- **Task-agnostic 问题**: 不同 representation 在不同任务表现差异大
- **Domain Gap**: 预训练数据（真实世界）与控制环境（模拟）的差异
- **Frozen 问题**: 策略学习中保持冻结，无法适应任务

## 代表工作

- [[ORCA]]: 通过条件化实现 task-adaptive 视觉表征
- [[VC-1]]: 机器人控制的 task-agnostic 视觉表征 baseline
- [[SCR]]: Stable Diffusion 作为视觉表征

## 相关概念

- [[CLIP]]
- [[DINOv2]]
- [[VC-1]]
- [[Task Prompt]]
- [[Visual Prompt]]