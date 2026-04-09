---
type: concept
aliases: [SMPLer-X, SMPL 估计器]
---

# SMPLer-X

## 定义

基于 Transformer 的 SMPL-X 参数回归模型，从单张 RGB 图像预测完整的人体、手部和面部 3D 参数。

## 核心要点

1. **架构**: ViT backbone + SMPL-X 参数回归头
2. **输入**: 单张人体图像的 crop
3. **输出**: $\theta, \beta, \psi$ 完整 SMPL-X 参数
4. **多规模**: 提供 ViT-S、ViT-L 等不同 backbone 版本
5. **训练**: 在大规模合成/真实数据集上训练

## 代表工作

- [[PoseDreamer]]: 使用 SMPLer-X 作为下游 mesh recovery 模型，验证生成数据质量

## 相关概念

- [[SMPL-X]]
- [[ViT]]
- [[Human Mesh Recovery]]