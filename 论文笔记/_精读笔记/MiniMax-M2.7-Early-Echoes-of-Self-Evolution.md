---
title: "MiniMax M2.7: Early Echoes of Self-Evolution"
authors: [MiniMax Team]
year: 2026
venue: MiniMax Official Blog
tags: [paper, agent, self-evolution, llm, software-engineering]
arxiv_html: https://www.minimax.io/news/minimax-m27-en
created: 2026-04-17
---

# MiniMax M2.7: Early Echoes of Self-Evolution

## 元信息

| 项目 | 内容 |
|------|------|
| 标题 | MiniMax M2.7: Early Echoes of Self-Evolution |
| 作者 | MiniMax Team |
| 机构 | MiniMax |
| 发表 | 2026-03-18 |
| 链接 | [Official Blog](https://www.minimax.io/news/minimax-m27-en) / [OpenRoom Code](https://github.com/MiniMax-AI/OpenRoom) |

---

## 一句话总结

> MiniMax M2.7 是首个深度参与自身演化的模型，能构建复杂 agent harnesses，在软件工程、专业办公和交互娱乐三大场景展现接近 SOTA 的性能。

---

## 核心贡献

1. **模型自我演化**: M2.7 能构建研究 agent harness，自主迭代优化自身性能（100+轮迭代实现30%提升）
2. **专业软件工程**: SWE-Pro 56.22%、VIBE-Pro 55.6%、Terminal Bench 2 57.0%，接近 Opus 水平
3. **复杂技能遵循**: 97% 遵循率处理 40+ 个复杂 skills（每个超过 2000 tokens）

---

## 问题背景

### 要解决的问题

如何让模型具备深度参与自身演化的能力，实现 AI 自我迭代优化，同时在真实生产环境中展现专业级软件工程和办公能力。

### 现有方法的局限

- 传统模型仅被动接受训练，无法主动参与迭代流程
- 缺乏处理复杂技能和长流程任务的能力
- 在真实生产环境调试中综合推理能力不足

### 本文的动机

通过构建 agent harness（包含 memory、skills、MCP 等），让模型能够：
1. 自主运行研究工作流
2. 收集反馈并迭代自身架构
3. 在多团队协作环境中保持稳定表现

---

## 方法详解

### 整体框架

```
研究想法 --> [Agent Harness] --> 实验执行 --> 反馈收集 --> 自我优化 --> 模型迭代
                ↓
         [Memory + Skills + MCP + Tools]
```

### 核心 Agent Harness 架构

#### 短期记忆 (Short-term Memory)
- 每轮迭代后生成 markdown 文件记录状态

#### 自反馈 (Self-feedback)
- 对当前轮次结果进行自我批评
- 提供下一轮优化方向

#### 自优化 (Self-optimization)
- 基于历史记忆和反馈链进行改进
- 优化采样参数（temperature、frequency penalty、presence penalty）
- 设计工作流指导规则

### 自我迭代闭环

$$
\text{迭代} = \text{分析失败} \rightarrow \text{规划变更} \rightarrow \text{修改代码} \rightarrow \text{运行评估} \rightarrow \text{对比结果} \rightarrow \text{决策保留/回滚}
$$

---

## 关键实验结果

### 软件工程基准

| 基准测试 | M2.7 | 对比模型 |
|----------|------|----------|
| SWE-Pro | 56.22% | 接近 Opus 最佳水平 |
| VIBE-Pro | 55.6% | 与 Opus 4.6 相当 |
| Terminal Bench 2 | 57.0% | - |
| SWE Multilingual | 76.5 | 显著优势 |
| Multi SWE Bench | 52.7 | - |
| NL2Repo | 39.8% | - |

### 专业办公能力

| 基准测试 | M2.7 | 对比 |
|----------|------|------|
| GDPval-AA ELO | 1495 | 开源模型最高，仅次于 Opus 4.6/Sonnet 4.6/GPT5.4 |
| Toolathon | 46.3% | 全球顶级 |
| MM Claw | 62.7% | 接近 Sonnet 4.6 |

### MLE Bench Lite（机器学习竞赛）

| 模型 | 平均奖牌率 |
|------|------------|
| Opus-4.6 | 75.7% |
| GPT-5.4 | 71.2% |
| Gemini-3.1 | 66.6% |
| **M2.7** | **66.6%** |

最佳成绩：9金5银1铜（22个竞赛，24小时迭代）

---

## 核心应用场景

### 1. 生产环境调试

- 关联监控指标与部署时间线进行因果推理
- 统计分析 trace 采样，提出精确假设
- 主动连接数据库验证根因
- 定位缺失索引迁移文件
- 使用非阻塞索引创建止血
- **成果**: 生产事故恢复时间降至3分钟以内

### 2. 金融分析（TSMC 案例）

- 自主阅读年报和 earnings call
- 跨引用多份研究报告
- 独立设计假设并构建收入预测模型
- 基于模板输出 PPT 和研究报告
- 输出可直接进入后续工作流

### 3. Agent Teams 多智能体协作

- 角色边界稳定锚定
- 主动挑战队友的逻辑和伦理盲点
- 在复杂状态机中自主决策
- 内部用于产品原型开发

---

## 批判性思考

### 优点

1. 首个实现深度自我演化的模型，开启 AI 自迭代范式
2. 真实生产环境综合推理能力强（调试、因果分析、SRE决策）
3. 高 skill 遵循率（97%），适应复杂异构环境

### 局限性

1. 自我演化仍需人类关键决策点干预（30%-50% 工作流覆盖率）
2. 论文未披露模型架构细节和训练方法
3. 低资源场景测试仅为初步探索

### 潜在改进方向

1. 向全自主演化过渡：协调数据构建、模型训练、推理架构、评估全链路
2. 提升工作流覆盖率从 30%-50% 到更高
3. 更复杂的多智能体协作范式探索

### 可复现性评估

- [x] 代码开源（OpenRoom）
- [ ] 预训练模型
- [ ] 训练细节完整
- [x] Demo 可用

---

## 关联项目

### 开源项目

| 项目 | 链接 | 说明 |
|------|------|------|
| OpenRoom | [GitHub](https://github.com/MiniMax-AI/OpenRoom) | 交互式娱乐 agent 演示 |
| OpenRoom Demo | [Website](https://www.openroom.ai/) | 在线体验 |

### 产品入口

| 产品 | 链接 |
|------|------|
| MiniMax Agent | https://agent.minimax.io |
| API Platform | https://platform.minimax.io |
| Coding Plan | https://platform.minimax.io/subscribe/coding-plan |

---

## 速查卡片

> [!summary] MiniMax M2.7
> - **核心**: 首个深度自我演化模型
> - **方法**: Agent Harness（Memory + Skills + MCP + Self-feedback）
> - **结果**: SWE-Pro 56.22%，GDPval-AA ELO 1495
> - **代码**: https://github.com/MiniMax-AI/OpenRoom

---

*笔记创建时间: 2026-04-17*