---
type: concept
aliases: [Visual Prompt, 视觉提示]
category: 深度学习基础
---

# Visual Prompt

## 定义

ORCA 提出的可学习 prompt，将视觉编码器的 dense features 转换为条件 tokens。用于捕获帧级细粒度视觉细节。

## 核心要点

1. **来源**: DINOv2 dense features
2. **处理**: 通过小卷积层压缩为 tokens
3. **数量**: 通常 16 个 tokens ($l_v=16$)
4. **帧级**: 每帧独立计算，捕获动态细节

## 数学形式

Visual prompt 生成：

$$
p_v = \text{Conv}(\mathcal{E}_V(I))
$$

**符号说明**:
- $\mathcal{E}_V$: DINOv2 visual encoder
- $I$: 当前帧图像
- Conv: 压缩卷积层

## 工作原理

- DINOv2 提取空间密集特征
- 卷积层压缩为固定长度 tokens
- 与 task prompt 一起输入 text encoder
- Cross-attention maps 捕获细粒度区域（如机械臂不同部位）

## 与 global representation 的对比

| 类型 | 信息量 | 空间精度 |
|------|--------|----------|
| Global (如 TADP $S^*$) | 单向量 | 无空间信息 |
| Dense visual prompt | 空间特征 | 高空间精度 |

## 代表工作

- [[ORCA]]: 提出 visual prompt 捕获 frame-specific 细节
- Exploring Conditions for Diffusion Models in Robotic Control (Shin et al., 2026)

## 相关概念

- [[DINOv2]]
- [[Task Prompt]]
- [[Cross-Attention]]
- [[TADP]]