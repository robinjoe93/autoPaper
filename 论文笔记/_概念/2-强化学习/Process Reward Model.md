---
type: concept
aliases: [PRM, 过程奖励模型]
---

# Process Reward Model

## 定义

一种提供步骤级（step-level）密集反馈的奖励模型，用于强化学习训练中识别和评估推理过程的每一步正确性。

## 核心要点

1. **步骤级评估**: 对每个推理步骤 $s_i$ 进行正确性判断 $v_i \in \{correct, incorrect\}$
2. **密集反馈**: 相比 outcome-level 奖励提供更细粒度的信用分配
3. **生成式变体**: PRM-CoT 输出验证理由 $\tau_i$ + 判断 $v_i$ + 最终 verdict $y$

## 数学形式

验证输出格式：
$$
v = (v_1, v_2, ..., v_n, y)
$$

其中 $v_i$ 为步骤 $i$ 的判断，$y$ 为最终 verdict。

## 代表工作

- [[SPARK]]: 无参考 PRM 训练框架
- [[PRIME]]: 隐式奖励的过程强化
- [[TANGO]]: 联合训练 generator 和 verifier
- PRM800K: 人工标注的步骤级数据集 (Lightman et al., 2023)

## 相关概念

- [[Outcome Reward Model|ORM]]: 仅提供 outcome-level 奖励
- [[RLVR]]: 基于可验证奖励的强化学习
- [[Self-Consistency]]: 用于生成 PRM 训练数据