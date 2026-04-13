---
type: concept
aliases: [三维基础模型, 3D Generative Foundation Model]
---

# 3D Foundation Model

## 定义
在大规模 3D 数据集上预训练的生成模型，具有强大的跨类别 3D 形状生成能力。

## 核心要点
1. **架构**: VAE encoder/decoder + Diffusion/Flow-Matching generator（latent-generative pipeline）
2. **训练数据**: 大规模 3D asset 数据集（如 Objaverse、ShapeNet）
3. **能力**: 文本到 3D、类别无关生成、形状编辑
4. **代表模型**: [[DORA]]、Clay、Michibichi

## Latent-Generative Pipeline

VAE 编码：$\boldsymbol{x} = \mathcal{E}(\boldsymbol{S})$（shape → latent）

生成器采样：Flow Matching / Diffusion

VAE 解码：$\boldsymbol{S} = \mathcal{D}(\boldsymbol{x})$（latent → shape）

## 代表工作
- [[LaS-Comp]]: 利用 DORA 先验做 shape completion
- DORA: 3D VAE + Flow Matching
- Clay、Michibichi: 其他 3D foundation models

## 相关概念
- [[Shape Completion]]
- [[DORA]]
- [[Flow Matching]]
- [[VAE]]
- [[Foundation Models]]