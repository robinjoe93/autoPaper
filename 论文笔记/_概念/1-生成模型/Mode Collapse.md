---
type: concept
aliases: [模式坍塌, Mode Dropping]
---

# Mode Collapse

## 定义

生成模型只捕获真实数据分布的部分模态，导致生成样本多样性不足的现象。

## 表现

- 生成样本覆盖真实数据分布的部分区域
- 不同输入产生相似的输出
- Beta-Recall指标值较低

## 核心要点

1. GAN训练中的常见问题
2. 可通过改进架构、正则化缓解
3. [[Beta-Recall]]可量化检测mode collapse程度

## 代表工作

- [[SDQM]]: Beta-Recall用于检测多样性

## 相关概念

- [[GAN]]
- [[Beta-Recall]]
- [[Alpha-Precision]]