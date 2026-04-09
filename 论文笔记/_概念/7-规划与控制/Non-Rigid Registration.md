---
type: concept
aliases: [非刚性配准, Non-Rigid Registration, Deformable Registration]
---

# Non-Rigid Registration

## 定义

非刚性配准是一种点集匹配技术，用于建立两个具有不同形状的点集之间的对应关系，允许点集之间存在非线性变形。

## 数学形式

给定源点集 $O = \{\mathbf{u_i}\}_{i=1}^{N_O}$ 和目标点集 $O' = \{\mathbf{v_j}\}_{j=1}^{N_{O'}}$，找到变形函数 $\mathbf{f}$：

$$
\mathbf{f}^* = \arg\min_\mathbf{f} \mathcal{C}(O, \mathbf{f}(O'), O')
$$

其中 $\mathcal{C}$ 是配准代价函数。

## 变形变换公式

$$
p_{t}\rightarrow\mathbf{f}(p_{t}),\quad R_{t}\rightarrow\mathrm{orth}(\mathbf{J_{f}}(p_{t})R_{t})
$$

**符号说明**:
- $p_t$: 位置点
- $R_t$: 姿态矩阵
- $\mathbf{f}$: 变形函数
- $\mathbf{J_f}$: Jacobian 矩阵
- $\mathrm{orth}(\cdot)$: 正交化操作

## 核心要点

1. **非线性变换**: 允许弹性变形，而非仅刚性变换
2. **点集对应**: 建立源点与目标点的匹配关系
3. **优化求解**: 通过迭代优化找到最佳变形函数
4. **应用广泛**: 医学图像配准、机器人轨迹转移

## 在 SoftMimicGen 中的应用

用于建立源演示中可变形物体与新场景中物体的对应关系，实现轨迹转移。

## 代表工作

- Chui & Rangarajan (2003): "A New Point Matching Algorithm for Non-Rigid Registration"
- [[SoftMimicGen]]: 用于可变形物体操作的数据生成

## 相关概念

- [[Deformable Object Manipulation]]: 应用场景
- [[SoftMimicGen]]: 核心技术组件
- Rigid Registration: 对比概念