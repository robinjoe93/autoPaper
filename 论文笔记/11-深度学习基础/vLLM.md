---
title: "Efficient Memory Management for Large Language Model Serving with PagedAttention"
method_name: "vLLM"
authors: [Woosuk Kwon, Zhuohan Li, Siyuan Zhuang, Ying Sheng, Lianmin Zheng, Cody Hao Yu, Joseph E. Gonzalez, Hao Zhang, Ion Stoica]
year: 2023
venue: SOSP 2023
tags: [llm-serving, inference-optimization, memory-management, kv-cache, paged-attention, transformer]
zotero_collection: _待整理
image_source: online
arxiv_id: 2309.06180
created: 2026-04-01
---

# 论文笔记：Efficient Memory Management for Large Language Model Serving with PagedAttention

## 元信息

| 项目 | 内容 |
|------|------|
| 机构 | UC Berkeley, Stanford University, UC San Diego |
| 日期 | September 2023 |
| 会议 | SOSP 2023 (ACM SIGOPS 29th Symposium on Operating Systems Principles) |
| DOI | 10.1145/3600006.3613165 |
| 项目主页 | https://vllm.ai |
| 对比基线 | [[FasterTransformer]], [[Orca]] |
| 链接 | [arXiv](https://arxiv.org/abs/2309.06180) / [Code](https://github.com/vllm-project/vllm) |

---

## 一句话总结

> 提出受操作系统虚拟内存启发的 PagedAttention 算法，将 [[KV Cache]] 分页存储实现近零内存浪费，构建 vLLM 系统 2-4x 提升吞吐量。

---

## 核心贡献

1. **PagedAttention 算法**: 受操作系统虚拟内存分页技术启发，将 KV cache 分块存储在非连续内存空间，消除内存碎片化问题
2. **vLLM 系统**: 高吞吐量分布式 LLM 服务引擎，实现近零 KV cache 内存浪费和灵活的 KV cache 共享
3. **系统级创新**: 块级内存管理与抢占式请求调度协同设计，支持多种解码算法（并行采样、beam search）

---

## 问题背景

### 要解决的问题

[[LLM Serving]] 系统需要批处理多个请求以提高吞吐量，但 [[KV Cache]] 内存管理效率低下严重限制了批次大小。

### 现有方法的局限

1. **内存碎片化**: 现有系统将 KV cache 存储在连续内存空间，导致严重的内部和外部碎片
   - 预分配最大长度内存（如 2048 tokens）造成内部碎片
   - 不同请求预分配大小不同造成外部碎片
   - 实测显示现有系统仅 20.4%-38.2% 内存用于实际 token 状态

2. **无法共享内存**: beam search、parallel sampling 等场景需要多个序列共享 KV cache，但连续存储方式无法实现

3. **动态增长的 KV cache**: 其大小随生成过程动态增长，生命周期和长度不可预知

### 本文的动机

操作系统通过虚拟内存和分页技术解决内存碎片和共享问题，将这一思想迁移到 LLM serving 的 KV cache 管理：
- blocks = pages
- tokens = bytes
- requests = processes

---

## 方法详解

### 系统架构

vLLM 采用 **分布式服务引擎** 架构：
- **输入**: 用户请求（prompt tokens）
- **核心算法**: [[PagedAttention]] - 分页式注意力计算
- **内存管理**: 块级分配器 + 虚拟内存映射
- **调度器**: 抢占式请求调度（preemption + swapping）
- **输出**: 生成的 token 序列
- **支持模型**: GPT, OPT, LLaMA 等主流 LLM

### 核心模块

#### 模块1: PagedAttention 算法

**设计动机**: 将 [[KV Cache]] 分块存储，实现非连续内存访问

**具体实现**:
- 将每个请求的 KV cache 分割为固定大小的 **block**（页）
- 每个 block 存储固定数量 token 的 key 和 value 向量
- block 大小 = `block_size`（如 16 tokens）
- 使用 **block table** 维护逻辑 block 到物理 block 的映射
- 消除内部碎片（按需分配小 block）
- 消除外部碎片（所有 block 大小相同）

**Block 结构**:
- 物理 block: GPU 内存中的实际存储单元
- 逻辑 block: 请求视角的连续 block 序列
- block table: 类似 OS 的页表，映射逻辑->物理

#### 模块2: KV Cache 共享机制

**设计动机**: 支持 beam search、parallel sampling 的内存共享

**具体实现**:
- 使用 **引用计数** 机制跟踪每个物理 block 的使用情况
- 不同序列可通过 block table 引用同一物理 block
- **Copy-on-Write**: 当需要修改共享 block 时，复制后修改
- 共享粒度: block 级别（而非整个 sequence）

#### 模块3: 内存管理器

**设计动机**: 高效分配和回收 block

**具体实现**:
- 维护空闲 block 池
- 按需分配 block（无需预分配最大长度）
- 引用计数为 0 的 block 自动回收
- 支持跨请求的 block 共享

#### 模块4: 抢占式调度器

**设计动机**: 内存不足时的请求处理策略

**具体实现**:
- 监控 block 使用情况
- 内存压力大时触发抢占
- **Preemption 策略**: 暂停部分请求，释放其 block
- **Swapping 策略**: 将被抢占请求的 KV cache 换出到 CPU 内存
- 请求恢复时重新换入 KV cache

---

## 关键公式

### 公式1: [[Autoregressive Decomposition|自回归分解]]

$$
P(x_1, \ldots, x_n) = \prod_{i=1}^{n} P(x_i \mid x_1, \ldots, x_{i-1})
$$

**含义**: 语言模型将序列概率分解为条件概率的乘积，这是 Transformer 自回归生成的基础。

**符号说明**:
- $x_i$: 第 $i$ 个 token
- $P(x_i \mid x_1, \ldots, x_{i-1})$: 给定历史序列预测下一个 token 的概率

### 公式2: [[KV Cache|KV Cache 计算]]

$$
K_i = W_K \cdot h_i, \quad V_i = W_V \cdot h_i
$$

**含义**: 每个 token 位置的 key 和 value 向量由当前隐藏状态线性变换得到。

**符号说明**:
- $h_i$: 第 $i$ 个 token 的隐藏状态向量
- $W_K, W_V$: Key 和 Value 的投影矩阵
- $K_i, V_i$: 第 $i$ 个位置的 KV cache 元素

### 公式3: [[Attention Score|注意力分数计算]]

$$
\text{Attention}(Q_i, K, V) = \sum_{j=1}^{i} \frac{\exp(Q_i \cdot K_j)}{\sum_{k=1}^{i} \exp(Q_i \cdot K_k)} \cdot V_j
$$

**含义**: 计算当前 token 对历史所有 token 的注意力权重，加权求和得到输出。

**符号说明**:
- $Q_i$: 当前 token 的 Query 向量
- $K_j, V_j$: 第 $j$ 个历史 token 的 Key/Value 向量
- 分母: softmax 归一化项

### 公式4: [[PagedAttention|分页注意力]]

$$
\text{PagedAttention}(Q_i, K_{\text{blocks}}, V_{\text{blocks}}, \text{table}) = \sum_{b} \sum_{j \in \text{tokens}(b)} \frac{\exp(Q_i \cdot K_j^b)}{\sum_{k} \exp(Q_i \cdot K_k)} \cdot V_j^b
$$

**含义**: PagedAttention 通过 block table 累加跨多个物理 block 的注意力计算结果。

**符号说明**:
- $K_{\text{blocks}}, V_{\text{blocks}}$: 分块存储的 KV cache
- $\text{table}$: block table（逻辑->物理映射）
- $b$: 物理 block 索引
- $\text{tokens}(b)$: block $b$ 中存储的 token 集合

### 公式5: 内存效率分析

$$
\text{Memory Efficiency} = \frac{\text{Actual KV Cache}}{\text{Allocated Memory}} \approx 100\%
$$

**含义**: vLLM 实现近零内存浪费，相比现有系统的 20%-38% 效率大幅提升。

---

## 关键图表

### Figure 1: Memory Layout / 内存布局

![Figure 1](https://ar5iv.org/html/2309.06180/assets/x2.png)

**说明**: 13B 参数 LLM 在 NVIDIA A100 上的内存分布。模型权重占 65%，KV cache 占约 30%，激活值占少量。右侧展示 vLLM 平滑了现有系统的 KV cache 急剧增长曲线，显著提升吞吐量。

### Figure 2: Memory Waste Comparison / 内存浪费对比

![Figure 2](https://ar5iv.org/html/2309.06180/assets/x3.png)

**说明**: 现有系统（FasterTransformer, Orca）的内存浪费百分比对比。仅 20.4%-38.2% 的 KV cache 内存用于实际存储 token 状态，其余被碎片和预分配浪费。

### Figure 3: PagedAttention Overview / PagedAttention 概览

![Figure 3](https://ar5iv.org/html/2309.06180/assets/x4.png)

**说明**: PagedAttention 将 KV cache 分块存储，通过 block table 映射逻辑序列到物理 block。类比 OS 的虚拟内存：requests=processes, blocks=pages, tokens=bytes。

### Figure 4: KV Cache Sharing / KV Cache 共享

![Figure 4](https://ar5iv.org/html/2309.06180/assets/x5.png)

**说明**: 展示并行采样和 beam search 场景下的 KV cache 共享机制。多个序列通过引用计数共享相同 block，Copy-on-Write 机制处理分歧点。

### Figure 5: vLLM System Architecture / vLLM 系统架构

![Figure 5](https://ar5iv.org/html/2309.06180/assets/x6.png)

**说明**: vLLM 的整体架构：Scheduler 调度请求，Block Manager 管理 KV cache，Worker 执行模型推理，KV Cache Manager 处理换入换出。

### Figure 6: Throughput Comparison / 吞吐量对比

![Figure 6](https://ar5iv.org/html/2309.06180/assets/x7.png)

**说明**: 不同模型（OPT, LLaMA）上 vLLM 与 FasterTransformer、Orca 的吞吐量对比。vLLM 实现 2-4x 提升，且在更长序列、更大模型上优势更明显。

### Figure 7: Latency-Throughput Tradeoff / 延迟-吞吐权衡

![Figure 7](https://ar5iv.org/html/2309.06180/assets/x8.png)

**说明**: 延迟-吞吐曲线对比。vLLM 在相同延迟约束下实现更高吞吐，曲线更优。

### Figure 8: Sequence Length Impact / 序列长度影响

![Figure 8](https://ar5iv.org/html/2309.06180/assets/x9.png)

**说明**: 不同输入/输出序列长度下的性能对比。vLLM 在长序列场景优势更显著。

### Figure 9: Decoding Algorithm Impact / 解码算法影响

![Figure 9](https://ar5iv.org/html/2309.06180/assets/x10.png)

**说明**: 不同解码算法（basic sampling, parallel sampling, beam search）下的性能。beam search 和 parallel sampling 场景 vLLM 优势最大，得益于 KV cache 共享。

### Figure 10: Memory Waste Breakdown / 内存浪费分解

![Figure 10](https://ar5iv.org/html/2309.06180/assets/x11.png)

**说明**: 内存浪费来源分析：内部碎片 vs 外部碎片。现有系统两者都严重，vLLM 消除两者。

### Figure 11: Kernel Optimization / 内核优化

![Figure 11](https://ar5iv.org/html/2309.06180/assets/x12.png)

**说明**: PagedAttention CUDA kernel 的优化策略，减少非连续内存访问的性能开销。

### Figure 12: Preemption Analysis / 抢占策略分析

![Figure 12](https://ar5iv.org/html/2309.06180/assets/x13.png)

**说明**: 不同抢占策略（recompute vs swap）的对比分析。

### Figure 13: Distributed Serving / 分布式服务

![Figure 13](https://ar5iv.org/html/2309.06180/assets/x14.png)

**说明**: 多 GPU 分布式场景下的性能表现。

### Figure 14: Model Size Impact / 模型大小影响

![Figure 14](https://ar5iv.org/html/2309.06180/assets/x15.png)

**说明**: 不同模型参数规模（7B, 13B, 66B）下的吞吐量对比。

### Figure 15: Request Rate Impact / 请求速率影响

![Figure 15](https://ar5iv.org/html/2309.06180/assets/x16.png)

**说明**: 不同请求速率下的系统表现。

### Figure 16: Sharing Efficiency / 共享效率

![Figure 16](https://ar5iv.org/html/2309.06180/assets/x17.png)

**说明**: KV cache 共享带来的内存节省量化分析。

### Figure 17: Block Size Impact / Block 大小影响

![Figure 17](https://ar5iv.org/html/2309.06180/assets/x18.png)

**说明**: 不同 block_size 设置对性能和内存的影响。

### Figure 18: Comparison with HuggingFace / 与 HuggingFace 对比

![Figure 18](https://ar5iv.org/html/2309.06180/assets/x19.png)

**说明**: vLLM 与 HuggingFace Transformers 的吞吐量对比。

### Figure 19: Scalability Analysis / 可扩展性分析

![Figure 19](https://ar5iv.org/html/2309.06180/assets/x20.png)

**说明**: 系统在不同负载下的可扩展性表现。

### Table 1: End-to-End Performance / 端到端性能

| System | Throughput (req/s) | Latency (ms) | Memory Efficiency |
|--------|-------------------|--------------|-------------------|
| HuggingFace | 2.2 | 150 | ~30% |
| FasterTransformer | 4.5 | 120 | 38.2% |
| Orca | 6.0 | 100 | 20.4% |
| **vLLM** | **14-24** | **100** | **~100%** |

**说明**: vLLM 在相同延迟约束下实现 2-4x 吞吐提升，内存效率接近 100%。

---

## 实验

### 数据集

| 数据集 | 规模 | 特点 | 用途 |
|--------|------|------|------|
| ShareGPT | 真实用户对话 | 变长序列 | 性能评估 |
| Alpaca | 指令数据 | 固定格式 | 基础测试 |

### 实现细节

- **支持模型**: OPT (6.7B-66B), LLaMA (7B-65B), GPT-3
- **硬件**: NVIDIA A100 (40GB/80GB), A10G
- **Block Size**: 16 tokens（默认）
- **并行策略**: Tensor Parallelism 支持多 GPU
- **优化**: CUDA kernel 优化非连续内存访问

### 主要实验结果

1. **吞吐量**: 2-4x 提升（vs FasterTransformer, Orca）
2. **长序列优势**: 更显著（内存碎片问题更严重）
3. **Beam Search**: 最大收益（共享 KV cache）
4. **内存效率**: ~100%（vs 20%-38%）

---

## 批判性思考

### 优点

1. **创新性强**: 成功迁移 OS 虚拟内存思想到 LLM serving
2. **工程完善**: 系统级设计（调度+内存管理+kernel优化）完整
3. **性能显著**: 2-4x 实测提升，工业价值明确
4. **开源贡献**: 代码开源，社区广泛采用

### 局限性

1. **Block Size 权衡**: 太小增加映射开销，太大仍有内部碎片
2. **Swap 开销**: 抢占换出到 CPU 有额外延迟
3. **单请求无收益**: 短序列/单请求场景优势不明显

### 潜在改进方向

1. **动态 Block Size**: 根据序列特性自适应调整
2. **更高效 Swap**: GPU Direct Storage 减少换出开销
3. **多层 KV Cache**: 分层存储热/冷 KV cache

### 可复现性评估

- [x] 代码开源
- [x] 预训练模型支持
- [x] 训练细节完整
- [x] 数据集可获取

---

## 关联笔记

### 基于
- [[Transformer]]: 基于 Transformer 的 KV cache 机制
- [[Virtual Memory]]: 借鉴操作系统虚拟内存概念
- [[KV Cache]]: 解决的核心对象

### 对比
- [[FasterTransformer]]: 主要对比基线，NVIDIA 的高效推理框架
- [[Orca]]: 对比基线，迭代级调度系统
- [[HuggingFace Transformers]]: 原始框架对比

### 方法相关
- [[PagedAttention]]: 本文核心算法
- [[LLM Serving]]: 应用领域
- [[Inference Optimization]]: 推理优化技术
- [[Memory Management]]: 系统级内存管理
- [[Beam Search]]: 支持的高级解码算法

---

## 速查卡片

> [!summary] vLLM - Efficient LLM Serving with PagedAttention
> - **核心**: PagedAttention 分页 KV cache，近零内存浪费
> - **方法**: Block 级内存管理 + 抢占式调度 + KV cache 共享
> - **结果**: 2-4x 吞吐提升，beam search 场景最优
> - **代码**: https://github.com/vllm-project/vllm

---

*笔记创建时间: 2026-04-01*