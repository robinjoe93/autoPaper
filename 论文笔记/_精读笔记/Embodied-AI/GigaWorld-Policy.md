---
title: "GigaWorld-Policy: An Efficient Action-Centered World-Action Model"
method_name: "GigaWorld-Policy"
authors: [Angen Ye, Boyuan Wang, Chaojun Ni, Guan Huang, Guosheng Zhao, Hao Li, Hengtao Li, Jie Li, Jindi Lv, Jingyu Liu, Min Cao, Peng Li, Qiuping Deng, Wenjun Mei, Xiaofeng Wang, Xinze Chen, Xinyu Zhou, Yang Wang, Yifan Chang, Yifan Li, Yukun Zhou, Yun Ye, Zhichao Liu, Zheng Zhu]
year: 2026
venue: arXiv
tags: [world-action-model, robot-policy-learning, diffusion-transformer, embodied-ai, video-generation]
zotero_collection: Embodied-AI
image_source: online
arxiv_html: https://arxiv.org/html/2603.17240v2
created: 2026-04-20
---

# 论文笔记：GigaWorld-Policy: An Efficient Action-Centered World-Action Model

## 元信息

| 项目 | 内容 |
|------|------|
| 机构 | GigaAI |
| 日期 | 2026年3月 |
| 项目主页 | https://gigaai-research.github.io/GigaWorld-Policy/ |
| 对比基线 | [[Motus]], [[Pi05]], [[Cosmos-Policy]] |
| 链接 | [arXiv](https://arxiv.org/abs/2603.17240) |

---

## 一句话总结

> 提出 action-centered 的 [[World-Action Model]]，通过因果设计使推理时可跳过未来视频生成，实现 9 倍推理加速同时提升任务成功率 7%。

---

## 核心贡献

1. **高效推理的 WAM 架构**: 通过因果注意力设计，使未来视频 token 不影响动作 token，推理时可跳过视频生成分支，实现低延迟控制
2. **密集监督的训练策略**: 同时优化动作预测和视频预测目标，视频生成作为辅助监督提供物理一致性约束
3. **大规模数据预训练范式**: 收集约 10000 小时机器人/人类演示数据，将通用视频生成模型转化为机器人策略 backbone

---

## 问题背景

### 要解决的问题

现有 [[World-Action Model]] (WAM) 方法存在两个关键瓶颈：
1. **推理开销大**: 联合推理未来视觉动态和动作需要大量计算，视频 token 数量远大于动作 token
2. **动作预测依赖视频质量**: 联合建模使动作预测准确度高度依赖未来视频预测质量，误差会累积传播

### 现有方法的局限

| 方法类型 | 代表工作 | 局限性 |
|----------|----------|--------|
| VLM-based VLA | [[Pi05]], [[GigaBrain-0]] | 仅靠稀疏动作监督，缺乏视觉动态约束 |
| 双向注意力 WAM | [[Motus]], [[VideoVLA]] | 推理时必须生成未来视频，延迟高 |
| 两阶段设计 | [[Mimic-video]], [[LingBot-VA]] | 需要完整视频预测才能获得动作，仍有推理开销 |

### 本文的动机

作者认为可以将未来视觉动态作为训练时的监督信号，而非推理时的必需步骤：
- 训练时：视频生成提供密集监督和物理一致性约束
- 推理时：因果设计使视频生成可选，直接输出动作

---

## 方法详解

### 模型架构

GigaWorld-Policy 基于 5B 参数的 [[Diffusion Transformer]] backbone (Wan 2.2)，采用 action-centered 设计：

- **输入**: 语言指令 $l$ + 多视角观测 $o_t$ + 本体状态 $s_t$
- **Backbone**: Wan 2.2 5B [[Diffusion Transformer]]
- **核心模块**: [[Causal Self-Attention]] 统一视频和动作建模
- **输出**: [[Action Chunking|动作块]] $a_{t:t+p-1}$（可选未来视频）
- **总参数**: 5B

### 核心模块

#### 输入 Token 化

**多视角合并**: 将三个相机视角合并为单一复合图像：

$$
o_{t}^{comp}={Compose}\!\left(o_{t}^{left},\,o_{t}^{front},\,o_{t}^{right}\right).
$$

**Token 类型**:
- $T_o$: 当前观测的视觉 token
- $T_f$: 未来观测的视觉 token
- $T_s$: 本体状态 token
- $T_a$: 动作 token
- $T_l$: 语言指令 token（通过 cross-attention 注入）

#### 统一 Token 序列

$$
T_{t}=\big[\;T_{o}\ ;\,T_{s}\,;\;T_{a}\,;\;T_{f}\,\big].
$$

#### 因果注意力机制

关键设计：[[Causal Mask]] 控制信息流：
- $T_s$ 和 $T_o$ 可互相 attend，但不能 attend $T_a$ 或 $T_f$
- $T_a$ 可 attend $T_s$ 和 $T_o$，但不能 attend $T_f$（**关键**）
- $T_f$ 可 attend $T_s$, $T_o$ 和 $T_a$

这确保了**动作预测不受未来视频预测影响**，使推理时可跳过 $T_f$。

---

## 关键公式

### 公式1: [[VLA Policy|VLA 策略建模]]

$$
a_{t:t+p-1}\sim q_{\Theta}(\,\cdot\mid o_{t},\,s_{t},\,l\,).
$$

**含义**: 传统 VLA 仅建模动作分布，缺乏视觉动态监督。

**符号说明**:
- $a_{t:t+p-1}$: 动作块（长度 $p$）
- $o_t$: 当前观测
- $s_t$: 本体状态
- $l$: 语言指令

### 公式2: [[Action-Video Joint Modeling|动作-视频联合建模]]

$$
\big(a_{t:t+p-1},\,c_{t}\big)\sim g_{\Theta}(\,\cdot\mid o_{t},\,s_{t},\,l\,).
$$

**含义**: 模型同时输出动作块和动作潜变量 $c_t$（用于引导视频预测）。

### 公式3: [[Visual Dynamics|视觉动态预测]]

$$
(o_{t+\Delta},\,o_{t+2\Delta},\,\ldots,\,o_{t+K\Delta})\sim g_{\Theta}\!\big(\,\cdot\mid o_{t},\,s_{t},\,l,\,c_{t}\big).
$$

**含义**: 以动作为条件预测未来稀疏观测序列。

**符号说明**:
- $\Delta$: 时间步长（=12）
- $K = \lfloor p/\Delta\rfloor$: 预测帧数

### 公式4: [[Flow Matching|流匹配训练]]

**噪声插值**:

$$
x^{(s)}=(1-s)\epsilon+sx.
$$

**目标速度**:

$$
\dot{x}^{(s)}=x-\epsilon.
$$

### 公式5: [[Video Loss|视频生成损失]]

$$
\mathcal{L}_{video}=\mathbb{E}_{s,\epsilon}\Big[\big\|g_{\Theta}\!\big(z_{f}^{(s)},\,s\mid T_{s},\,T_{o},\,T_{a},\,T_{l}\big)-\dot{z}_{f}^{(s)}\big\|^{2}\Big].
$$

**含义**: 预测未来视频 latent 的速度场，以动作为条件。

### 公式6: [[Action Loss|动作预测损失]]

$$
\mathcal{L}_{action}=\mathbb{E}_{s,\epsilon}\Big[\big\|g_{\Theta}\!\big(a^{(s)},\,s\mid T_{s},\,T_{o},\,T_{l}\big)-\dot{a}^{(s)}\big\|^{2}\Big].
$$

**含义**: 预测动作的速度场，不依赖未来视频 token。

### 公式7: 总损失

$$
\mathcal{L}_{all}=\lambda_{video}\,\mathcal{L}_{video}+\lambda_{action}\,\mathcal{L}_{action}.
$$

**符号说明**:
- $\lambda_{video}=1$
- $\lambda_{action}=5$

### 公式8: 推理时的动作解码

$$
\frac{da^{(s)}}{ds}=g_{\Theta}\!\big(a^{(s)},\,s\mid w_{t}\big),\qquad s\in[0,1].
$$

**含义**: 从噪声积分到动作，不生成未来视频 token。

**符号说明**:
- $w_{t}=\big(T_{l},\ T_{s},\ T_{o}\big)$: 条件上下文

---

## 关键图表

### Figure 1: 性能对比

![Figure 1](https://arxiv.org/html/2603.17240v2/2603.17240v2/x1.png)

**说明**: GigaWorld-Policy 在真实机器人场景中实现 9x 推理加速，同时任务成功率比 Motus 提升 7%。

### Figure 2: 方法对比

![Figure 2](https://arxiv.org/html/2603.17240v2/2603.17240v2/x2.png)

**说明**: 四种范式对比：(a) VLM-based VLA (b) 双向注意力 WAM (c) 两阶段设计 (d) GigaWorld-Policy 的 action-centered 设计。

### Figure 3: 架构概览

![Figure 3](https://arxiv.org/html/2603.17240v2/2603.17240v2/x3.png)

**说明**: 预训练阶段从大规模视频学习动作相关表征；后训练阶段联合预测动作和未来视频；推理时视频生成可选。

### Figure 4: 因果注意力机制

![Figure 4](https://arxiv.org/html/2603.17240v2/2603.17240v2/x4.png)

**说明**: 关键设计——动作 token $T_a$ 不 attend 未来视频 token $T_f$，使推理时可跳过视频生成。

### Table 1: 预训练数据集

| Dataset | Hours | Dataset | Hours | Dataset | Hours |
|--------|------|--------|------|--------|------|
| EgoDex | 800 | Agibot | 2,500 | EGO4D | 3,500 |
| RoboMind | 300 | RDT | 25 | Open X-Embodiment | 3,500 |
| DROID | 350 | ATARA | 10 | Something-Something V2 | 200 |

**说明**: 总计约 10000 小时的具身数据预训练。

### Table 2: RoboTwin 2.0 仿真实验结果

| Method | Adjust Bottle | Place A2b | Place Bread | Place Cans | Place Fan | Avg |
|--------|---------------|-----------|-------------|------------|-----------|-----|
| pi-0.5 | 0.79 | 0.62 | 0.38 | 0.40 | 0.25 | - |
| GigaBrain-0 | 0.83 | 0.60 | 0.46 | 0.47 | 0.36 | - |
| Cosmos-Policy | 1.00 | 0.48 | 0.77 | 0.97 | - | - |
| Motus | 0.99 | 0.49 | 0.67 | **0.98** | - | 0.89 |
| **GigaWorld-Policy** | **1.00** | **0.94** | **0.94** | **1.00** | - | **0.95** |

**说明**: GigaWorld-Policy 比 Motus 快 9 倍，性能提升 7%；比 pi-0.5 提升 95%。

---

## 实验

### 数据集

| 数据集 | 规模 | 特点 | 用途 |
|--------|------|------|------|
| Embodied Pre-training | ~10000h | 机器人+人类演示 | 预训练 |
| RoboTwin 2.0 | 仿真 | 多任务 | 评估 |
| Real-world Tasks | 真实机器人 | pick-and-place 等 | 评估 |

### 实现细节

- **Backbone**: Wan 2.2 5B [[Diffusion Transformer]]
- **Action Chunk**: $p=48$ 步
- **Future Stride**: $\Delta=12$
- **Loss Weight**: $\lambda_{action}=5$, $\lambda_{video}=1$
- **硬件**: A100 GPU

### 关键实验结果

1. **推理速度**: 0.36s/推理（比 Motus 快 9 倍）
2. **真实机器人成功率**: 比 Motus 提升 7%
3. **仿真基准**: 比 pi-0.5 提升 95%（RoboTwin 2.0）

---

## 批判性思考

### 优点

1. **推理高效**: 因果设计使视频生成可选，大幅降低延迟
2. **训练密集监督**: 视频生成提供物理一致性约束
3. **数据利用充分**: 预训练阶段充分利用多种数据源

### 局限性

1. **依赖预训练 backbone**: 需要 Wan 2.2 等大规模视频模型初始化
2. **多视角处理简单**: 仅用空间拼接，未深入融合跨视角信息
3. **未来视频可选**: 丢失了视频预测的在线纠正能力

### 潜在改进方向

1. 探索更复杂的多视角融合机制
2. 研究推理时动态启用/禁用视频分支的策略
3. 结合 3D/4D 世界模型增强空间理解

### 可复现性评估

- [ ] 代码开源（待发布）
- [ ] 预训练模型
- [ ] 训练细节完整
- [ ] 数据集可获取

---

## 关联笔记

### 基于

- [[Wan 2.2]]: 预训练 backbone
- [[Flow Matching]]: 训练框架
- [[Diffusion Transformer]]: 核心架构

### 对比

- [[Motus]]: WAM baseline，双向注意力设计
- [[Pi05]]: VLM-based VLA baseline
- [[VideoVLA]]: 联合视频-动作建模

### 方法相关

- [[World-Action Model]]: 核心范式
- [[Causal Attention]]: 关键设计
- [[Action Chunking]]: 输出形式
- [[Visual Dynamics]]: 监督来源

### 硬件/数据相关

- [[RoboTwin 2.0]]: 仿真评估基准
- [[Open X-Embodiment]]: 预训练数据

---

## 速查卡片

> [!summary] GigaWorld-Policy
> - **核心**: action-centered WAM，推理时视频生成可选
> - **方法**: 因果注意力 + 流匹配训练
> - **结果**: 9x 加速，成功率 +7%
> - **代码**: 待发布

---

*笔记创建时间: 2026-04-20*