---
title: "HY-World 2.0: A Multi-Modal World Model for Reconstructing, Generating, and Simulating 3D Worlds"
method_name: "HY-World"
authors: [Team HY-World, Chenjie Cao, Xuhui Zuo, Zhenwei Wang, Yisu Zhang, Junta Wu, Zhenyang Liu, Yuning Gong, Yang Liu, Bo Yuan, Chao Zhang, Coopers Li, Dongyuan Guo, Fan Yang, Haiyu Zhang, Hang Cao, Jianchen Zhu, Jiaxin Lin, Jie Xiao, Jihong Zhang, Junlin Yu, Lei Wang, Lifu Wang, Lilin Wang, Linus, Minghui Chen, Peng He, Penghao Zhao, Qi Chen, Rui Chen, Rui Shao, Sicong Liu, Wangchen Qin, Xiaochuan Niu, Xiang Yuan, Yi Sun, Yifei Tang, Yifu Sun, Yihang Lian, Yonghao Tan, Yuhong Liu, Yuyang Yin, Zhiyuan Min, Tengfei Wang, Chunchao Guo]
year: 2026
venue: arXiv
tags: [world-model, 3d-generation, 3d-reconstruction, 3d-gaussian-splatting, diffusion-model, panorama-generation, trajectory-planning]
zotero_collection: 6-3D视觉/World-Model
image_source: online
arxiv_html: https://arxiv.org/html/2604.14268v1
created: 2026-04-20
---

# 论文笔记：HY-World 2.0: A Multi-Modal World Model for Reconstructing, Generating, and Simulating 3D Worlds

## 元信息

