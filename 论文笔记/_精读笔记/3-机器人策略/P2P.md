---
title: "Scaling Behavior Cloning Improves Causal Reasoning: An Open Model for Real-Time Video Game Playing"
method_name: "P2P"
authors: [Yuguang Yue, Irakli Salia, Samuel Hunt, Chris Green, Wenzhe Shi, Jonathan J Hunt]
year: 2026
venue: arXiv
tags: [behavior-cloning, game-agent, causal-reasoning, scaling-law, vision-language-action, real-time-inference]
zotero_collection: 3-机器人策略
image_source: local
arxiv_html: https://arxiv.org/html/2601.04575v2
created: 2026-04-13
---

# 论文笔记：Scaling Behavior Cloning Improves Causal Reasoning

## 元信息

| 项目 | 内容 |
|------|------|
| 机构 | Elefant AI |
| 日期 | January 2026 |
| 项目主页 | https://elefant-ai.github.io/open-p2p/ |
| 对比基线 | [[VPT]], [[OpenVLA]] |
| 链接 | [arXiv](https://arxiv.org/abs/2601.04575) |

---

## 一句话总结

> 开放的视频游戏玩法基础模型，通过扩展 [[Behavior Cloning]] 的模型规模和数据规模，揭示了因果推理能力随规模提升的规律，在消费级 GPU 上实现 20 Hz 实时推理。

---

## 核心贡献

1. **开放数据与方法**: 发布 8300+ 小时高质量人类游戏数据、训练代码、推理代码和预训练权重
2. **P2P 模型架构**: 轻量级解码器-only Transformer，支持 20 Hz 实时推理
3. **因果推理扩展规律**: 发现更大模型和更多数据提升因果性评分，扩展定律 L(D) = L_∞ + (D_c/D)^α

---

## 问题背景

### 要解决的问题

如何在消费级 GPU 上实现实时（20 Hz）的多人游戏智能体，同时理解行为克隆的扩展规律和因果推理能力？

### 现有方法的局限

- 大多数 VLA 模型基于预训练 VLM，推理速度慢，难以达到实时要求
- 单游戏智能体（如 Minecraft、CS:GO）无法泛化到多种游戏
- [[Behavior Cloning]] 存在因果混淆问题：模型依赖历史动作而非视觉输入做决策
- 缺乏大规模开放数据和方法，难以复现和扩展研究

### 本文的动机

作者认为通过扩展模型和数据规模可以改善因果推理能力，同时设计轻量级架构实现实时推理，并开放所有资源促进社区研究。

---

## 方法详解

### 模型架构

[[P2P]] 采用 **解码器-only Transformer** 架构：

- **输入**: 文本指令 $t_i$ + 视觉观测 $o_i$ + 真实动作 $a_i$ + 推理 token $k_i$ + 动作预测 token $a_{in}$
- **图像编码器**: EfficientNet 前 6 层 + 线性投影（$N_i \in \{1, ..., 4\}$ 个视觉 token）
- **文本编码器**: EmbeddingGemma + mean pooling
- **核心模块**: [[Action Decoder]] 将 $a_{in}$ 解码为完整动作空间（$N_a = 8$ 个 token）
- **输出**: 键盘（4 token）+ 鼠标移动（2 token）+ 鼠标按钮（2 token）
- **总参数**: 150M / 300M / 600M / 1.2B

每个时间步 token 数为 $N_i + N_a + 3$，使用 [[RoPE]] 位置编码和滑动窗口注意力。

### 核心模块

#### 模块1: Action Decoder

**设计动机**: 避免在主 Transformer 中预测所有动作 token，实现约 5x 推理加速

**具体实现**:
- 主 Transformer 输出单个潜在动作 token $a_{in}$
- 轻量级解码器自回归解码为 8 个动作 token
- 允许 4 个同时按键 + 2 个鼠标移动 + 2 个鼠标按钮

#### 模块2: Causal Attention Mask

**设计动机**: 防止训练时信息泄漏，确保 $a_{in}$ 不能看到当前时间步的真实动作

**具体实现**:
- 使用自定义因果注意力掩码
- $a_{in}$ 禁止关注同一时间步的 ground-truth 动作
- 其他 token 不关注历史 $a_{in}$ 以稳定训练

#### 模块3: 训练-推理差距缓解

**设计动机**: 视频压缩和 resize 导致训练输入与推理输入不一致

**具体实现**:
- 使用 RGB 编码而非 YUV（差距更小）
- 确保 resize 函数在训练和推理中比特级一致
- 应用 [[Data Augmentation]]：空间变换、颜色扰动、Planckian jitter、ISO noise、模糊、锐化、平移
- 鼠标动作使用截断正态分布采样：$\hat{\mu}_x=0, \hat{\sigma}_x=96, \hat{\mu}_y=0, \hat{\sigma}_y=22$

---

## 关键公式

### 公式1: [[Scaling Law|扩展定律]]

$$
L(D) = L_{\infty} + \left(\frac{D_c}{D}\right)^{\alpha}
$$

**含义**: 测试损失与训练数据量的幂律关系

**符号说明**:
- $L(D)$: 使用 $D$ 个训练帧时的测试损失
- $L_{\infty}$: 不可约损失（1.2B 模型估计为 1.111）
- $D$: 训练帧数
- $D_c$: 拟合常数（估计为 17）
- $\alpha$: 拟合常数（估计为 0.2336）

### 公式2: [[Causality Score|因果性评分]]

$$
\text{score}(f) = \sum_{b=1}^{B} \sum_{c=1}^{C} \mathrm{KL}\big(f(o_{b,c}, a_{b,c}) || f(\tilde{o}_{b,c}, a_{b,c})\big)
$$

**含义**: 衡量模型对视觉输入的因果依赖程度，高分表示更强依赖视觉而非动作历史

**符号说明**:
- $f$: 策略模型
- $o$: 原始图像序列
- $\tilde{o}$: 扰动图像序列（替换为同游戏不同场景的帧）
- $a$: 原始动作序列（保持不变）
- $B$: batch size（32）
- $C$: 时间块数（10）
- 扰动概率 $p = 0.5$

### 公式3: Toy Problem 因果性度量

$$
c = \log p(a=1|s_o) - \log p(a=1|s_b)
$$

**含义**: 在玩具环境中衡量策略是否正确响应障碍而非刹车灯

**符号说明**:
- $s_o$: 有障碍、无刹车灯的状态
- $s_b$: 无障碍、有刹车灯的状态
- 最优因果策略：$c_{optimal} = \infty$

---

## 关键图表

### Figure 1: 游戏序列示例

![P2P_fig1_gameplay](https://raw.githubusercontent.com/robinjoe93/autoPaper/main/assets/images/P2P_fig1_gameplay.png)

**说明**: 展示文本注释和动作对齐的游戏序列。键盘动作简化为 WASD，箭头表示鼠标移动方向。

### Figure 2: P2P 架构与注意力掩码

![P2P_fig2a_architecture](https://raw.githubusercontent.com/robinjoe93/autoPaper/main/assets/images/P2P_fig2a_architecture.png)
![P2P_fig2b_mask](https://raw.githubusercontent.com/robinjoe93/autoPaper/main/assets/images/P2P_fig2b_mask.png)

**说明**: (a) P2P 解码器-only Transformer 架构，文本 token $t_i$ → 图像 token $o_i$ → 推理 token $k_i$ → 动作预测 token $a_{in}$。Action Decoder 自回归解码。 (b) 自定义注意力掩码，绿色表示 1（可关注），灰色表示 0。$a_{in}$ 不能关注当前时间步的 ground-truth 动作。

### Figure 3: 训练-推理差距

![P2P_fig3a_gap](https://raw.githubusercontent.com/robinjoe93/autoPaper/main/assets/images/P2P_fig3a_gap.png)
![P2P_fig3b_augment](https://raw.githubusercontent.com/robinjoe93/autoPaper/main/assets/images/P2P_fig3b_augment.png)

**说明**: (a) 视频压缩导致训练-推理差距，RGB 编码比 YUV 差距更小，QP 值越大质量越差。 (b) 数据增强显著缓解差距。

### Table 1: 程序化环境性能

| Model Size | Hovercraft (↓) | Simple-FPS (↑) | FPS (↑) |
|------------|----------------|----------------|---------|
| 150M | 41.1 ± 8.8 | 25.3 ± 6.5 | 80 |
| 300M | 41.7 ± 8.2 | 25.1 ± 5.6 | 64 |
| 600M | 38.1 ± 4.2 | 26.5 ± 10.4 | 62 |
| 1.2B | **36.8 ± 2.9** | **26.6 ± 3.7** | 40 |

**说明**: Hovercraft 测量完成一圈的时间（秒），Simple-FPS 测量击中敌人数减被击中数。更大的模型方差更低。FPS 在 RTX 5090 上测量。

### Figure 4: 人类偏好比较

![P2P_fig4a_comparison](https://raw.githubusercontent.com/robinjoe93/autoPaper/main/assets/images/P2P_fig4a_comparison.png)
![P2P_fig4b_text](https://raw.githubusercontent.com/robinjoe93/autoPaper/main/assets/images/P2P_fig4b_text.png)

**说明**: (a) 更大模型在人类偏好中获胜比例更高。 (b) 指令跟随测试，给定"press the red button"指令，成功率显著提升。

### Figure 5: 扩展定律曲线

![P2P_fig5_scaling](https://raw.githubusercontent.com/robinjoe93/autoPaper/main/assets/images/P2P_fig5_scaling.png)

**说明**: 1.2B 模型的测试损失与数据量呈幂律关系，拟合良好。

### Figure 6: 因果性玩具问题

![P2P_fig6a_toy_env](https://raw.githubusercontent.com/robinjoe93/autoPaper/main/assets/images/P2P_fig6a_toy_env.jpeg)
![P2P_fig6b_toy](https://raw.githubusercontent.com/robinjoe93/autoPaper/main/assets/images/P2P_fig6b_toy.png)

**说明**: (a) 玩具环境：障碍（因果特征）和刹车灯（非因果但相关）。 (b) 更深的网络学习因果策略更快，线性网络 SGD 无法学习因果策略。

### Figure 7: 因果性评分与规模

![P2P_fig7_causality](https://raw.githubusercontent.com/robinjoe93/autoPaper/main/assets/images/P2P_fig7_causality.png)

**说明**: 因果性评分随模型规模和数据规模增加（30M 低数据区域除外）。

### Figure 8: 标注数据分布

![P2P_fig8_distribution](https://raw.githubusercontent.com/robinjoe93/autoPaper/main/assets/images/P2P_fig8_distribution.png)

**说明**: 8300+ 小时标注数据涵盖多种游戏类型。

### Figure 11: Tokenizer 训练效果

![P2P_fig11a_frozen_train](https://raw.githubusercontent.com/robinjoe93/autoPaper/main/assets/images/P2P_fig11a_frozen_train.png)
![P2P_fig11b_frozen_val](https://raw.githubusercontent.com/robinjoe93/autoPaper/main/assets/images/P2P_fig11b_frozen_val.png)

**说明**: 解冻图像 tokenizer 训练显著降低训练和验证 perplexity。

---

## 实验

### 数据集

| 数据集 | 规模 | 特点 | 用途 |
|--------|------|------|------|
| Annotated Data | 8300+ hours | 文本注释 + 动作 + 纠正轨迹 | 训练 |
| Unlabeled Data | ~500M frames | 公开游戏视频 | 预训练 |
| Hovercraft | 程序化 | 固定赛道绕圈 | 评估 |
| Simple-FPS | 程序化 | 简单 FPS 射击 | 评估 |
| DOOM/Quake | Steam | 单人游戏关卡 | 人类评估 |
| Roblox | 多人 | Be-a-Shark / Hypershot | 人类评估 |

### 实现细节

- **Backbone**: EfficientNet 前 6 层（解冻训练）
- **文本编码器**: EmbeddingGemma（冻结）
- **优化器**: SGD
- **分辨率**: 192 × 192
- **推理频率**: 20 Hz
- **硬件**: NVIDIA RTX 5090

### 可视化结果

模型在 DOOM、Quake 等游戏中表现出接近人类的游戏能力。定性结果显示在项目主页 https://elefant-ai.github.io/open-p2p/。

---

## 批判性思考

### 优点

1. 完全开放：数据、代码、模型权重全部发布
2. 实时推理：20 Hz 在消费级 GPU 上实现
3. 因果推理研究：首次系统分析 BC 扩展对因果性的影响
4. 文本条件：支持指令跟随

### 局限性

1. 文本指令理解能力有限（数据多样性不足）
2. 需要大量高质量人类数据
3. 仅支持键盘-鼠标控制，不支持控制器
4. 因果性评分是间接度量，难以直接验证因果正确性

### 潜在改进方向

1. 扩展文本指令的多样性和数量
2. 探索更高效的图像编码器
3. 与强化学习结合解决分布偏差
4. 支持更多游戏类型和控制器

### 可复现性评估

- [x] 代码开源
- [x] 预训练模型
- [x] 训练细节完整
- [x] 数据集可获取

---

## 关联笔记

### 基于

- [[Behavior Cloning]]: 核心方法
- [[VPT]]: 视频预训练方法
- [[DAgger]]: 纠正数据收集启发

### 对比

- [[OpenVLA]]: 基于 VLM 的 VLA 模型，推理慢
- [[SIMA]]: 多游戏智能体，不开放
- [[Game-TARS]]: 预训练游戏智能体，不开放

### 方法相关

- [[Scaling Law]]: 扩展定律分析
- [[Causal Reasoning]]: 因果推理研究
- [[Transformer]]: 解码器架构
- [[RoPE]]: 位置编码
- [[Data Augmentation]]: 训练-推理差距缓解
- [[KL Divergence]]: 因果性评分度量

### 硬件/数据相关

- [[EfficientNet]]: 图像编码器
- [[EmbeddingGemma]]: 文本编码器
- [[Vision Language Model]]: 文本注释生成

---

## 速查卡片

> [!summary] P2P: Pixels2Play
> - **核心**: 扩展行为克隆改善因果推理
> - **方法**: 解码器-only Transformer + Action Decoder
> - **结果**: 1.2B 模型因果性评分最高，20 Hz 实时推理
> - **代码**: https://elefant-ai.github.io/open-p2p/

---

*笔记创建时间: 2026-04-13*