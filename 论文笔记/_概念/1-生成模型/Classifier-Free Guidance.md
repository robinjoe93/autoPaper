---
type: concept
aliases: [CFG, classifier-free guidance, 无分类器引导]
---

# Classifier-Free Guidance

## 定义

扩散模型中无需外部分类器即可实现条件引导的技术，通过同时训练条件模型和无条件模型，推理时用两者差值引导生成。

## 数学形式

CFG velocity：
$$
v_{cfg} = v_{\text{uncond}} + w_{cfg} (v_{\text{cond}} - v_{\text{uncond}})
$$

引导强度：
$$
w_{cfg} \in [1, \infty]
$$

- $w_{cfg} = 1$: 无引导（仅条件模型）
- $w_{cfg} \to \infty$: 强引导（条件差值主导）

## 核心要点

1. **联合训练**: 同一模型同时学习有条件和无条件预测
2. **无需分类器**: 避免训练额外 classifier，更稳定
3. **引导强度可调**: $w_{cfg}$ 控制条件符合度与多样性权衡
4. **主流方法**: 当前扩散模型标准引导技术
5. **推理应用**: SOAR 使用 CFG 构造偏离轨迹状态

## 代表工作

- [[SOAR]]: 使用 CFG velocity 构造偏离轨迹状态
- Ho & Salimans (2022): Classifier-Free Diffusion Guidance 原始论文

## 相关概念

- [[Diffusion Model]]
- [[Guidance]]
- [[Conditional Generation]]
- [[Prompt Following]]