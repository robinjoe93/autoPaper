---
type: concept
aliases: [抓取合成, Grasp-Pose Generation]
---

# Grasp Synthesis

## 定义

抓取合成（Grasp Synthesis）是指给定物体几何形状，自动计算机器人手相对于物体的位姿（位置和姿态），使得抓取稳定、可行且适合后续任务的过程。

## 数学形式

一般形式化为优化问题：

$$
\boldsymbol{g}^* = \arg\min_{\boldsymbol{g}} \mathcal{L}(\boldsymbol{g}) \quad \text{s.t.} \quad \boldsymbol{c}(\boldsymbol{g}) \leq 0
$$

其中：
- $\boldsymbol{g}$: 抓取位姿
- $\mathcal{L}$: 目标函数（如力闭合质量、接触相似度）
- $\boldsymbol{c}$: 约束（如碰撞、关节限制、可达性）

## 核心要点

1. **三大方法类别**:
   - 采样-based: 在位姿空间随机采样，评估质量
   - 优化-based: 通过梯度下降优化目标函数
   - 学习-based: 训练神经网络预测抓取位姿

2. **关键约束**:
   - [[Force Closure]]: 抓取稳定性
   - 运动学可达性: IK解存在
   - 碰撞避免: 手不与环境碰撞
   - 关节限制: 在关节限位内

3. **评估指标**:
   - Force closure margin
   - Grasp wrench space volume
   - 任务-specific指标

## 代表工作

- [[UltraDexGrasp]]: 双层优化框架，底层QP优化接触力，顶层优化手部位姿
- [[DexGraspNet]]: 可微分力闭合估计器优化抓取
- [[BODex]]: 双层优化抓取合成

## 相关概念

- [[Force Closure]]
- [[Inverse Kinematics]]
- [[Motion Planning]]