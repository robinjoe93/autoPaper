---
type: concept
aliases: [AutoML, Auto Keras]
---

# AutoKeras

## 定义

开源AutoML工具，自动选择和训练神经网络架构，简化深度学习模型开发流程。

## 核心要点

1. 自动搜索最佳神经网络架构
2. 支持图像分类、文本分类等任务
3. SDQM中用于Dataset Separability子指标

## SDQM中的应用

在Dataset Separability计算中：
- 自动选择网络架构
- 训练分类器区分真实/合成嵌入
- 记录验证准确率和参数数量

## 代表工作

- [[SDQM]]: Dataset Separability组件

## 相关概念

- [[AutoML]]
- [[Neural Architecture Search]]
- [[NAS]]