---
type: concept
aliases: [力闭合, Force-Closure]
---

# Force Closure

## 定义

力闭合（Force Closure）是指机器人手通过接触点施加的力能够抵抗任意外力和力矩的状态。一个抓取被称为力闭合的，当且仅当对于任何外力/力矩，都存在一组接触力能够平衡它。

## 数学形式

力闭合的充要条件：Grasp Wrench Space (GWS) 包含原点的一个邻域。

$$
0 \in \mathrm{int}(\mathcal{W})
$$

其中 $\mathcal{W}$ 是grasp wrench space：

$$
\mathcal{W} = \left\{ \boldsymbol{w} \;\middle|\; \boldsymbol{w} = \sum_{i=1}^{k} \boldsymbol{G}_i \boldsymbol{f}_i,~\boldsymbol{f}_i \in \mathcal{F}_i \right\}
$$

## 核心要点

1. **摩擦锥约束**: 接触力必须满足摩擦锥约束，不能无限大
2. **最小法向力**: 可以计算出抵抗给定外力所需的最小法向力
3. **抓取质量指标**: 常用grasp wrench space的体积或最小抵抗力来衡量抓取质量
4. **不同接触模型**: 点接触、软手指、硬手指等模型对应不同的力闭合条件

## 代表工作

- [[UltraDexGrasp]]: 使用力闭合作为抓取合成的优化目标之一
- [[DexGraspNet]]: 大规模灵巧抓取数据集，基于力闭合生成抓取

## 相关概念

- [[Grasp Synthesis]]
- [[Friction Cone]]
- [[Wrench]]