| 项目 | 内容 |
|------|------|
| 机构 | Tencent Hunyuan |
| 日期 | April 2026 |
| 项目主页 | https://3d-models.hunyuan.tencent.com/world/ |
| 对比基线 | [[Marble]] |
| 链接 | [arXiv](https://arxiv.org/abs/2604.14268) / [Code](https://github.com/Tencent-Hunyuan/HY-World-2.0) |

---

## 一句话总结

> 首个开源的多模态世界模型，统一了从文本/单视图图像生成可探索3D世界与从多视图重建精确3D结构的能力，通过四阶段流水线实现高保真 [[3D Gaussian Splatting]] 场景生成。

---

## 核心贡献

1. **统一框架**: 首个开源系统性地统一"世界生成"与"世界重建"的离线3D世界模型，支持文本、单视图、多视图、视频等多种输入模态
2. **四阶段流水线**: 提出 Panorama Generation -> Trajectory Planning -> World Expansion -> World Composition 的完整生成流程
3. **高性能渲染平台**: 引入 [[WorldLens]] 高性能 3DGS 渲染平台，支持实时交互探索与角色支持

---

## 问题背景

### 要解决的问题
现有世界模型解决方案通常只能专注于"生成"或"重建"单一任务：
- 生成方法难以保持严格的重建精度
- 重建方法缺乏生成先验来补充未观测区域

### 现有方法的局限
- [[HY-World 1.0]] 等生成方法无法从多视图输入恢复精确3D结构
- VGGT、Depth Anything 等重建方法无法从稀疏输入（如文本）生成新内容
- 闭源模型 [[Marble]] 展现了统一能力，但开源社区缺乏类似系统

### 本文的动机
作者认为可以通过统一框架同时实现生成与重建：
- 对于稀疏输入执行世界生成（合成可导航 3DGS 场景）
- 对于密集多视图输入执行世界重建（恢复几何准确的3D结构）

---

## 方法详解

### 模型架构

HY-World 2.0 采用 **四阶段流水线** 架构：

- **输入**: 文本提示 $l$ 或单视图图像 $\mathbf{I}^{ref}$ 或多视图图像/视频
- **Stage I**: [[HY-Pano 2.0]] 用于全景生成，输出 $360^{\circ} \times 180^{\circ}$ 全景图
- **Stage II**: [[WorldNav]] 用于轨迹规划，生成多样化相机路径
- **Stage III**: [[WorldStereo 2.0]] 用于世界扩展，合成多视图关键帧
- **Stage IV**: [[WorldMirror 2.0]] + [[3D Gaussian Splatting]] 用于世界组成
- **输出**: 可导航的高保真 3DGS 场景

### 核心模块

#### Stage I: Panorama Generation (HY-Pano 2.0)

**设计动机**: 全景图捕获完整的 $360^{\circ}$ 视场，为大规模3D世界生成提供空间一致性基础

**具体实现**:
- 使用 [[Multi-Modal Diffusion Transformer]] (MMDiT) 进行隐式自适应映射
- 在统一潜在空间处理条件输入和全景目标，通过自注意力学习 perspective-to-ERP 变换
- 采用 circular padding + pixel blending 解决 ERP 边界不连续问题

**数据策略**:
- 整合真实世界全景图与 Unreal Engine 渲染的合成环境
- 严格过滤低质量样本（拼接瑕疵、拍摄设备曝光等）

#### Stage II: Trajectory Planning (WorldNav)

**设计动机**: 从全景图推导探索轨迹，最大化导航空间覆盖

**具体实现**:
- 使用 [[MoGe-2]] 进行几何感知初始化，构建全景点云 $\mathbf{P}^{pan}$
- 使用 [[Qwen3-VL]] + [[SAM 3]] 进行语义分割和导航分析
- 使用 [[Recast Navigation]] 构建 NavMesh 定义可穿越区域
- 设计五种轨迹模式：Regular、Surrounding、Reconstruct-Aware、Wandering、Aerial

#### Stage III: World Expansion (WorldStereo 2.0)

**设计动机**: 利用相机引导的视频生成合成广泛新视图用于世界扩展

**具体实现**:
- **Keyframe-VAE**: 在关键帧潜在空间进行生成，避免 Video-VAE 的时空压缩导致的质量退化
- **相机控制**: 结合 Plücker rays 和点云作为相机引导
- **Global-Geometric Memory (GGM)**: 维护全局一致的粗略场景结构
- **Spatial-Stereo Memory++ (SSM++)**: 强化局部对应和精细细节

**三阶段训练**:
- Domain-Adaption: 相机引导关键帧生成
- Middle-Training: 记忆机制训练
- Post-Train: 模型蒸馏加速推理

#### Stage IV: World Composition (WorldMirror 2.0)

**设计动机**: 将生成的关键帧序列重建为完整的 3DGS 场景

**具体实现**:
- 通过 WorldMirror 2.0 从多视图重建点云
- 使用定制的 3DGS 训练策略优化生成视图
- 引入 [[MaskGaussian]] 策略处理遮罩区域

---

## 关键公式

### 公式1: [[WorldStereo 2.0|目标视图点云投影]]

$$
\mathbf{P}^{tar}_{i}(x)\simeq\mathbf{R}^{c\rightarrow w}_{i}\mathrm{D}(x)\mathbf{K}^{-1}_{i}\hat{x},
$$

**含义**: 从参考视图点云投影到目标视图，用于相机控制引导

**符号说明**:
- $\mathbf{P}^{tar}_{i}(x)$: 目标视图 $i$ 在像素 $x$ 处的点云
- $\mathbf{R}^{c\rightarrow w}_{i}$: 相机到世界变换矩阵
- $\mathrm{D}(x)$: 参考视图在像素 $x$ 的单目深度估计
- $\mathbf{K}_{i}$: 目标视图 $i$ 的内参矩阵
- $\hat{x}$: 齐次像素坐标

### 公式2: [[Global-Geometric Memory|全局点云扩展]]

$$
\mathbf{P}^{glo}=[\mathbf{P}^{ref},\hat{\mathbf{P}}]\in\mathbb{R}^{(N+\hat{N})\times 3},
$$

**含义**: 构建扩展全局点云用于 GGM 训练，包含参考点云和随机采样新视图点云

**符号说明**:
- $\mathbf{P}^{glo}$: 全局点云
- $\mathbf{P}^{ref}$: 参考视图点云
- $\hat{\mathbf{P}}$: 从 $T_{g}$ 个新视图随机采样的额外点云

### 公式3: [[Distribution Matching Distillation|DMD蒸馏损失]]

$$
\nabla\mathcal{L}_{\text{DMD}}=-\mathbb{E}_{t}\left(\int\left(s_{\text{real}}(x_{t},t)-s_{\text{fake}}(x_{t},t)\right)\frac{dx_{t}}{d\theta}dz\right),
$$

**含义**: 通过近似 KL 散度蒸馏少步扩散学生模型

**符号说明**:
- $s_{\text{real}}(x_{t},t)$: 冻结的真实分数函数
- $s_{\text{fake}}(x_{t},t)$: 可训练的假分数函数
- $\theta$: 学生生成器参数

---

## 关键图表

### Figure 1: Versatile applications of HY-World 2.0

![Figure 1](https://arxiv.org/html/2604.14268v1/x1.png)

**说明**: 展示 HY-World 2.0 的多功能应用场景，统一世界生成（从文本/单视图合成可探索3D世界）和世界重建（从多视图恢复3D表示），赋能机器人仿真、游戏开发和环境建图。

### Figure 2: Architecture of HY-World 2.0

![Figure 2](https://arxiv.org/html/2604.14268v1/x2.png)

**说明**: 四阶段流水线架构：Panorama Generation -> Trajectory Planning -> World Expansion -> World Composition，每个阶段的核心模型/算法标注在底部。

### Figure 3: Panorama Generation Architecture

![Figure 3](https://arxiv.org/html/2604.14268v1/x3.png)

**说明**: HY-Pano 2.0 的全景生成框架，左侧展示主流程，右侧展示用于无缝全景生成的 circular padding（潜在空间）和 pixel blending（像素空间）策略。

### Figure 4: Scene Parsing for Trajectory Planning

![Figure 4](https://arxiv.org/html/2604.14268v1/x4.png)

**说明**: 轨迹规划的初始场景解析，获取全景点云、网格、语义掩码和 NavMesh，使用 MoGe-2、SAM 3、Recast Navigation 等方法。

### Figure 5: Five Trajectory Modes in WorldNav

![Figure 5](https://arxiv.org/html/2604.14268v1/x5.png)

**说明**: WorldNav 设计的五种轨迹模式：(a) Regular 用于一般视野扩展，(b) Surrounding 围绕重要物体，(c) Reconstruct-Aware 针对重建缺口，(d) Wandering 到达边界，(e) Aerial 俯视视角。

### Figure 6: Three Training Stages of WorldStereo 2.0

![Figure 6](https://arxiv.org/html/2604.14268v1/x6.png)

**说明**: WorldStereo 2.0 的三阶段训练：逐步实现相机控制、记忆一致性和快速推理。

### Figure 7: WorldStereo 2.0 Pipeline

![Figure 7](https://arxiv.org/html/2604.14268v1/x7.png)

**说明**: WorldStereo 2.0 总体流程：(a) 主 DiT 分支通过 SSM++ 增强细粒度一致性，(b) 相机控制分支通过全景点云（GGM）引导精确轨迹和几何一致性。

### Figure 8: Reconstruction with Different VAE

![Figure 8](https://arxiv.org/html/2604.14268v1/x8.png)

**说明**: Keyframe-VAE vs Video-VAE 的重建和新视图生成对比，Keyframe-VAE 在大视角变化下保持更好的外观一致性和生成质量。

### Figure 9: Keyframe-VAE vs Video-VAE Architecture

![Figure 9](https://arxiv.org/html/2604.14268v1/x9.png)

**说明**: Keyframe-VAE 仅进行空间压缩（避免时空压缩导致的运动模糊和几何扭曲），对 $(1+T_{kf})$ 个关键帧独立编码，$T_{kf} \ll T_{vid}$。

### Figure 10: Memory Training Data Construction

![Figure 10](https://arxiv.org/html/2604.14268v1/x10.png)

**说明**: WorldStereo 2.0 记忆训练数据构建：(a) GGM 训练的全局点云，(b) SSM++ 训练的轨迹检索策略（时间错位检索 vs 多轨迹检索）。

### Figure 11: RoPE Modification in SSM++

![Figure 11](https://arxiv.org/html/2604.14268v1/x11.png)

**说明**: SSM++ 中的 RoPE 修改，目标帧与检索参考视图水平拼接，每个检索视图继承其配对目标帧的时间索引。

### Figure 12: WorldMirror 2.0 Architecture

![Figure 12](https://arxiv.org/html/2604.14268v1/x12.png)

**说明**: WorldMirror 2.0 统一前馈模型架构，输入多视图图像及可选几何先验，同时预测稠密点云、深度图、表面法向量、相机参数和 3DGS 属性。

### Table 1: Trajectory Details of WorldNav

| 类型 | Regular | Surrounding | Recon-Aware | Wandering | Aerial | Total |
|------|---------|-------------|-------------|-----------|--------|-------|
| Max Number | 9 | 5 | 10 | 3 | 8 | 35 |
| Attached to Object | x | check | check | x | - | - |
| Iterative | x | x | check | x | x | - |

**说明**: WorldNav 各轨迹类型的详细信息，Aerial 包含 surrounding 和 wandering 轨迹的变体。

### Table 2: Video Generation Paradigms Comparison

| Paradigms | Native VDM | AR | WorldStereo 2.0 |
|-----------|------------|-----|------------------|
| Receptive Field | Bidirectional | Autoregressive | Bidirectional |
| Trajectory Length/Num | Long/Single | Long/Single | Medium/Multiple |
| Latent Space | Video Clip | Video Clip | Keyframe Image |
| Frame Quality | Medium | Poor | High |
| Frame Redundancy | High | High | Low |
| Precise Camera Control | Poor | Medium | Good |
| Consistency | Medium | Medium | Good |
| Efficiency | Low | Low | High |

**说明**: 不同视频生成范式对比，WorldStereo 2.0 在关键帧空间生成，兼顾质量、一致性和效率。

### Table 3: WorldMirror 1.0 vs 2.0

| Component | WorldMirror 1.0 | WorldMirror 2.0 |
|-----------|-----------------|------------------|
| Position Encoding | Absolute RoPE | Normalized RoPE |
| Depth Supervision | GT depth only | GT depth + GT/Pseudo normal |
| Invalid Pixel Modeling | Confidence only | Confidence + Depth mask head |
| Acceleration | None | Token/Frame SP + BF16 + FSDP |
| Data | Open-sourced | + Internal UE renderings |
| Pseudo-Label Enhancement | x | Normal pseudo-labels |
| Image Res./Num. Sampling | Independent | Token-budget dynamic |
| Curriculum Stages | 2 stages | 3 stages |
| Resolution Sampling | 100K-250K pixels | 50K-500K pixels |

**说明**: WorldMirror 2.0 的主要改进，包括模型架构、数据和训练策略三方面。

---

## 实验

### 数据集

| 数据集 | 规模 | 特点 | 用途 |
|--------|------|------|------|
| HY-Pano 数据集 | 大规模 | 真实全景 + UE合成 | 全景生成训练 |
| DL3DV-10K | 10K场景 | 多视图视频 | WorldStereo 训练 |
| TartanAir | 大规模 | SLAM数据集 | 记忆训练 |
| Internal UE Renderings | 大规模 | 多轨迹渲染 | 记忆训练 |
| Map-free Visual Relocalization | 多场景 | 无地图重定位 | 重建训练 |

### 实现细节

- **HY-Pano 2.0**: 基于 MMDiT，支持任意视角输入的隐式 perspective-to-ERP 映射
- **WorldStereo 2.0**: 基于预训练 Video DiT + 相机 Adapter，Keyframe-VAE 编码
- **WorldMirror 2.0**: Transformer backbone + DPT decoder heads，支持 50K-500K 分辨率
- **优化**: DMD 蒸馏为 4-step 模型，Token/Frame SP 加速

### 定量结果

#### HY-Pano 2.0 性能
在全景生成基准上达到开源最佳性能，显著优于之前方法

#### WorldStereo 2.0 性能
- 在单视图场景重建任务中达到高保真度
- 相机控制精度显著提升
- 记忆训练和蒸馏消融实验验证各模块贡献

#### WorldMirror 2.0 性能
- 点云重建精度达到开源最佳
- 深度估计、法向量估计显著改进
- 灵活分辨率推理能力

### 可视化结果

生成的 3DGS 场景具有：
- 高保真视觉质量
- 可导航的探索空间
- 几何一致性
- 与闭源 Marble 可比的性能

---

## 批判性思考

### 优点
1. **统一框架**: 首个开源系统统一生成与重建
2. **完整流水线**: 四阶段设计覆盖从全景到 3DGS 的全流程
3. **技术创新**: Keyframe-VAE、GGM、SSM++ 等多项创新
4. **开源贡献**: 全部模型权重、代码和技术细节公开

### 局限性
1. **计算开销**: 四阶段流水线计算量较大
2. **依赖外部模型**: 多处依赖 MoGe-2、SAM 3 等外部模型
3. **泛化性**: 主要验证室内/室外场景，极端场景未知

### 潜在改进方向
1. **端到端优化**: 探索更紧凑的统一模型
2. **实时生成**: 进一步加速推理
3. **在线世界模型**: 结合 HY-World 1.5 的在线能力

### 可复现性评估
- [x] 代码开源
- [x] 预训练模型
- [x] 训练细节完整
- [x] 数据集可获取（内部UE数据可能受限）

---

## 关联笔记

### 基于
- [[HY-World 1.0]]: 离线3D世界生成基础
- [[HY-World 1.5]]: 在线视频世界生成
- [[WorldStereo]]: 相机引导视频生成
- [[WorldMirror]]: 统一3D重建模型

### 对比
- [[Marble]]: 闭源商业产品，性能基准

### 方法相关
- [[3D Gaussian Splatting]]: 最终输出表示
- [[Diffusion Model]]: 生成核心
- [[Video Diffusion Model]]: WorldStereo 基础
- [[Keyframe-VAE]]: 高保真编码
- [[Global-Geometric Memory]]: 全局结构一致性
- [[Spatial-Stereo Memory]]: 局部细节一致性

### 硬件/数据相关
- [[Unreal Engine]]: 合成数据渲染
- [[MoGe-2]]: 单目几何估计
- [[SAM 3]]: 语义分割
- [[Recast Navigation]]: NavMesh生成

---

## 速查卡片

> [!summary] HY-World 2.0
> - **核心**: 首个开源多模态世界模型，统一生成与重建
> - **方法**: 四阶段流水线 (Panorama -> Trajectory -> Expansion -> Composition)
> - **结果**: 高保真可导航 3DGS 场景，性能媲美 Marble
> - **代码**: https://github.com/Tencent-Hunyuan/HY-World-2.0

---

*笔记创建时间: 2026-04-20*