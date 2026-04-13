---
type: concept
aliases: [形状补全, 3D Shape Completion, Point Cloud Completion]
---

# Shape Completion

## 定义
从部分观测的 3D 点云或网格重建完整 3D 形状的任务。

## 数学形式

输入：Partial 点云 $\boldsymbol{S}_p \in \mathbb{R}^{k \times 3}$

输出：Complete 点云 $\boldsymbol{S}_c \in \mathbb{R}^{N \times 3}$

目标：$\boldsymbol{S}_c$ 在观测区域与 $\boldsymbol{S}_p$ 一致，缺失区域合理补全

## 核心要点
1. **Partial Patterns 多样**：单视角扫描、随机裁剪、语义部件缺失
2. **挑战**：需跨类别泛化、无需配对数据、处理边界不连续
3. **评估指标**：CD（Chamfer Distance）、EMD（Earth Mover's Distance）、Fidelity

## 代表工作
- [[LaS-Comp]]: Zero-shot，利用 3D foundation model 先验
- [[SDS-Complete]]: 2D prior + SDS 优化
- [[ComPC]]: 3D Gaussians + Zero-1-to-3
- SVDFormer、AdaPoinTr: 监督学习方法

## 相关概念
- [[3D Foundation Model]]
- [[Point Cloud]]
- [[Chamfer Distance]]