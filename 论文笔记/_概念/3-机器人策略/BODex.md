---
type: concept
aliases: [BODex, Bilevel Optimization for Dexterous Grasping]
---

# BODex

## 定义

BODex (Bilevel Optimization for Dexterous Grasping) 是一种使用双层优化框架进行灵巧抓取合成的方法，将抓取姿态生成建模为上层优化手部位姿、下层优化接触力的层次化问题。

## 数学形式

双层优化结构：

$$
\min_{\boldsymbol{g}} \mathcal{L}_{\text{upper}}(\boldsymbol{g}, \boldsymbol{f}^*(\boldsymbol{g}))
$$

其中下层问题：

$$
\boldsymbol{f}^*(\boldsymbol{g}) = \arg\min_{\boldsymbol{f}} \mathcal{L}_{\text{lower}}(\boldsymbol{f}; \boldsymbol{g}) \quad \text{s.t.} \quad \boldsymbol{f} \in \mathcal{F}
$$

## 核心要点

1. **层次化优化**: 上层更新手部位姿，下层优化接触力
2. **高效求解**: 使用GPU加速和QP求解器
3. **物理约束**: 自然融入力闭合和关节限制
4. **可扩展**: 支持不同手型和物体

## 代表工作

- [[UltraDexGrasp]]: 采用BODex的双层优化框架进行抓取合成

## 相关概念

- [[Grasp Synthesis]]
- [[Force Closure]]
- [[cuRobo]]