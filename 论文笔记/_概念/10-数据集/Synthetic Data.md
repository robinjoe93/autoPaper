---
type: concept
aliases: [合成数据, Simulated Data]
---

# Synthetic Data

## 定义

通过物理仿真、渲染引擎或生成式AI模型创建的人工数据，用于替代或补充真实训练数据。

## 类型

1. **物理仿真数据**: Unity、Unreal Engine等渲染
2. **生成式AI数据**: 扩散模型、GAN生成
3. **混合数据**: 真实+合成组合

## 核心要点

1. 解决真实数据稀缺、标注昂贵的问题
2. 物理仿真具有可重复性、可追溯性
3. 需要评估与真实数据的域差距

## 代表工作

- [[SDQM]]: 合成数据质量评估方法
- [[RarePlanes]]: 航空图像合成数据集
- [[DIMO]]: 工业物体合成数据集

## 相关概念

- [[Domain Gap]]
- [[Domain Adaptation]]
- [[Data Augmentation]]