---
title: "Train a Unified Multimodal Data Quality Classifier with Synthetic Data"
method_name: "UniFilter"
authors: [Weizhi Wang, Rongmei Lin, Shiyang Li, Colin Lockard, Ritesh Sarkhel, Sanket Lokegaonkar, Jingbo Shang, Xifeng Yan, Nasser Zalmout, Xian Li]
year: 2025
venue: arXiv
tags: [data-quality, multimodal-data, mllm-pretraining, synthetic-data, data-filtering]
zotero_collection: _待整理
image_source: online
arxiv_html: https://arxiv.org/html/2510.15162
created: 2026-04-01
---

# 论文笔记：Train a Unified Multimodal Data Quality Classifier with Synthetic Data

## 元信息

| 项目 | 内容 |
|------|------|
| 机构 | UC Santa Barbara / Amazon Stores Foundational AI / UC San Diego |
| 日期 | October 2025 |
| 项目主页 | https://victorwz.github.io/UniFilter/ |
| 对比基线 | [[DFN]], [[MLMFilter]] |
| 链接 | [arXiv](https://arxiv.org/abs/2510.15162) / [Code](https://github.com/Victorwz/UniFilter) |

---

## 一句话总结

> 提出 UniFilter，首个能统一处理 image-text caption 和 interleaved document 数据的多模态质量分类器，通过半合成数据训练，显著提升 MLLM 预训练效果。

---

## 核心贡献

1. **统一多模态数据过滤**: 首个能同时处理 image-text paired caption 和 interleaved document 数据的质量分类器，突破 CLIP-based 方法只能处理单图文对的限制
2. **半合成数据生成方法**: 使用真实图片 + Claude-3-Sonnet 生成的 4 级质量文本，构建规模化训练数据
3. **下游性能提升**: MLLM 在 UniFilter 筛选数据上预训练后，zero-shot/few-shot 和 SFT 后性能均显著提升

---

## 问题背景

### 要解决的问题

[[MLLM]] 持续预训练使用 image-text caption 和 interleaved document 数据混合，但**高质量 interleaved 数据筛选方法未被探索**。现有方法如 [[CLIPScore]] 只能处理单图文对，无法评估包含多图长文的 interleaved 文档。

### 现有方法的局限

- **CLIPScore/DFN**: 仅支持单图文对，基于图文相似度，无法处理 interleaved 文档
- **MLMFilter**: 虽基于 MLLM，但只能处理 caption 数据，且需 GPT-4V 生成昂贵的 100-way quality scores
- **无 interleaved 过滤器**: 文档级多模态数据质量评估完全空白

### 本文的动机

采用 MLLM 架构可统一处理两种数据类型：对 interleaved 数据，图像和文本按原始交错顺序重构；对 caption 数据，图像编码与文本嵌入拼接。通过输出浮点质量分数实现统一评估。

---

## 方法详解

### 模型架构

UniFilter 采用 **MLLM 分类架构**：

- **输入**: 图像 $I$ + 文本 $T$（caption 或 interleaved 文档）
- **Vision Encoder**: [[SigLIP]]-SO-400M (384px)
- **Visual Projector**: 2D [[Adaptive Average Pooling]] + MLP (144 tokens/image)
- **LLM Backbone**: Qwen-2.5-0.5B
- **输出头**: 新初始化的分类头 $\text{Embd\_Size} \times 1$
- **总参数**: ~0.5B

### 核心模块

#### 模块1: 统一多模态输入处理

**设计动机**: 支持 caption 和 interleaved 两种数据格式的统一处理

**具体实现**:
- **Caption 数据**: 图像编码 $\mathbf{V}$ 与文本嵌入 $\mathbf{T}$ 直接拼接
- **Interleaved 数据**: 图像和文本分别编码后按原始交错顺序重构序列
- 输入序列: $\mathbf{X} = [\mathbf{V}_1, \mathbf{T}_1, \mathbf{V}_2, \mathbf{T}_2, ...]$ 或 $[\mathbf{V}, \mathbf{T}]$

#### 模块2: 质量分数预测

**设计动机**: 将 MLLM 转为回归模型输出连续质量分数

**具体实现**:
- 移除原始语言模型头 ($\text{Embd\_Size} \times \text{Vocab\_Size}$)
- 新增分类头输出 scalar logit $s \in \mathbb{R}$
- 使用 [[MSE Loss]] 与合成质量标签对齐

#### 模块3: 高效架构设计

**设计动机**: 在 pre-training 数据规模上保持高吞吐量

**具体实现**:
- 选择 Qwen-2.5-0.5B (而非更大模型) 平衡性能与效率
- 使用 AvgPool 压缩图像 token (144/image) 降低序列长度
- 最终 UniFilter 达到 **130 samples/s** 吞吐量，媲美 DFN-CLIP 的 128 samples/s

---

## 关键公式

### 公式1: [[Quality Score Prediction|质量分数预测]]

$$
s = f_{\text{UniFilter}}(\mathbf{V}, \mathbf{T})
$$

**含义**: MLLM 输出的 scalar logit 表示输入多模态数据的质量分数

**符号说明**:
- $s$: 输出的质量分数 (scalar)
- $\mathbf{V}$: 视觉编码后的图像 token 序列
- $\mathbf{T}$: 文本嵌入序列
- $f_{\text{UniFilter}}$: UniFilter 模型函数

### 公式2: [[MSE Loss|训练损失]]

$$
\mathcal{L} = \frac{1}{N} \sum_{i=1}^{N} (s_i - y_i)^2
$$

**含义**: 使用 MSE 损失将预测分数与 4 级质量标签 (0,1,2,3) 对齐

**符号说明**:
- $\mathcal{L}$: 训练损失
- $s_i$: 第 $i$ 个样本的预测质量分数
- $y_i$: 第 $i$ 个样本的质量标签 (0=easy negative, 1=medium negative, 2=hard negative, 3=positive)
- $N$: 训练样本数

### 公式3: Interleaved 文档质量评估

$$
s_{\text{doc}} = f_{\text{UniFilter}}([\mathbf{V}_1, \mathbf{T}_1, \mathbf{V}_2, \mathbf{T}_2, ..., \mathbf{V}_k, \mathbf{T}_k])
$$

**含义**: 对包含 $k$ 个图像的 interleaved 文档，按原始顺序重构后整体评估质量

**符号说明**:
- $s_{\text{doc}}$: 文档级质量分数
- $\mathbf{V}_j$: 第 $j$ 个图像的视觉 token
- $\mathbf{T}_j$: 第 $j$ 个文本段的嵌入
- $k$: 文档中的图像数量

---

## 关键图表

### Figure 1: Semi-Synthetic Data Generation Pipeline

![Figure 1](https://arxiv.org/html/2510.15162v1/x6.png)

**说明**: 半合成数据生成流程。从 DataComp/OBELICS 采样真实图像，用 Claude-3-Sonnet 按 4 级质量要求生成对应文本，构建 sample-score 训练对。

### Figure 2: UniFilter Architecture

![Figure 2](https://arxiv.org/html/2510.15162v1/x7.png)

**说明**: UniFilter 统一架构。Left: 处理 image-text caption 数据；Right: 处理 interleaved document 数据。MLLM backbone 输出质量分数 logit。

### Figure 3: Efficiency vs Accuracy Trade-off

![Figure 3](https://arxiv.org/html/2510.15162v1/figs/efficiency_vs_acc.png)

**说明**: 不同架构配置的分类 F1 与推理速度对比。Qwen2.5-0.5B + SigLIP-SO400M + AvgPool(144) 达到最佳平衡点。

### Figure 4: Synthetic Data Generation (Caption)

![Figure 4](https://victorwz.github.io/UniFilter/static/images/synthetic_data_generation.png)

**说明**: Caption 数据的合成流程示意：采样图像 -> Claude-3 生成 4 级质量文本 -> 构建 training pairs。

### Figure 5: Data Quality Prompt Design

![Figure 5](https://victorwz.github.io/UniFilter/static/images/data_prompt.png)

**说明**: 4 级质量定义的 prompt 设计，用于指导 Claude-3-Sonnet 生成不同质量的文本。

### Figure 6: Caption Data Performance

![Figure 6](https://victorwz.github.io/UniFilter/static/images/caption_performance.png)

**说明**: 仅使用 caption 数据预训练的 MLLM zero-shot VQA 性能对比，UniFilter 筛选数据在 5 个 benchmark 上平均超越 DFN +2.6。

### Figure 7: Interleaved Data Performance

![Figure 7](https://victorwz.github.io/UniFilter/static/images/interleaved_performance.png)

**说明**: 混合 caption + interleaved 数据预训练的 MLLM few-shot ICL 性能。UniFilter 在 4-shot/8-shot 平均分别超越 DFN +0.7/+2.8。

### Figure 8: SFT Performance Scaling

![Figure 8](https://victorwz.github.io/UniFilter/static/images/scaling_sft.png)

**说明**: Visual SFT 后的下游性能。UniFilter 预训练的 MLLM 在 VQA 平均 +3.1，MMMU +1.5 提升。

### Table 1: Data Quality Requirements

| Quality Level | Caption Data Requirements | Interleaved Data Requirements |
|---------------|---------------------------|-------------------------------|
| Easy Negative (0) | 完全不相关的描述 | 文档不流畅，图文无关，无教育价值 |
| Medium Negative (1) | 有明显描述错误 | 可读但有写作错误，图文主题相关但不对齐 |
| Hard Negative (2) | 仅一个属性错误 | 有细微错误，图文部分相关但图像无助理解 |
| Positive (3) | 高质量详尽描述 | 高质量综合文档，图像位置恰当提供补充信息 |

**说明**: 4 级质量分类的定义标准，用于指导合成数据生成。

### Table 2: Architecture Ablation

| LLM | Vision Encoder | Projector | Tokens/Image | Resolution | Val Acc | Val F1 |
|-----|----------------|-----------|--------------|------------|---------|--------|
| Phi-3-3.8b | CLIP-L | MLP | 256 | 224 | 90.8 | 87.1 |
| Phi-3-3.8b | SigLIP-SO-400M | AvgPool+MLP | 144 | 384 | 91.9 | 88.5 |
| Gemma-2-2b | SigLIP-SO-400M | AvgPool+MLP | 144 | 384 | 88.2 | 83.1 |
| **Qwen2.5-1.5b** | SigLIP-SO-400M | AvgPool+MLP | 144 | 384 | **95.2** | **94.3** |
| Qwen2.5-0.5b | SigLIP-SO-400M | AvgPool+MLP | 144 | 384 | 94.8 | 93.8 |

**关键发现**: Qwen2.5 系列在质量分类任务上表现最优，0.5B 版本是性能效率最佳平衡点。

### Table 3: Caption-Only Pre-training Results

| Methods | GQA | VQA-v2 | VizWiz | OKVQA | TextVQA | Avg. |
|---------|-----|--------|--------|-------|---------|------|
| DFN | 25.8 | 39.6 | 21.6 | 26.0 | 30.7 | 28.7 |
| MLMFilter-ITM | 28.3 | 42.7 | 21.7 | 26.0 | 31.6 | 30.2 |
| MLMFilter-CTQ | 28.4 | 40.7 | 20.3 | 27.2 | 35.2 | 30.4 |
| **UniFilter** | **29.6** | **43.2** | **22.9** | **28.2** | 32.5 | **31.3** |

**说明**: 仅使用筛选 caption 数据预训练 5B tokens 的 zero-shot VQA 性能。UniFilter 在平均分上最优。

### Table 4: Mixed Caption+Interleaved Pre-training

| Methods | Shots | GQA | VQA-v2 | VizWiz | OKVQA | TextVQA | Avg. |
|---------|-------|-----|--------|--------|-------|---------|------|
| No Filtering | 0 | 17.6 | 22.5 | 12.2 | 23.9 | 29.1 | 21.1 |
| DFN | 0 | 21.8 | 36.6 | 16.7 | 20.6 | 30.4 | 25.2 |
| **UniFilter** | **0** | **22.9** | **37.8** | **22.4** | **25.1** | **33.9** | **28.4** |
| DFN | 4 | 40.9 | 60.0 | 39.2 | 43.6 | 43.6 | 45.5 |
| **UniFilter** | **4** | **42.2** | 59.7 | **40.6** | **44.8** | **43.5** | **46.2** |
| DFN | 8 | 41.0 | 61.5 | 45.9 | 44.9 | 43.9 | 47.4 |
| **UniFilter** | **8** | **42.0** | 60.8 | **56.3** | **46.4** | **45.5** | **50.2** |

**关键发现**: UniFilter 在 0-shot 上平均 +3.2，8-shot 上平均 +2.8 提升，证明 interleaved 数据质量筛选对 ICL 能力提升关键。

### Table 5: Visual SFT Results

| Interleaved Pretrain | GQA | VQA-v2 | VizWiz | OKVQA | TextVQA | VQA Avg. | POPE | MMMU | MMBench | MMStar |
|----------------------|-----|--------|--------|-------|---------|----------|------|------|---------|--------|
| No-Pretrain | 28.5 | 49.7 | 15.5 | 27.5 | 32.3 | 30.7 | 81.8 | 40.5 | 74.4 | 36.9 |
| DFN | 32.3 | 57.3 | 16.7 | 27.0 | 41.6 | 35.0 | 82.9 | 39.8 | 75.4 | 38.0 |
| **UniFilter** | **33.0** | **60.7** | **19.5** | **32.3** | **44.7** | **38.1** | **83.2** | **42.0** | **77.0** | **38.5** |

**关键发现**: UniFilter 预训练 + SFT 后，VQA 平均 +3.1，MMMU +1.5，MBench +1.6 提升，高质量预训练收益延续到 SFT 阶段。

---

## 实验

### 数据集

| 数据集 | 规模 | 特点 | 用途 |
|--------|------|------|------|
| DataComp-medium-128M | 128M | 噪声 web-crawled caption | Caption 过滤实验 |
| OBELICS | 141M | web-scale interleaved 文档 | Interleaved 过滤实验 |
| MSCOCO + Flickr30k | 4k | 高质量人工标注 caption | 合成数据补充 |
| LLaVA-1.5 + ShareGPT4V | 575k | visual instruction data | Visual SFT |

### 实现细节

- **Vision Encoder**: SigLIP-SO-400M (384px, frozen)
- **Projector**: 2D AvgPool (输出 144 tokens) + MLP (可训练)
- **LLM Backbone**: Qwen-2.5-0.5B (可训练)
- **训练数据**: 80k 合成样本 (40k caption + 40k interleaved) + 4k MSCOCO/Flickr
- **训练轮数**: 10 epochs
- **损失函数**: MSE Loss
- **推理吞吐**: 130 samples/s

### 可视化结果

高质量 interleaved 文档筛选显著提升 MLLM 的 few-shot ICL 能力，尤其在 VizWiz (+10.4) 等需要强 instruction following 的任务上。

---

## 批判性思考

### 优点

1. **首创性**: 首个统一处理 caption 和 interleaved 数据的质量分类器
2. **高效设计**: 0.5B 参数达到媲美 DFN-CLIP 的推理速度，实用性强
3. **合成数据创新**: 半合成方法避免昂贵人工标注，质量可控可扩展
4. **全面验证**: 从预训练到 SFT 全链路验证，下游收益明确

### 局限性

1. **依赖 Claude-3**: 合成数据生成依赖闭源模型，存在成本和可复现性风险
2. **4 级分类简化**: 相比 MLMFilter 的 100-way score，质量粒度较粗
3. **仅验证小模型**: 主要实验基于 Phi-3-3.8B，大规模 MLLM 验证待补充
4. **未见视频数据**: 未扩展到 video-text interleaved 数据

### 潜在改进方向

1. **开源合成引擎**: 用开源 MLLM 替代 Claude-3 降低依赖
2. **细粒度评分**: 扩展到更精细的质量等级 (如 0-10)
3. **视频数据支持**: 扩展 UniFilter 到 video-text interleaved 场景
4. **主动学习**: 结合人工标注迭代优化分类器

### 可复现性评估

- [x] 代码开源 (GitHub)
- [x] 模型开源 (HuggingFace: UniFilter-Qwen2.5-1.5B)
- [x] 训练数据开源 (HuggingFace: unifilter_train_data)
- [x] 筛选数据集开源 (OBELICS-HQ-5M)
- [x] 训练细节完整

---

## 关联笔记

### 基于

- [[CLIPScore]]: 原有 caption 过滤基线
- [[DFN]]: SOTA CLIP-based 过滤方法
- [[MLMFilter]]: MLLM-based caption 过滤方法

### 对比

- [[DFN]]: 只能处理 caption 数据
- [[MLMFilter]]: 需要 GPT-4V 生成昂贵标注

### 方法相关

- [[SigLIP]]: Vision Encoder
- [[Adaptive Average Pooling]]: Visual Projector 压缩
- [[MSE Loss]]: 训练目标
- [[MLLM]]: 基础架构
- [[In-Context Learning]]: Interleaved 数据提升的目标能力

### 数据相关

- [[DataComp]]: Caption 数据源
- [[OBELICS]]: Interleaved 数据源

---

## 速查卡片

> [!summary] UniFilter
> - **核心**: 统一多模态数据质量分类器 (caption + interleaved)
> - **方法**: MLLM 架构 + 半合成 4 级质量训练数据
> - **结果**: Zero-shot +3.2, 8-shot +2.8, SFT +3.1 VQA 提升
> - **代码**: https://github.com/Victorwz/UniFilter

---

*笔记创建时间: 2026-04-01*