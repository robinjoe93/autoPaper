---
title: "Abstract Sim2Real through Approximate Information States"
method_name: "ASTRA"
authors: [Yunfu Deng, Yuhao Li, Josiah P. Hanna]
year: 2026
venue: arXiv
tags: [sim2real, state-abstraction, reinforcement-learning, robot-navigation, simulator-grounding, approximate-information-state]
zotero_collection: 3-机器人策略
image_source: online
arxiv_html: https://arxiv.org/html/2604.15289v1
created: 2026-04-20
---

# 论文笔记：Abstract Sim2Real through Approximate Information States

## 元信息

| 项目 | 内容 |
|------|------|
| 机构 | University of Wisconsin-Madison, University of Massachusetts Amherst |
| 日期 | April 2026 |
| 领域 | Robotics (cs.RO) |
| 链接 | [arXiv](https://arxiv.org/abs/2604.15289) |

---

## 一句话总结

> 提出抽象 sim2real 问题的形式化定义，通过 [[Approximate Information State|近似信息状态]] 学习历史依赖的模拟器修正，实现从粗粒度抽象模拟器到真实世界的策略迁移。

---

## 核心贡献

1. **抽象 Sim2Real 形式化**: 首次使用 RL 状态抽象理论形式化定义抽象 sim2real 问题，揭示抽象诱导的部分可观测性是核心挑战
2. **历史依赖的模拟器修正**: 提出基于历史的模拟器修正方法，通过 [[GRU]] 处理状态-动作历史序列以缓解部分可观测问题
3. **自预测表征学习**: 引入 [[Self-Predictive Representation|自预测表征]] 约束，使隐状态包含任务相关信息，显著优于仅优化预测准确率的方法

---

## 问题背景

### 要解决的问题

传统 sim2real 方法假设模拟器与目标域共享相同的状态-动作空间，仅动力学参数不同（参数化不匹配）。但实际中构建高保真模拟器困难，常需使用**抽象模拟器**——状态空间被有意缩减的简化模拟器。核心问题是：如何在抽象模拟器中训练策略并成功迁移到真实世界？

### 现有方法的局限

1. **参数化方法失效**: [[Domain Randomization|域随机化]]、系统辨识等方法处理参数化不匹配，但无法处理状态空间结构差异
2. **Markov 假设失效**: 抽象诱导部分可观测性，使得 $\Pr(s_{i+1}^s|s_i^s,a_i) \neq \Pr(s_{i+1}^s|s_i^s,a_i,s_{i-1}^s,a_{i-1})$，标准修正目标定义不清
3. **信息缺失**: 仅优化下一抽象状态预测准确率，不保证隐状态包含任务关键信息

### 本文的动机

抽象模拟器虽简化建模但忽略关键动力学细节。作者发现：若修正函数考虑状态-动作历史，可隐式捕获被抽象掉的变量影响。基于 [[Approximate Information State|AIS]] 理论，设计包含奖励预测和自预测约束的多目标学习框架。

---

## 方法详解

### 模型架构

ASTRA 采用 **隐空间动力学修正** 架构：
- **输入**: 抽象状态-动作历史 $h_i^s = (s_1^s, a_1, ..., s_i^s, a_{i-1})$
- **Backbone**: 共享 [[GRU]] 循环网络，输出隐状态 $z_i^s$
- **核心模块**:
  - [[Source History Encoder|源历史编码器]] $\psi^s: \mathcal{H}^s \rightarrow \mathcal{Z}$
  - [[Latent Dynamics Model|潜态动力学模型]] $P_{\text{lat}}: \mathcal{Z} \times \mathcal{A} \rightarrow \Delta(\mathcal{Z})$
  - [[Reward Predictor|奖励预测器]] $R_{\text{pred}}: \mathcal{Z} \times \mathcal{A} \rightarrow \mathbb{R}$
  - [[Abstract State Predictor|抽象状态预测器]] $f_{\text{abs}}: \mathcal{Z} \times \mathcal{A} \times \mathcal{S}^s \rightarrow \tilde{\mathcal{S}}^s$
- **输出**: 修正后的抽象下一状态 $\hat{s}_{i+1}^s$、预测奖励 $\hat{r}_i$、潜态转移 $z_{i+1}^s$

### 核心模块

#### 模块1: 历史编码器 $\psi^s$

**设计动机**: 将抽象历史映射到隐空间，捕获被抽象掉的动力学信息

**具体实现**:
- 使用 [[GRU]] 处理历史序列 $(s_1^s, a_1, ..., s_i^s, a_{i-1})$
- 输出隐状态 $z_i^s = \psi^s(\Phi(h_i^t))$，其中 $\Phi$ 将真实历史投影到抽象历史

#### 模块2: 潜态动力学模型 $P_{\text{lat}}$

**设计动机**: 预测隐状态转移，形成 Markov 隐空间

**具体实现**:
- 输出 Gaussian 参数 $(\mu_i, \log\sigma_i^2)$
- 满足 AIS 条件: $P(z_{i+1}^s|\Phi(h_i^t),a_i) \approx P(z_{i+1}^s|z_i^s,a_i)$

#### 模块3: 奖励预测器 $R_{\text{pred}}$

**设计动机**: 确保隐状态包含任务相关信息

**具体实现**:
- 从隐状态预测真实奖励: $\mathbb{E}[r_i^t|\Phi(h_i^t),a_i] \approx R_{\text{pred}}(z_i^s, a_i)$

#### 模块4: 抽象状态预测器 $f_{\text{abs}}$

**设计动机**: 修正模拟器状态转移以匹配真实世界

**具体实现**:
- 接收隐状态、动作和模拟器原始预测 $s_{i+1}^s$
- 输出修正状态 $\hat{s}_{i+1}^s = f_{\text{abs}}(z_i^s, a_i, s_{i+1}^s)$

---

## 关键公式

### 公式1: [[Approximate Information State|总训练目标]]

$$
\mathcal{L} = \lambda_1 \mathcal{L}_{\text{trans}} + \lambda_2 \mathcal{L}_{\text{rew}} + \lambda_3 \mathcal{L}_{\text{abs}}
$$

**含义**: 联合优化潜态动力学预测、奖励预测和抽象状态修正

**符号说明**:
- $\lambda_1, \lambda_2, \lambda_3$: 各损失权重系数
- $\mathcal{L}_{\text{trans}}$: 潜态动力学自预测损失
- $\mathcal{L}_{\text{rew}}$: 奖励预测损失
- $\mathcal{L}_{\text{abs}}$: 抽象状态修正损失

### 公式2: [[Self-Predictive Representation|潜态动力学损失]]

$$
\mathcal{L}_{\text{trans}} = -\sum_{i} \log \mathcal{N}(z_{i+1}^s \mid \mu_i, \sigma_i)
$$

**含义**: 使潜态动力学模型能够准确预测下一隐状态分布

**符号说明**:
- $z_{i+1}^s = \psi^s(\Phi(h_{i+1}^t))$: 下一步的真实隐状态编码
- $(\mu_i, \sigma_i)$: $P_{\text{lat}}(z_i^s, a_i)$ 输出的 Gaussian 参数

### 公式3: [[Reward Prediction|奖励预测损失]]

$$
\mathcal{L}_{\text{rew}} = \sum_{i} \|R_{\text{pred}}(z_i^s, a_i) - r_i^t\|^2
$$

**含义**: 确保隐状态包含足够预测奖励的信息（AIS 核心条件之一）

**符号说明**:
- $R_{\text{pred}}(z_i^s, a_i)$: 从隐状态预测的奖励
- $r_i^t$: 真实环境中的实际奖励

### 公式4: [[Abstract State Correction|抽象状态修正损失]]

$$
\mathcal{L}_{\text{abs}} = \sum_{i} \|f_{\text{abs}}(z_i^s, a_i, s_{i+1}^s) - \phi(s_{i+1}^t)\|^2
$$

**含义**: 修正抽象模拟器状态转移以匹配真实世界观测

**符号说明**:
- $f_{\text{abs}}$: 状态修正函数
- $s_{i+1}^s$: 模拟器原始预测
- $\phi(s_{i+1}^t)$: 真实状态经抽象映射后的状态

### 公式5: [[Encoder Alignment|编码器对齐损失]]

$$
\mathcal{L}_{\text{align}} = D(p_i^s, p_i^t)
$$

**含义**: 使目标编码器 $\psi^t$ 与源编码器 $\psi^s$ 在相同动作下产生相似的潜态分布

**符号说明**:
- $p_i^s := P(z_{i+1}^s|z_i^s, a_i)$: 源编码器潜态转移分布
- $p_i^t := P(z_{i+1}^t|z_i^t, a_i)$: 目标编码器潜态转移分布
- $D$: [[Maximum Mean Discrepancy|MMD]] 分布距离度量

---

## 关键图表

### Figure 1: 抽象模拟器与真实机器人轨迹对比

![Figure 1](https://arxiv.org/html/2604.15289v1/2604.15289v1/figs/description.png)

**说明**: 相同速度命令在点质量模拟器中产生完美跟踪（蓝色），但在真实 NAO 机器人上产生显著偏差（橙色），展示抽象动力学与真实动力学之间的差距。

### Figure 2: PointMaze 到 AntMaze 迁移轨迹

![Figure 2](https://arxiv.org/html/2604.15289v1/2604.15289v1/figs/killer.png)

**说明**: 展示不同方法从点质量模拟器（抽象）到四足机器人 AntMaze（目标）的迁移效果。左侧无修正策略在拐角处碰撞；中间 NAS 通过历史处理学习安全导航；右侧 ASTRA 进一步识别高风险区域并成功导航。

### Figure 3: 导航任务成功率

![Figure 3](https://arxiv.org/html/2604.15289v1/2604.15289v1/results/ral_antmaze_result.png)

**说明**: U-Maze（左）和 Long Maze（右）任务成功率。ASTRA 在两种设置均达最高成功率，显著优于 [[Domain Randomization|DR]]、[[COMPASS]] 和 [[Neural-Augmented Simulation|NAS]]。

### Figure 4: Humanoid 抽象层级

![Figure 4](https://arxiv.org/html/2604.15289v1/2604.15289v1/figs/humanoid_levels.png)

**说明**: 三种抽象层级：Walker2D（最抽象，仅建模腿部）、Kinematics（保留完整形态）、Extended Kinematics（增加 CoM 信息）。

### Figure 5: Humanoid 仿人运动结果

![Figure 5](https://arxiv.org/html/2604.15289v1/2604.15289v1/figs/humanoid_final.png)

**说明**: 三种抽象层级下的仿人运动性能。抽象程度越高，ASTRA 相对 NAS 的优势越明显，验证自预测约束在严重抽象场景下的重要性。

### Figure 6: 数据效率分析

![Figure 6](https://arxiv.org/html/2604.15289v1/2604.15289v1/figs/data.png)

**说明**: 随数据规模增长，ASTRA 在 75%-100% 数据区间提升最显著，超过 100% 后收益递减，表明约 200 轨迹足以覆盖状态空间。

### Table 1: NAO 机器人真实世界评估

| Method | Navigation Success | Navigation Distance (m) | Ball-Kicking Success |
|--------|-------------------|------------------------|---------------------|
| DT | 0.27 +/- 0.21 | 10.91 | 0.07 +/- 0.03 |
| DR | 0.33 +/- 0.31 | 9.30 | 0.12 +/- 0.04 |
| RMA | 0.51 +/- 0.17 | 10.33 | 0.10 +/- 0.01 |
| DT+IQL | 0.31 +/- 0.10 | 10.72 | 0.08 +/- 0.01 |
| IQL | 0.17 +/- 0.14 | 9.77 | 0.05 +/- 0.03 |
| COMPASS | 0.50 +/- 0.08 | 12.70 | 0.40 +/- 0.25 |
| NAS | 0.53 +/- 0.06 | 12.41 | 0.37 +/- 0.22 |
| **ASTRA** | **0.73 +/- 0.05** | 12.33 | **0.56 +/- 0.05** |

**说明**: NAO 导航任务 ASTRA 达 73% 成功率，显著优于所有基线（17%-53%）；踢球任务达 56% 成功率。距离指标显示 ASTRA 学习更保守的策略以提高成功率。

### Table 2: 组件消融实验

| Configuration | Success Rate |
|---------------|-------------|
| Full ASTRA | 0.40 +/- 0.05 |
| w/o dynamics (L_trans) | 0.35 +/- 0.04 |
| w/o reward (L_rew) | 0.29 +/- 0.10 |
| NAS | 0.27 +/- 0.06 |

**关键发现**: 奖励预测损失贡献最大（移除后成功率从 0.40 降至 0.29 且方差翻倍），动力学预测损失提供辅助正则化。Full ASTRA vs NAS (0.40 vs 0.27) 验证自预测约束的关键作用。

---

## 实验

### 数据集/任务

| 任务 | 抽象模拟器 | 目标环境 | 数据规模 |
|------|-----------|---------|---------|
| AntMaze Navigation | 2D PointMaze | AntMaze (四足机器人) | 200 trajectories |
| Humanoid Locomotion | Walker2D/Kinematics/ExtKin | Humanoid (仿人机器人) | 200 trajectories |
| NAO Navigation | 2D Point Mass | NAO 双足机器人 | 50 -> 200 (增强) |
| NAO Ball-Kicking | 2D Point + Ball | NAO 双足机器人 | 200 trajectories |

### 实现细节

- **Backbone**: 共享 [[GRU]] 循环网络，三个任务特定输出头
- **RL算法**: 导航任务使用 [[PPO]]，仿人运动和 NAO 使用 [[SAC]]
- **训练方式**: 联合优化所有参数，先训练 $\psi^s$ 再冻结并训练 $\psi^t$
- **硬件**: 未明确说明

### 主要实验结论

1. **历史依赖必要性**: NAS 通过历史处理优于 DR/COMPASS，验证部分可观测性需历史信息
2. **自预测约束重要性**: ASTRA vs NAS 差距验证奖励预测和潜态动力学约束的关键作用
3. **抽象程度影响**: 抽象越严重，ASTRA 相对优势越大；高保真模拟器下差距缩小
4. **形态鲁棒性**: 在腿部长度放大 1.25 倍的变异 Ant 上，ASTRA 达 65% 成功率（vs 直接迁移 21%）
5. **数据效率**: 100% 数据量（200 轨迹）足以覆盖状态空间，增加数据收益递减

---

## 批判性思考

### 优点

1. **理论贡献**: 首次形式化抽象 sim2real 问题，揭示部分可观测性核心挑战
2. **方法创新**: 基于 AIS 的自预测约束设计，显式塑造任务相关隐状态
3. **实验全面**: 包含 sim2sim 和真实机器人验证，多抽象层级对比
4. **实用价值**: 降低模拟器开发成本，使粗粒度抽象模拟器可用于策略学习

### 局限性

1. **抽象程度限制**: 过度抽象时历史依赖可能仍不足，需研究可接受抽象阈值
2. **状态映射假设**: 需已知 $\phi: \mathcal{S}^t \rightarrow \mathcal{S}^s$，限制应用于原始传感器而非高层变量
3. **数据需求**: 仍需非 trivial 真实数据（~200 轨迹），虽少于直接策略学习
4. **共享动作空间**: 假设抽象与真实环境共享动作空间，实际常需层次映射

### 潜在改进方向

1. **自适应抽象**: 自动识别可接受抽象程度，避免过度抽象失效
2. **端到端学习**: 学习 $\phi$ 映射，减少对现有感知/状态估计系统的依赖
3. **主动数据收集**: 优化数据采集策略，减少真实世界轨迹需求
4. **多保真融合**: 结合不同保真度模拟器，利用互补信息

### 可复现性评估

- [ ] 代码开源（论文未提及）
- [ ] 预训练模型
- [x] 训练细节完整（算法、损失函数、超参）
- [x] 实验设置清晰（任务、基线、数据规模）
- [x] 真实机器人验证

---

## 关联笔记

### 基于

- [[Neural-Augaged Simulation|NAS]] (Golemo et al., 2018): 历史依赖修正的思想基础
- [[Approximate Information State|AIS]] (Subramanian et al., 2022): 隐状态设计理论依据
- [[Self-Predictive Representation]]: 自预测约束设计来源
- [[State Abstraction]] (Abel, 2022): 抽象 sim2real 形式化理论基础

### 对比

- [[Domain Randomization|DR]]: 处理参数化不匹配，无法处理抽象问题
- [[COMPASS]]: 强域随机化方法，在抽象场景失效
- [[RMA|Rapid Motor Adaptation]]: 针对参数化不确定性设计，无法处理抽象诱导的部分可观测
- [[IQL]]: 直接在目标域训练，数据效率低且性能不稳定

### 方法相关

- [[Sim2Real]]: 核心研究领域
- [[State Abstraction]]: 状态空间缩减理论
- [[Partial Observability]]: 抽象诱导的核心挑战
- [[GRU]]: 处理历史序列的循环网络
- [[PPO]] / [[SAC]]: 使用的 RL 算法

### 硬件/数据相关

- [[NAO Robot]]: 真实机器人验证平台
- [[AntMaze]]: 四足导航任务
- [[Humanoid]]: 仿人运动任务

---

## 速查卡片

> [!summary] ASTRA: Abstract Sim2Real through AIS
> - **核心**: 历史依赖 + 自预测约束的抽象模拟器修正
> - **方法**: 基于 AIS 的潜态动力学 + 奖励预测 + 状态修正
> - **结果**: NAO 导航 73% 成功率，踢球 56%，优于 NAS/COMPASS/DR
> - **理论**: 首次形式化抽象 sim2real，揭示部分可观测性挑战

---

*笔记创建时间: 2026-04-20*