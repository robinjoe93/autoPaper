---
title: "Hyperagents"
method_name: "Hyperagents"
authors: [Jenny Zhang, Bingchen Zhao, Wannan Yang, Jakob Foerster, Jeff Clune, Minqi Jiang, Sam Devlin, Tatiana Shavrina]
year: 2026
venue: arXiv
tags: [self-improving-ai, open-endedness, meta-learning, foundation-models, recursive-self-improvement, agent-systems, ai-safety]
zotero_collection: 1-AI/1-Agent
image_source: local
created: 2026-04-13
---

# 论文笔记：Hyperagents

## 元信息

| 项目 | 内容 |
|------|------|
| 机构 | University of British Columbia / Vector Institute / University of Edinburgh / NYU / FAIR at Meta / Meta Superintelligence Labs |
| 日期 | March 2026 |
| 项目主页 | https://github.com/facebookresearch/Hyperagents |
| 对比基线 | [[Darwin Godel Machine]], ADAS |
| 链接 | [arXiv](https://arxiv.org/abs/2603.19461) / [Code](https://github.com/facebookresearch/Hyperagents) |

---

## 一句话总结

> 提出Hyperagents框架，将任务智能体与元智能体整合为单一可编辑程序，实现元认知级别的自我修改，支持任意可计算任务的开端式自我改进。

---

## 核心贡献

1. **Hyperagents框架**: 提出自引用智能体概念，整合任务智能体和元智能体为单一可编辑程序，使自我改进机制本身可被修改
2. **元认知自我修改**: 智能体不仅能改进任务执行能力，还能改进生成未来改进的机制，突破固定元级机制的限制
3. **跨域泛化验证**: 在编程、论文评审、机器人奖励设计、奥林匹克数学评分四个领域验证方法有效性，证明元级改进可跨域迁移

---

## 问题背景

### 要解决的问题

现有自改进AI系统依赖**固定的人工设计的元级机制**（如固定的指令生成流程），这从根本上限制了系统的改进速度上限。即使添加元-元系统也只能将问题向上转移，最终导致无限递归。

### 现有方法的局限

- **Darwin Godel Machine (DGM)**: 仅在编程领域有效，依赖任务性能与自我修改技能的**对齐假设**——即编程能力提升会自然转化为自我改进能力提升
- **固定元级机制**: 所有现有方法都使用不可修改的改进指令生成机制，限制自我改进潜力
- **领域特异性**: DGM等方法难以扩展到编程之外，因为任务解决技能与自我反思/自我修改技能在非编程领域往往不匹配

### 本文的动机

作者认为：要让系统真正实现无界限的自我改进，必须使**改进机制本身可被修改**。通过将元智能体纳入可编辑程序，系统可以改进"如何改进"的能力，实现**元认知自我修改**。

---

## 方法详解

### 模型架构

Hyperagents 采用 **自引用架构**：

- **输入**: 任务描述 $T$ + 智能体代码库 $\mathcal{A}$
- **核心组件**: [[Task Agent|任务智能体]] 解决目标任务，[[Meta Agent|元智能体]] 修改智能体代码
- **关键特性**: 元智能体本身是可编辑代码的一部分，可被自身修改
- **输出**: 改进后的智能体代码 $\mathcal{A}'$ + 任务执行结果

### 核心概念定义

#### Agent (智能体)

定义为任意可计算程序，可选地包含对[[Foundation Models]]、外部工具或学习组件的调用。智能体不限于特定表示形式（如神经网络或提示词），可包含任意算法逻辑、记忆和控制流。

#### Task Agent (任务智能体)

被实例化用于解决特定任务集的智能体。例如：
- 为软件仓库生成代码编辑（Polyglot benchmark）
- 预测论文接受/拒绝决策（论文评审）
- 为机器人环境设计奖励函数（Genesis仿真）

#### Meta Agent (元智能体)

唯一任务是修改现有智能体并生成新智能体的智能体。可访问整个历史智能体档案和评估结果，提出的改进不仅针对任务解决逻辑，也可针对元智能体本身。

#### Hyperagent (超智能体)

自引用智能体，将任务智能体和元智能体整合到**单一可编辑程序**中。元智能体是同一可编辑程序的一部分，可以重写自己。使用Python实现（图灵完备），有潜力构建任意可计算机器。

### 核心模块

#### 模块1: [[Metacognitive Self-modification|元认知自我修改]]

**设计动机**: 打破固定元级机制的限制，让改进过程本身可被改进

**具体实现**:
- [[Meta Agent]] 作为可编辑代码的一部分
- 每次修改可同时影响任务智能体和元智能体
- 改进不限于任务行为，还包括"生成未来改进的机制"

#### 模块2: Open-ended Exploration (开端式探索)

**设计动机**: 避免过早收敛到局部最优，支持持续积累进步

**具体实现**:
- 维护**智能体档案** $\mathcal{A} = \{(a_0, s_0), (a_1, s_1), \ldots, (a_t, s_t)\}$
- 父代选择概率正比于性能、反比于成功编译的子代数量
- 成功变体作为未来改进的 stepping stones

---

## 关键公式

### 公式1: [[Improvement@k|性能提升度量]]

$$
\text{imp@k}(M, A, G, T) = \max_{A' \in \mathcal{A}^{(k)}(M,A,G)} \text{Evaluate}(A', T) - \text{Evaluate}(A, T)
$$

**含义**: 衡量固定元智能体 $M$ 在 $k$ 步修改内能获得的最大性能提升

**符号说明**:
- $M$: 初始元智能体
- $A$: 初始任务智能体
- $G$: 智能体生成算法
- $T$: 评估任务集
- $\mathcal{A}^{(k)}$: $k$ 步内生成的所有任务智能体集合

### 公式2: [[Growth Score|后代增长评分]]（迁移智能体选择）

$$
G_\gamma(i) = \frac{1}{|D(i)|} \sum_{j \in D(i)} (\alpha_j - \alpha_i) \cdot \gamma^{\text{dist}(i,j)}
$$

**含义**: 衡量智能体 $i$ 作为 stepping stone 的价值，优先选择能稳定产生更好后代的智能体

**符号说明**:
- $D(i)$: 智能体 $i$ 的后代集合
- $\alpha_j$: 智能体 $j$ 的评估分数
- $\gamma \in (0, 1]$: 衰减因子，控制对远代后代的权重
- $\text{dist}(i,j)$: 从 $i$ 到后代 $j$ 的边数

### 公式3: [[UCB Score|置信上限评分]]（自动发现的父代选择）

$$
\text{ucb\_score} = \text{normalized\_score} + \text{exploration\_weight} \cdot \sqrt{\frac{\log(\text{total\_children} + 1)}{\text{children} + 1}}
$$

**含义**: DGM-H自动发现的父代选择策略，结合性能与探索奖励

**符号说明**:
- $\text{normalized\_score}$: 归一化的性能分数
- $\text{exploration\_weight}$: 探索权重系数
- $\text{total\_children}$: 档案中总子代数
- $\text{children}$: 该智能体的子代数

---

## 关键图表

### Figure 1: Overview / 系统概览

![[Hyperagents_fig1_overview.png]]

**说明**: DGM-Hyperagents (DGM-H) 扩展 Darwin Godel Machine 到任意可计算任务。上方为原始DGM：编码智能体同时作为任务智能体和元智能体，使用固定的人工设计的指令生成机制。下方为DGM-H：任务智能体和元智能体整合为单一可修改程序（Hyperagent），元级改进机制本身可被编辑，实现**元认知自我修改**。

### Figure 2: DGM vs DGM-H Architecture

**DGM Architecture (Top)**:

![[Hyperagents_fig2_dgm_top.png]]

**DGM-H Architecture (Bottom)**:

![[Hyperagents_fig2_dgm-h_bottom.png]]

**说明**: 对比展示DGM与DGM-H的关键差异。DGM依赖固定指令生成机制，要求任务性能与自我修改技能对齐。DGM-H消除此假设，元智能体可自主改进。

### Figure 4: Main Experiment Results

![[Hyperagents_fig4_results.png]]

**说明**: DGM-H在论文评审和机器人奖励设计领域的训练/测试性能。DGM-H持续改进并超越所有基线：无自我改进的版本（DGM-H w/o self-improve）、无开端探索的版本（DGM-H w/o open-ended）、以及原始DGM。测试集结果显示泛化能力。

### Figure 5: Conceptual Comparison

![[Hyperagents_fig5_comparison.png]]

**说明**: 各方法的架构组件对比。DGM-H唯一具有完整的自我改进元智能体、开端探索和元认知自我修改三个特性。

### Figure 6: Robotics Reward Design Visualization

![[Hyperagents_fig6_robotics_reward.png]]

**说明**: 机器人奖励设计领域的结果对比。左图为默认奖励函数导致的站立行为，右图为生成的奖励函数诱导的跳跃行为（更优的躯干高度最大化策略）。

### Figure 7: Paper Review Self-improvement Archive

![[Hyperagents_fig7_archive_review.png]]

**说明**: 论文评审领域的智能体档案可视化。展示DGM-H如何从初始简单智能体逐步发展为具有复杂评审逻辑的智能体。

### Figure 8: Robotics Reward Design Archive

![[Hyperagents_fig8_archive_robotics.png]]

**说明**: 机器人奖励设计领域的智能体档案。展示奖励函数设计的进化过程。

### Figure 9: Math Grading Archive

![[Hyperagents_fig9_archive_math.png]]

**说明**: 奥林匹克数学评分领域的智能体档案。BetterGrader自动发现改进的评分策略。

### Figure 10: Confusion Matrices for IMO Grading

![[Hyperagents_fig10_confusion_left.png]]

![[Hyperagents_fig10_confusion_right.png]]

**说明**: 左图为ProofAutoGrader，右图为DGM-H发现的BetterGrader。BetterGrader减少了对中间解决方案的极端标签误判，正确识别更多"Almost"和"Partial"案例。

### Figure 11-12: Modifiable Parent Selection

![[Hyperagents_fig11_parent_test.png]]

![[Hyperagents_fig12_archive_modifiable.png]]

**说明**: DGM-H可自动修改父代选择机制。修改版优于随机选择但未超越精心设计的机制，展示了完全自引用优化的潜力和局限。

### Table 1: Method Comparison

| Method | Self-improving meta agents | Open-ended exploration | Metacognitive self-modification |
|--------|---------------------------|------------------------|--------------------------------|
| ADAS | No | No | No |
| DGM w/o open-ended | Yes | No | No |
| DGM | Yes | Yes | No |
| **DGM-H** | **Yes** | **Yes** | **Yes** |

**说明**: DGM-H是唯一同时具备三个关键特性的方法。

### Table 2: Domain Summary

| Domain | Input | Output | Metric |
|--------|-------|--------|--------|
| Coding (Polyglot) | Repo + instruction | Code diff | Pass rate |
| Paper Review | Paper text | Accept/Reject | Accuracy |
| Robotics Reward Design | Task description | Reward function | RL policy performance |
| IMO Grading | Problem + solution | Score (0/1/6/7) | Accuracy |

**说明**: 四个评估领域的输入输出格式和度量标准。

### Table 3: Foundation Models Used

| Domain | Self-modification | Evaluation |
|--------|------------------|------------|
| Polyglot | Claude 3.5 Sonnet (New) | o3-mini |
| Paper Review | Claude 4.5 Sonnet | GPT-4o |
| Robotics Reward Design | Claude 4.5 Sonnet | Claude 4.5 Sonnet |
| IMO Grading | Claude 4.5 Sonnet | o4-mini |

**说明**: 各实验设置使用的[[Foundation Models]]。

---

## 算法伪代码

### Algorithm 1: DGM-H Main Loop

```
Input: Initial agent a₀, task set T, maximum iterations τ
Output: Archive of scored agents A

s₀ ← Evaluate(a₀, T)           // Start with initial agent
initialize A ← {(a₀, s₀)}

for t ← 1 to τ do
    P ← SelectParents(A)       // Sample parent agents
    
    foreach (a, ·) ∈ P do
        a' ← a.Modify(a, A)    // Metacognitive self-modification
        s' ← Evaluate(a', T)   // Evaluate on tasks
        
        if IsValid(a') then
            A ← A ∪ {(a', s')}
        end
    end
end

return A
```

---

## 实验

### 数据集

| 数据集 | 规模 | 特点 | 用途 |
|--------|------|------|------|
| Polyglot | 60 tasks | 编码benchmark | 训练/测试 |
| Paper Review | 300 papers | 顶级会议论文 | 训练/验证/测试 |
| Genesis Go2Walking | 6 reps | 四足机器人仿真 | 训练/测试 |
| IMO-GradingBench | 100 tasks | 奥数评分 | 训练/验证/测试 |

### 实现细节

- **Backbone**: [[Foundation Models]] (Claude 4.5 Sonnet / o3-mini / GPT-4o)
- **迭代次数**: 80-100 iterations
- **评估协议**: 分阶段评估（先小样本筛选，再全量评估）
- **成本估算**: 100迭代约88.6M tokens

### 主要结果

1. **编程领域**: DGM-H在Polyglot上达到与原始DGM相当的改进（从0.084提升到0.267）
2. **论文评审**: 显著超越所有基线，测试准确率从0.030提升至0.532
3. **机器人奖励设计**: 成功生成跳跃行为奖励函数，超越默认站立策略
4. **数学评分**: BetterGrader准确率提升4.06%，改善中间标签识别

### 元级改进案例

DGM-H自动发现的改进策略包括：
- **Persistent Memory**: 创建MemoryTool类存储跨代知识
- **Performance Tracker**: 记录各代性能用于数据驱动决策
- **Prompt Template Systems**: 抽象提示词为可复用模板
- **Multi-generation Refinement**: 基于多代性能数据迭代优化

---

## 批判性思考

### 优点

1. **理论突破**: 打破固定元级机制限制，实现真正的自引用改进
2. **跨域验证**: 四个不同领域均显示有效性，证明泛化能力
3. **元级迁移**: 改进的自我改进机制可跨域转移和积累
4. **自动化发现**: 无人工设计，自动发现经典算法（如UCB）和有效策略

### 局限性

1. **父代选择**: 自动修改的父代选择机制仍未超越精心设计的方法
2. **计算成本**: 每次运行需大量tokens（约88.6M），成本较高
3. **评估依赖**: 依赖任务评估信号，可能受评估游戏（Goodhart定律）影响
4. **安全考量**: 自改进系统可能放大人类偏见，需谨慎设计评估标准

### 潜在改进方向

1. **完全自引用**: 进一步使父代选择、评估机制等全部可修改
2. **效率优化**: 减少tokens消耗，支持更长时间尺度改进
3. **安全机制**: 增加人机协同监督，防止评估游戏
4. **更多领域**: 扩展到科学发现、创意生成等更多任务类型

### 可复现性评估

- [x] 代码开源 (GitHub)
- [ ] 预训练模型（使用商业API）
- [x] 训练细节完整（Appendix D）
- [x] 数据集可获取（公开benchmark）

---

## 关联笔记

### 基于

- [[Darwin Godel Machine|DGM]]: 直接扩展的基础框架
- [[Open-endedness]]: 开端式探索的理论基础
- [[Godel Machine]]: 自改进AI的理论先驱

### 对比

- [[ADAS]]: 无自我改进元智能体的基线
- [[AlphaGo Zero]]: 固定学习算法的自对弈改进

### 方法相关

- [[Foundation Models]]: 智能体的基础组件
- [[Meta Learning]]: 元级学习的理论框架
- [[Recursive Self-improvement]]: 核心技术路线
- [[Self-referential Learning]]: 自引用学习的具体实现
- [[Agent Systems]]: 智能体系统设计范式

### 硬件/数据相关

- [[Genesis Simulator]]: 机器人奖励设计使用的仿真器
- [[Polyglot Benchmark]]: 编码评估数据集
- [[IMO-GradingBench]]: 数学评分数据集

---

## 速查卡片

> [!summary] Hyperagents
> - **核心**: 自引用智能体，元改进机制本身可被修改
> - **方法**: Task Agent + Meta Agent 整合为单一可编辑程序
> - **结果**: 四领域验证，元级改进跨域迁移
> - **代码**: https://github.com/facebookresearch/Hyperagents

---

*笔记创建时间: 2026-04-13*