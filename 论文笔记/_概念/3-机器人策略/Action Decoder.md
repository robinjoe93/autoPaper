---
type: concept
aliases: [动作解码器]
---

# Action Decoder

## 定义

将单个潜在动作 token 解码为完整动作空间的轻量级解码器模块，用于高效的动作预测。

## 数学形式

$$
a = \text{Decoder}(a_{\text{in}})
$$

其中 $a_{\text{in}}$ 是主 Transformer 输出的单个潜在 token，$a$ 是完整动作序列（$N_a$ 个 token）。

## 核心要点

1. 避免在主 Transformer 中预测所有动作 token，实现约 5x 推理加速
2. 自回归解码：逐个生成动作 token
3. 允许同时多按键输入（如 4 个键盘键 + 鼠标移动 + 鼠标按钮）

## 代表工作

- [[P2P]]: 使用 Action Decoder 实现 20 Hz 实时游戏推理

## 相关概念

- [[Behavior Cloning]]
- [[Transformer]]
- [[Autoregressive Decomposition]]