---
type: concept
aliases: [SMPL-X, SMPLX, 扩展 SMPL]
---

# SMPL-X

## 定义

统一的人体参数化模型，扩展了 SMPL 以同时建模身体、手部和面部，提供完整的人体几何表示。

## 数学形式

参数化表示：

$$
\theta \in \mathbb{R}^{55 \times 3}, \quad \beta \in \mathbb{R}^{10}, \quad \psi \in \mathbb{R}^{10}
$$

**符号说明**:
- $\theta$: 姿态参数（身体 21 + 左手 15 + 右手 15 + 下颌 1 + 眼球 3 = 55 个关节）
- $\beta$: 形状参数（10 维 PCA 系数）
- $\psi$: 面部表情参数（10 维）

3D 关键点回归：

$$
J(\beta) = J_{\text{template}} + W_s \beta
$$

$$
R_\theta(J(\beta)) = \text{KinematicTree}(\theta, J(\beta))
$$

## 核心要点

1. **统一建模**: 身体 + 双手 + 面部一体化，解决之前分建模的不一致问题
2. **参数紧凑**: 仅需 55×3+10+10=175 个参数即可表示完整人体
3. **兼容性**: 与 SMPL 兼容，可使用现有 SMPL 工具和数据
4. **应用广泛**: 人体重建、姿态估计、动画、VR/AR

## 代表工作

- [[PoseDreamer]]: 使用 SMPL-X 作为 3D 人体标注的统一格式
- SMPL-X: SMPL eXpressive (Pavlakos et al., 2019)

## 相关概念

- [[SMPL]]
- [[SMPLer-X]]
- [[Object Keypoint Similarity]]