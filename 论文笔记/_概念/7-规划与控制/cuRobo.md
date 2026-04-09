---
type: concept
aliases: [cuRobo, CUDA Robotics]
---

# cuRobo

## 定义

cuRobo是一个GPU加速的机器人运动规划库，提供并行化的碰撞检测、逆运动学求解和轨迹优化功能，专为实时机器人应用设计。

## 核心要点

1. **GPU并行化**: 所有核心算法在GPU上并行执行，速度提升显著
2. **主要功能**:
   - 碰撞检测和距离计算
   - 逆运动学(IK)求解
   - 最小Jerk轨迹优化
   - 运动规划

3. **应用场景**:
   - 实时运动规划
   - 抓取可行性分析
   - 轨迹优化

4. **特点**:
   - 支持任意机器人URDF
   - 有符号距离场(SDF)表示
   - 可微分，支持梯度优化

## 代表工作

- [[UltraDexGrasp]]: 使用cuRobo进行IK分析、碰撞检测和运动规划

## 相关概念

- [[Motion Planning]]
- [[Inverse Kinematics]]
- [[Grasp Synthesis]]