---
title: MMLU
tags: [benchmark, multitask, evaluation]
created: 2026-04-03
---

# MMLU

## 定义

MMLU (Massive Multitask Language Understanding) 是一个大规模多任务语言理解基准，涵盖 57 个学科领域的多项选择题。

## 特点

- **规模**: 约 16K 测试题目
- **领域**: STEM、人文、社科等 57 个学科
- **格式**: 四选一多项选择题
- **难度**: 从基础到专业水平

## 变体

- **Global MMLU**: 多语言版本
- **MMLU-Pro**: 增强难度版本

## 在 Simula 中的应用

[[Simula]] 使用 Global MMLU 验证多语言合成数据：
- 生成多语言选择题数据
- 在 Nepali、Korean 等语言上测试
- 合成数据在某些子集上优于真实数据

## 评估指标

准确率（Accuracy）

## 相关概念

- [[GSM8K]]
- [[Simula]]
- [[Synthetic Data]]