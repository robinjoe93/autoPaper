---
type: concept
aliases: [Di4C, Distillation for Discrete Correlated]
---

# Di4C

## 定义
一种离散扩散蒸馏方法，通过扩展模型输出为混合分布来学习相关性输出，避免因子化分布的局限性。

## 核心要点
1. 认识到因子化输出无法捕获 token 间的相关性
2. 使用混合分布扩展输出空间
3. 混合数量需指数增长才能捕获所有可能的 token 相关性
4. 教师模型使用模拟 Gaussian 破坏过程的离散训练，有天然优势

## 性能表现
- CIFAR10 上教师 40 步达到 FID 8.0
- Di4C 10 步达到 FID 20.6
- 被 [[D-MMD]] 超越（8 步 FID 5.0）

## 代表工作
- Hayakawa et al. (2024): Distillation of discrete diffusion through dimensional correlations
- [[D-MMD]]: 对比实验中作为 baseline

## 相关概念
- [[Discrete Diffusion Models]]
- [[D-MMD]]
- [[SDTT]]