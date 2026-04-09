---
type: concept
aliases: [科学任务长度, 任务复杂度框架, Ltask]
---

# Scientific Task Length

## 定义

ASI-Evolve提出的三维框架，用于系统刻画科研任务的内在复杂度，区分不同自动化系统的能力边界。

## 数学形式

任务复杂度定义：

$$
L_{task} = \langle C_{exec}, S_{space}, D_{feedback} \rangle
$$

**符号说明**:
- $C_{exec}$: 执行成本（计算资源、代码库复杂度）
- $S_{space}$: 搜索空间复杂度（目标开放性、边界预定义度）
- $D_{feedback}$: 反馈复杂度（多维信号综合难度）

## 核心要点

1. **$C_{exec}$**: GPU小时、代码修改范围、工程复杂度
2. **$S_{space}$**: 目标是否开放、解空间边界是否给定
3. **$D_{feedback}$**: 单scalar vs多维日志信号需综合解读
4. **定位系统**: 用此框架对比现有系统(SciMaster低, AlphaEvolve中, ASI-Evolve高)

## 代表工作

- [[ASI-Evolve]]: 论文核心概念框架

## 相关概念

- [[Evolutionary Agent]]
- [[Cognition Base]]