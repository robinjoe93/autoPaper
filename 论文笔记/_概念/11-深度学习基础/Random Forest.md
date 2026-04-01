---
type: concept
aliases: [随机森林, Random Forest Regression]
---

# Random Forest

## 定义

由多个决策树组成的集成学习方法，通过平均多棵树的预测结果来提高模型稳定性和准确性。

## 数学形式

$$
\hat{y} = \frac{1}{T}\sum_{t=1}^{T} h_t(x)
$$

其中$T$是决策树数量，$h_t$是第$t$棵树的预测。

## 核心要点

1. 处理多变量依赖关系效果好
2. 能识别重要特征
3. 对过拟合有一定抵抗力

## SDQM中的应用

用于整合多个子指标：
- Pearson相关系数达到0.87（最佳）
- 其他回归方法（Linear、Ridge、XGBoost）相关性在0.05范围内

## 代表工作

- Breiman (2001): 原始提出
- [[SDQM]]: 子指标融合方法

## 相关概念

- [[Decision Tree]]
- [[Ensemble Learning]]
- [[XGBoost]]