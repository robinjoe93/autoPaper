---
title: "Camera Reprojection"
tags: [3d-projection, camera-model, scene-reconstruction]
created: 2026-04-03
---

# Camera Reprojection

## 定义

将 3D 场景点投影到 2D 图像平面，考虑相机运动、视差和遮挡的过程。

## 核心公式

$$
\begin{bmatrix}u_{j,t}\\ v_{j,t}\\ 1\end{bmatrix}\sim K\left(R_{t}\,y^{\prime}_{j,t}+t_{t}\right)
$$

## 参数说明

- $u_{j,t}, v_{j,t}$: 投影的 2D 像素坐标
- $K$: 相机内参矩阵
- $R_{t}, t_{t}$: 第 $t$ 帧的相机外参
- $y^{\prime}_{j,t}$: 3D 场景点位置

## 核心作用

- 处理相机运动
- 计算视差效果
- 生成几何一致的渲染

## 应用场景

- [[InsertAnywhere]]: 生成各帧的对象投影
- 3D 渲染与重建
- 视觉里程计

## 关联概念

- [[4D Scene Reconstruction]]
- [[Object Placement]]
- [[Scene Flow Propagation]]