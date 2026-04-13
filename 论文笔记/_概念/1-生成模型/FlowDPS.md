---
type: concept
aliases: [FlowDPS]
---

# FlowDPS

## 定义
一种基于 Flow Matching 的图像编辑方法，提出 latent decomposition 思想，启发 LaS-Comp 的 ERS 双分支设计。

## 核心思想
将生成过程分解为 clean branch 和 noisy branch，分别处理确定性内容和随机性变化。

## 对 LaS-Comp 的启发
ERS 的双分支设计（clean branch + noisy branch）借鉴了 FlowDPS 的 latent decomposition 理念：
- Clean branch: 确保输入 fidelity
- Noisy branch: 引入 stochasticity 增加多样性

## 代表工作
- [[LaS-Comp]]: ERS 设计受 FlowDPS 启发

## 相关概念
- [[ERS]]
- [[Flow Matching]]
- [[Latent Decomposition]]