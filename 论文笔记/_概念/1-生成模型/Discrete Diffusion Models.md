---
type: concept
aliases: [离散扩散模型, Discrete Diffusion]
---

# Discrete Diffusion Models

## 定义
一类将扩散模型思想应用于离散数据（如文本、类别标签）的生成模型，通过逐步将数据转化为噪声状态再学习逆向恢复过程。

## 数学形式

扩散过程：$z_t \sim \text{Cat}(\alpha_t x + (1-\alpha_t) \pi)$，其中 $\pi$ 是平稳分布（mask 或 uniform）

## 核心要点
1. 与连续扩散不同，处理离散 token 序列而非连续向量
2. 主要类型包括 [[Masked Diffusion]]（吸收态过程）和 [[Uniform Diffusion]]（均匀转移）
3. 训练目标是学习从噪声状态恢复数据分布的条件分布
4. 采样需要大量迭代步骤（通常 512-1024 步）

## 代表工作
- [[D-MMD]]: 离散扩散模型蒸馏方法
- [[Masked Diffusion]] (Austin et al., 2021): 吸收态扩散
- [[Uniform Diffusion]] (Hoogeboom et al., 2021): 均匀转移扩散
- [[SDTT]]: 渐进蒸馏方法
- [[Di4C]]: 混合分布方法

## 相关概念
- [[Masked Diffusion]]
- [[Uniform Diffusion]]
- [[Diffusion Model]]
- [[Moment Matching Distillation|MMD]]