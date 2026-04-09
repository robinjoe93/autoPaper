---
title: "Scene Flow Propagation"
tags: [motion-propagation, scene-flow, object-tracking]
created: 2026-04-03
---

# Scene Flow Propagation

## 定义

使用场景流（3D 运动场）传播对象位置，处理物体间物理交互的方法。

## 核心公式

$$
y^{\prime}_{j,t+1}=y^{\prime}_{j,t}+\mathbf{V}_{t\rightarrow t+1}(y^{\prime}_{j,t})
$$

## 参数说明

- $y^{\prime}_{j,t}$: 第 $t$ 帧对象点位置
- $\mathbf{V}_{t\rightarrow t+1}$: 3D 场景流
- $y^{\prime}_{j,t+1}$: 传播后的下一帧位置

## 处理场景

解决物理交互问题，如：
- 苹果放在盘子上，盘子移动时苹果应跟随
- 对象与周围物体的运动关联

## 实现细节

1. 使用 [[SEA-RAFT]] 计算密集光流
2. 找对象周围 $K$ 个最近 3D 点
3. 聚合局部场景流
4. 更新对象中心位置

## 应用场景

- [[InsertAnywhere]]: 动态对象传播
- 动态场景理解
- 视频对象跟踪

## 关联概念

- [[SEA-RAFT]]
- [[4D-aware Mask Generation]]
- [[Object Placement]]