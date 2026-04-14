---
title: "Beyond Single Tokens: Distilling Discrete Diffusion Models via Discrete MMD"
method_name: "D-MMD"
authors: [Emiel Hoogeboom, David Ruhe, Jonathan Heek, Thomas Mensink, Tim Salimans]
year: 2026
venue: arXiv
tags: [discrete-diffusion, diffusion-distillation, moment-matching, text-generation, image-generation, generative-model]
zotero_collection: 论文笔记
image_source: local
created: 2026-04-13
---

# 论文笔记：Beyond Single Tokens: Distilling Discrete Diffusion Models via Discrete MMD

## 元信息

| 项目 | 内容 |
|------|------|
| 机构 | Google DeepMind Amsterdam |
| 日期 | March 2026 |
| 对比基线 | [[SDTT]], [[Di4C]], [[DiMO]] |
| 链接 | [arXiv](https://arxiv.org/abs/2603.20155) |

---

## 一句话总结

> D-MMD 将连续域的 [[Moment Matching Distillation|MMD]] 推广到离散扩散模型，实现少量步骤生成高质量样本，甚至超越教师模型。

---

## 核心贡献

1. **离散 MMD 框架**: 将连续扩散的 MMD 方法推广到离散扩散过程（[[Masked Diffusion]] 和 [[Uniform Diffusion]]）
2. **质量超越教师**: 在文本和图像生成任务上，蒸馏后的学生模型使用少量 NFE 即可超越教师
3. **新评估指标**: 提出 [[Gradient Moment]] 指标，比 generative perplexity 更能真实反映样本质量

---

## 问题背景

### 要解决的问题
[[Discrete Diffusion Models]] 采样需要大量步骤（如 1024 步），导致计算成本高。现有蒸馏方法在离散域效果不佳，容易导致模式崩溃。

### 现有方法的局限
- **[[SDTT]]**: 类似渐进蒸馏的方法，但无法处理相关性输出，会导致模式丢失
- **[[Di4C]]**: 通过混合分布扩展输出，但混合数量需指数增长才能捕获所有相关性
- **[[DiMO]]**: 仅支持单步蒸馏，不支持少步生成

### 本文的动机
连续扩散领域的 [[Moment Matching Distillation|MMD]] 已证明有效，核心思想是匹配条件期望。本文将此思想推广到离散域，通过对数概率空间进行矩匹配。

---

## 方法详解

### 核心思想

D-MMD 采用对抗性优化框架：
- **生成器** $g_\eta$: 最小化在教师模型下的损失，最大化在辅助模型下的损失
- **辅助模型** $\hat{x}_\phi$: 学习生成器的期望，并保持接近教师

### 模型架构

- **输入**: 噪声状态 $z_t$（扩散后的 token 序列）
- **核心模块**: [[Teacher Model]] $\hat{x}_\theta$ + [[Auxiliary Model]] $\hat{x}_\phi$ + [[Generator]] $\hat{x}_\eta$
- **输出**: 概率向量 $\hat{x}_\eta(z_t)$，用于采样生成 token

### 关键模块

#### 模块1: 生成器损失

**设计动机**: 通过对抗训练让生成器匹配教师分布

**具体实现**:
- 使用软概率向量 $\hat{x}_\eta(z_t)$ 而非硬样本
- 在对数概率空间计算矩匹配

#### 模块2: 辅助模型

**设计动机**: 跟踪生成器的期望分布，形成稳定的对抗优化

**具体实现**:
- 交替优化生成器和辅助模型
- 辅助模型被教师模型正则化

---

## 关键公式

### 公式1: [[KL Divergence]] 上界

$$
\text{KL}[q(x) \| p_\theta(x)] \leq \text{KL}[q(z_0, \ldots, z_1 | x) \| p_\theta(z_0, \ldots, z_1)]
$$

**含义**: 扩散模型的训练目标可以分解为逐步的 KL 散度最小化

**符号说明**:
- $q(x)$: 数据分布
- $p_\theta(x)$: 模型分布
- $z_t$: 时间 $t$ 的扩散状态

### 公式2: 条件期望最优解

$$
\mathbb{E}_q[x | z_t] = \hat{x}_\theta(z_t), \quad \text{under } q(x, t, z_t)
$$

**含义**: 扩散模型的最优解是给定噪声状态下的数据期望

**符号说明**:
- $\hat{x}_\theta(z_t)$: 模型输出
- $\mathbb{E}_q$: 数据分布下的期望

### 公式3: 典型扩散损失

$$
L(\theta) = \mathbb{E}_{q(x,t,z_t)}[w(t) L_t(x, \hat{x}_\theta(z_t), z_t)]
$$

**含义**: 通过时间加权损失训练扩散模型

**符号说明**:
- $w(t)$: 时间权重函数
- $L_t$: 时间 $t$ 的损失函数

### 公式4: 离散扩散后验

$$
q(z_s | z_t, x) = \text{Cat}\left(z_s \middle| \frac{[\alpha_{t|s} z_t + (1-\alpha_{t|s}) \mathbf{1}\pi^\top z_t] \odot [\alpha_s x + (1-\alpha_s) \pi]}{\alpha_t z_t^\top x + (1-\alpha_t) z_t^\top \pi}\right)
$$

**含义**: 给定当前状态和数据，推断前一时间步的状态分布

**符号说明**:
- $\alpha_t$: 噪声调度参数
- $\pi$: 平稳分布（mask 或 uniform）
- $\alpha_{t|s} = \alpha_t / \alpha_s$: 相对调度

### 公式5: 离散扩散损失

$$
L_t^{\text{disc}}(x, \hat{x}_\theta(z_t), z_t) = [w(t) \text{CE}(x | \hat{x}_\theta(z_t))]
$$

**含义**: 使用交叉熵损失训练离散扩散模型

**符号说明**:
- $\text{CE}(x | \hat{x})$: 交叉熵损失 $-\sum_c x_c \log \hat{x}_c$

### 公式6: [[MMD Loss]]

$$
L_{\text{MMD}}^*(\eta) = \mathbb{E}_{g_\eta(x,t,z_t)}\left[w(t) \|\mathbb{E}_q[x | z_t] - \mathbb{E}_{g_\eta}[x | z_t]\|^2\right]
$$

**含义**: 连续域 MMD 损失，匹配教师和生成器的条件期望

**符号说明**:
- $g_\eta$: 生成器采样分布
- $\mathbb{E}_{g_\eta}[x | z_t]$: 生成器的条件期望

### 公式7: D-MMD 通用形式

$$
L_{\text{D-MMD}}(\eta) = \min_\eta \max_\phi \mathbb{E}_{g_\eta(z_t,x,s,z_s)}\left[L_s(x, \hat{x}_\theta(z_s), z_s) - L_s(x, \hat{x}_\phi(z_s), z_s) - L_s(\hat{x}_\theta(z_s), \hat{x}_\phi(z_s), z_s)\right]
$$

**含义**: D-MMD 的 min-max 对抗优化框架

**符号说明**:
- $L_s$: 时间 $s$ 的损失函数
- $\hat{x}_\phi$: 辅助模型输出

### 公式8: 生成器损失（离散版）

$$
L_{\text{GEN}}(\eta) = \text{CE}[\hat{x}_\eta | \hat{x}_\theta(z_s)] - \text{CE}[\hat{x}_\eta | \hat{x}_\phi(z_s)] = -\sum_c (\hat{x}_\eta)_c \log \hat{x}_\theta(z_s)_c - \log \hat{x}_\phi(z_s)_c
$$

**含义**: 生成器通过对比教师和辅助模型的对数概率进行更新

**符号说明**:
- $\hat{x}_\eta$: 生成器输出的软概率向量
- $c$: 类别索引

### 公式9: 辅助模型损失

$$
L_{\text{AUX}}(\phi) = \text{CE}[x | \hat{x}_\phi(z_s)] + \text{CE}[\hat{x}_\theta | \hat{x}_\phi(z_s)] = -\sum_c (x + \hat{x}_\theta(z_s))_c \log \hat{x}_\phi(z_s)_c
$$

**含义**: 辅助模型学习生成器的期望并保持接近教师

**符号说明**:
- $x$: 硬样本或软概率向量

### 公式10: [[Gradient Moment]] 评估指标

$$
\|\mathbb{E}_g[\nabla_\theta \log p_{\theta}^{\text{LLM}}(x)] - \mathbb{E}_q[\nabla_\theta \log p_{\theta}^{\text{LLM}}(x)]\|^2
$$

**含义**: 用参考 LLM 的损失梯度衡量样本与数据的分布距离

**符号说明**:
- $g$: 模型采样分布
- $q$: 数据分布
- $p_{\theta}^{\text{LLM}}$: 参考语言模型

---

## 关键图表

### Figure 1: D-MMD vs Teacher 性能对比

![[D-MMD_fig1.png]]

**说明**: D-MMD 文本生成器使用更少的 NFE（函数评估次数）即可匹配或超越教师模型。GPT-2 moment 指标显示 16-64 步即可达到最佳性能。

### Figure 2: 生成的文本样本

![[D-MMD_fig2.png]]

**说明**: 16 步 Masked D-MMD 生成的 1024 token 文本样本（未精选），展示流畅的语法和语义连贯性。

### Figure 3: Perplexity vs Gradient Moment

![[D-MMD_fig3.png]]

**说明**: Top-p 采样对两个指标的影响。Perplexity 随温度降低持续改善，但 Gradient Moment 在过低温度时会下降——说明低温度样本偏离真实数据分布。

### Table 1: CIFAR10 FID 结果

| Model | 4 | 8 | 16 | 32 | 64 | 128 | 256 | 512 | 1024 |
|--------|---|---|-----|-----|-----|------|------|------|-------|
| Uniform Teacher | - | 17.1 | 10.7 | 7.9 | 7.6 | 7.5 | 5.0 | - | 36.3 |
| **Uniform D-MMD** | - | **3.7** | **3.8** | 7.9 | - | - | - | - | **4.1** |
| Masked Teacher | - | 47.1 | 20.0 | 7.8 | 6.7 | 6.4 | 12.7 | 122.9 | - |
| **Masked D-MMD** | - | **3.8** | **3.5** | 5.3 | **3.8** | 11.1 | 22.3 | - | - |

**说明**: D-MMD 在 CIFAR10 上大幅超越教师模型。Uniform D-MMD 用 32 步达到 FID 3.7，Masked D-MMD 用 64 步达到 FID 3.5。

### Table 2: 文本生成 GPT-2 GM 结果

| Model | 8 | 16 | 32 | 64 | 128 | 256 | 512 |
|--------|-----|-----|-----|-----|------|------|------|
| Uniform Teacher (p=0.50) | 0.326 | 0.324 | 0.310 | 0.330 | 0.337 | 0.375 | 0.330 |
| **Uniform D-MMD (p=0.70)** | **0.316** | **0.313** | **0.307** | - | - | - | - |
| Masked Teacher (p=0.85) | 0.307 | 0.275 | 0.275 | 0.236 | 0.402 | 0.225 | 0.297 |
| **Masked D-MMD (p=0.85)** | **0.231** | 0.275 | - | - | 0.456 | - | - |
| AR Baseline | 0.061 | - | - | - | - | - | - |

**说明**: Masked D-MMD 仅用 16 步即可超越教师的 256 步性能（0.236 vs 0.225）。

### Table 3: Block 自回归扩散结果

| Model | 16 | 256 |
|--------|------|------|
| 256-Block Uniform Teacher (p=0.9) | 0.225 | 0.225 |
| **256-Block Uniform D-MMD (p=0.7)** | **0.225** | - |

**说明**: 16 步 D-MMD 完全匹配 256 步教师模型的性能，适合实际应用场景。

### Table 4: 与 Di4C 对比

| Method | NFE | FID |
|--------|-----|------|
| Di4C Teacher | 40 | 8.0 |
| Di4C (hybrid) | 20 | 9.5 |
| Di4C | 10 | 20.6 |
| Uniform Teacher | 512 | 7.6 |
| **Uniform D-MMD** | **8** | **5.0** |
| **Uniform D-MMD** | **16** | **4.1** |
| Masked Teacher | 512 | 6.7 |
| **Masked D-MMD** | **16** | **5.3** |

**说明**: D-MMD 仅用 8 步就超越 Di4C 的所有配置，显著提升效率和质量。

### Table 5: 与 SDTT 等方法对比

| Method | NFE | GPT-2 GM | GPT2 PPL | Sample Entropy |
|--------|-----|----------|----------|----------------|
| Duo + DCD | 4 | 108.2 | 4.82 | 5.13 |
| Duo + Di4C | 4 | 150.7 | 4.81 | 5.17 |
| MDLM + SDTT | 4 | 339.7 | 5.38 | 5.19 |
| FMLM | 4 | 76.4 | 5.05 | 5.18 |
| Masked Teacher | 256 | 0.293 | 20.3 | 4.60 |
| SDTT (reimpl.) | 4 | 0.820 | 72.1 | 5.57 |
| **Masked D-MMD** | **16** | **0.236** | **17.2** | **5.00** |

**说明**: D-MMD 的 GPT-2 GM 远低于其他方法，证明样本分布更接近真实数据。

### Table 6: 噪声输入的重要性

| D-MMD Masked | 4 | 8 | 16 | 32 | 64 |
|--------------|------|------|------|------|------|
| without noise (FID) | 151.0 | 37.0 | 14.7 | 7.7 | 6.0 |
| without noise (entropy) | 1.26 | 1.37 | 1.57 | 1.86 | 1.91 |
| **with noise (FID)** | **22.3** | **12.7** | **5.3** | **3.8** | **3.5** |
| **with noise (entropy)** | **1.01** | **1.29** | **1.53** | **1.76** | **1.83** |

**说明**: 噪声输入对 Masked D-MMD 至关重要，使生成器能够更好地塌缩其因子化输出分布。

---

## 算法

### Algorithm 1: Discrete MMD Training

```
输入: 学生生成器 x_hat_eta, 教师模型 x_hat_theta, 辅助模型 x_hat_phi
      训练步 i, 采样步 k, 数据集 D, 权重函数 w(s), 损失函数 L

1. s, delta_t ~ U(0,1), U(0,1/k)
2. t = min(1, s + delta_t)
3. 从数据集 D 采样并扩散得到 z_t
4. 生成概率向量 x_hat_eta(z_t)
5. 采样 x ~ Categorical(p = x_hat_eta(z_t))
6. 使用采样器得到 z_s | x, z_t (stopgrad on z_s)

7. if i 是偶数:
   最小化 L_GEN = L_s(x_hat_eta, x_hat_theta(z_s), z_s) - L_s(x_hat_eta, x_hat_phi(z_s), z_s)
8. else:
   可选: 使用软目标 x <- x_hat_eta(z_t) (仅 masked diffusion)
   最小化 L_AUX = L_s(x, x_hat_phi(z_s), z_s) + L_s(x_hat_theta, x_hat_phi(z_s), z_s)
```

---

## 实验

### 数据集

| 数据集 | 规模 | 特点 | 用途 |
|--------|------|------|------|
| CIFAR10 | 50K images | 32x32x3 像素 | 图像生成评估 |
| Open Web Text | ~8M docs | 网页文本 | 文本生成评估 |

### 实现细节

- **模型**: Masked / Uniform Diffusion Transformer
- **采样步**: 教师使用 512-1024 步，D-MMD 使用 4-64 步
- **优化**: 交替优化生成器和辅助模型
- **Top-p 采样**: Masked (p=0.85), Uniform (p=0.70)

---

## 批判性思考

### 优点
1. **理论清晰**: 将 MMD 从连续域推广到离散域，保持核心矩匹配思想
2. **效果显著**: 学生模型超越教师，打破传统蒸馏的认知
3. **指标创新**: Gradient Moment 比 generative perplexity 更真实反映样本质量

### 局限性
1. **噪声依赖**: Masked D-MMD 需要额外的噪声输入，Uniform 版本无需但效果略差
2. **步骤限制**: 极少步骤（如 1-4 步）时性能仍有下降空间
3. **条件生成**: 论文主要关注无条件生成，条件生成场景需进一步验证

### 潜在改进方向
1. 结合 [[Consistency Models]] 思想实现单步生成
2. 探索更好的噪声注入策略
3. 扩展到条件生成和大语言模型场景

### 可复现性评估
- [ ] 代码开源（暂未发布）
- [x] 训练细节完整（算法、公式明确）
- [x] 数据集可获取（CIFAR10, OWT）
- [x] 实验结果详细（多表对比）

---

## 关联笔记

### 基于
- [[Moment Matching Distillation|MMD]]: 连续扩散蒸馏的核心方法
- [[Masked Diffusion]]: 吸收态离散扩散过程
- [[Uniform Diffusion]]: 均匀转移离散扩散过程

### 对比
- [[SDTT]]: 渐进蒸馏方法，易导致模式丢失
- [[Di4C]]: 混合分布方法，混合数需指数增长
- [[DiMO]]: 单步蒸馏方法，不支持少步生成

### 方法相关
- [[Gradient Moment]]: 新提出的评估指标
- [[Discrete Diffusion Models]]: 研究对象
- [[Temperature Sampling]]: 结合蒸馏的采样策略

---

## 速查卡片

> [!summary] D-MMD: Discrete Moment Matching Distillation
> - **核心**: 将连续 MMD 推广到离散扩散，通过对数概率空间进行矩匹配
> - **方法**: min-max 对抗优化 + 交替训练生成器和辅助模型
> - **结果**: 16-64 步超越教师（CIFAR10 FID 3.5-4.1, Text GM 0.23）
> - **关键**: 噪声输入对 Masked D-MMD 重要

---

*笔记创建时间: 2026-04-13*