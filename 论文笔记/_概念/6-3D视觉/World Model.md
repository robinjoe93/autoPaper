---
title: World Model
created: 2026-04-20
tags: [3d-vision, generative-model, simulation, robotics]
---

# World Model

## 定义

世界模型（World Model）是一种能够模拟、理解和预测复杂3D环境的AI系统。它捕获真实世界的物理和空间动力学，使智能体能够在虚拟环境中进行交互和规划。

## 核心特征

1. **环境模拟**: 能够生成或重建3D场景表示
2. **空间理解**: 理解场景的几何结构和语义信息
3. **交互能力**: 支持智能体在虚拟世界中的导航和操作
4. **预测能力**: 能够预测环境状态的变化

## 应用领域

- **虚拟现实/增强现实**: 生成沉浸式虚拟环境
- **机器人仿真**: 提供机器人训练和测试环境
- **游戏开发**: 自动生成游戏场景
- **自动驾驶**: 模拟驾驶场景进行规划

## 主要范式

### 离线3D世界模型
- 生成可导航的3D场景（如 [[HY-World]]）
- 显式建模3D结构，与标准CG流程兼容

### 在线视频世界模型
- 实时交互式视频生成（如 [[HY-World 1.5]]）
- 由用户动作驱动的实时世界建模

## 代表工作

- [[HY-World]]: 多模态世界模型，统一生成与重建
- [[HY-World 1.5]]: 实时交互世界建模
- [[Marble]]: 闭源商业世界模型
- [[WorldStereo]]: 相机引导视频生成
- [[WorldMirror]]: 统一3D重建模型

## 相关概念

- [[3D Gaussian Splatting]]: 世界模型的输出表示
- [[Diffusion Model]]: 生成核心技术
- [[Video Diffusion Model]]: 视频生成基础
- [[3D Foundation Model]]: 3D理解基础模型

## 参考文献

- Ha & Schmidhuber (2018): "World Models"
- HY-World Team (2026): "HY-World 2.0"