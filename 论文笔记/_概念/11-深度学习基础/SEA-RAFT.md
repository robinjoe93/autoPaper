---
title: "SEA-RAFT"
tags: [optical-flow, motion-estimation, video-processing]
created: 2026-04-03
---

# SEA-RAFT

## 定义

一种高效的稠密光流估计方法，用于计算视频帧之间的像素对应关系。

## 核心特点

- 稠密光流估计
- 高精度、高效率
- 适用于 4D 场景重建

## 应用场景

- [[InsertAnywhere]]: 用于 Scene Flow based Object Propagation
- [[Uni4D]]: 作为运动估计组件
- 视频编辑与特效
- 动态场景理解

## 技术细节

计算密集光流场 $\mathbf{V}_{t\rightarrow t+1}$，表示帧 $t$ 到帧 $t+1$ 的像素位移。

## 关联概念

- [[Optical Flow]]
- [[4D Scene Reconstruction]]
- [[Scene Flow Propagation]]