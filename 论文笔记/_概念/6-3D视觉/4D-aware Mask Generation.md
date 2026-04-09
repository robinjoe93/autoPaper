---
title: "4D-aware Mask Generation"
tags: [mask-generation, 4d-geometry, occlusion-aware]
created: 2026-04-03
---

# 4D-aware Mask Generation

## 定义

利用 4D 场景几何重建生成时间一致、遮挡正确的对象 mask 序列的方法。

## 核心步骤

1. **4D Scene Reconstruction**: 重建场景几何和运动
2. **User-Controlled Placement**: 用户指定首帧位置/尺度
3. **Scene Flow Propagation**: 基于光流传播对象运动
4. **Camera-Aligned Reprojection**: 投影到各帧并分割

## 关键创新

- 处理复杂遮挡场景
- 时间一致的 mask 序列
- 用户可控的几何放置

## 应用

- [[InsertAnywhere]]: Stage 1 的核心模块
- 视频对象跟踪
- 视频编辑

## 相关公式

对象点云变换: $y^{\prime}_{j}=s_{\text{obj}}\,R_{\text{obj}}\,y_{j}+t_{\text{obj}}$

场景流传播: $y^{\prime}_{j,t+1}=y^{\prime}_{j,t}+\mathbf{V}_{t\rightarrow t+1}(y^{\prime}_{j,t})$

## 关联概念

- [[4D Scene Reconstruction]]
- [[Uni4D]]
- [[SAM2]]
- [[SEA-RAFT]]