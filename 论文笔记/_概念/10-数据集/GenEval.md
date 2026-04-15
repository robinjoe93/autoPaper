---
type: concept
aliases: [GenEval benchmark, compositional evaluation]
---

# GenEval

## 定义

文本到图像生成模型的组合准确性评估基准，测试模型对复杂提示词中多个对象、属性、空间关系的生成能力。

## 评估维度

- **物体计数**: 生成正确数量的对象
- **颜色**: 正确应用颜色属性
- **位置**: 正确的空间布局
- **属性绑定**: 正确关联属性与对象

## 评分形式

$$
\text{GenEval} \in [0, 1]
$$

越高表示组合准确性越好。

## 核心要点

1. **组合测试**: 专注多对象、属性绑定的组合生成
2. **规则评估**: 使用规则检测而非模型评分
3. **难度梯度**: 从单对象到多对象逐步增加难度
4. **标准化**: 成为扩散模型组合能力标准评估

## 代表工作

- [[SOAR]]: GenEval 从 0.70 提升至 0.78（+11% 相对）
- [[Flow-GRPO]]: GenEval 从 0.63 提升至 0.95（原始论文）

## 相关概念

- [[T2I-CompBench]]
- [[DrawBench]]
- [[Text-to-Image]]
- [[Compositional Generation]]