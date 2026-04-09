---
type: concept
aliases: [Azure Kinect, Kinect DK]
---

# Azure Kinect DK

## 定义

Azure Kinect DK是微软开发的开发者套件，包含RGB相机、深度传感器和IMU，常用于机器人感知、3D重建和计算机视觉研究。

## 核心要点

1. **传感器配置**:
   - 1MP RGB相机
   - 深度传感器(飞行时间ToF)
   - 6轴IMU
   - 麦克风阵列

2. **深度模式**:
   - NFOV (窄视场)
   - WFOV (宽视场)
   - 点云输出

3. **特点**:
   - 高精度深度测量
   - 支持点云直接输出
   - 与Azure生态集成

4. **应用场景**:
   - 机器人感知
   - 3D扫描
   - 姿态估计
   - 手势识别

## 代表工作

- [[UltraDexGrasp]]: 使用两个Azure Kinect DK作为眼在手上相机，采集场景点云

## 相关概念

- [[Point Cloud]]
- [[RGB-D Camera]]
- [[Depth Estimation]]