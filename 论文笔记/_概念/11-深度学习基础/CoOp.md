---
type: concept
aliases: [CoOp, Learning to Prompt]
category: 深度学习基础
---

# CoOp

## 定义

学习 prompt 的方法，通过优化可学习的 prompt tokens 替代手工设计的文本 prompt。最初用于 CLIP 的 few-shot 适应。

## 核心要点

1. **可学习 tokens**: 在文本 prompt 前添加 learnable tokens
2. **固定模板**: 保留类别名，添加 prefix tokens
3. **端到端**: 与下游任务一起优化
4. **参数高效**: 只需优化少量 tokens

## 数学形式

Prompt 形式：

$$
\text{Prompt} = [p_1, p_2, ..., p_M, \text{CLASS\_NAME}]
$$

**符号说明**:
- $p_i$: 可学习的 token embedding
- $M$: prompt 长度（通常 4-16）

## 在机器人控制中的应用

- **ORCA 的 baseline**: 使用 CoOp 式的 learnable prefix
- **局限**: 仍固定任务名，不如 ORCA 的纯 learnable task prompt 灵活

## 与 ORCA 的对比

| 方法 | Prompt 内容 | 灵活性 |
|------|-------------|--------|
| CoOp | learnable prefix + 固定任务名 | 有限 |
| ORCA task prompt | 纯 learnable tokens | 高（完全隐式学习） |

## 代表工作

- [[ORCA]]: CoOp 作为 baseline 对比
- Learning to Prompt for Vision-Language Models (Zhou et al., 2022)

## 相关概念

- [[CLIP]]
- [[Task Prompt]]
- [[Prompt Learning]]