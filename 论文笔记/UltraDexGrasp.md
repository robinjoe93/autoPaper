---
title: "UltraDexGrasp: Learning Universal Dexterous Grasping for Bimanual Robots with Synthetic Data"
method_name: "UltraDexGrasp"
authors: [Sizhe Yang, Yiman Xie, Zhixuan Liang, Yang Tian, Jia Zeng, Dahua Lin, Jiangmiao Pang]
year: 2026
venue: arXiv
tags: [dexterous-grasping, bimanual-robotics, sim-to-real, synthetic-data, grasp-synthesis]
zotero_collection: 3-Robotics/1-VLX/VLA
image_source: online
arxiv_html: https://arxiv.org/html/2603.05312v1
created: 2026-04-03
---

# 论文笔记：UltraDexGrasp

## 元信息

| 项目 | 内容 |
|------|------|
| 机构 | Shanghai AI Laboratory, CUHK, Zhejiang University, HKU, PKU |
| 日期 | March 2026 |
| 项目主页 | [yangsizhe.github.io/ultradexgrasp](https://yangsizhe.github.io/ultradexgrasp/) |
| 对比基线 | [[DP3]], [[DexGraspNet]] |
| 链接 | [arXiv](https://arxiv.org/abs/2603.05312) / [Code](https://github.com/InternRobotics/UltraDexGrasp) |

---

## 一句话总结

> 提出首个大规模双手机器人灵巧抓取数据集UltraDexGrasp-20M（2000万帧），结合优化-based抓取合成与规划-based演示生成，训练出的策略在真实世界实现81.2%成功率。

---

## 核心贡献

1. **UltraDexGrasp-20M数据集**: 首个大规模多策略双手机器人灵巧抓取数据集，包含2000万帧、1000物体、多种抓取策略
2. **数据生成流程**: 整合优化-based抓取合成与规划-based演示生成，产生物理合理且多样的轨迹
3. **通用抓取策略**: 简单有效的策略架构，点云输入 + 单向注意力 + 控制命令预测，支持多种抓取策略
4. **零样本Sim-to-Real**: 仅用合成数据训练，实现84.0%仿真成功率和81.2%真实世界成功率

---

## 问题背景

### 要解决的问题

双手机器人的通用灵巧抓取面临三大挑战：
1. **数据稀缺**: 现有数据生成方法主要针对单手或平行夹爪，双手机器人数据严重不足
2. **多策略需求**: 不同尺寸、形状、重量的物体需要不同抓取策略（双持、整手、二指捏取、三指三脚架）
3. **物理约束**: 抓取需要满足力闭合、运动学可达、无碰撞等物理约束

### 现有方法的局限

| 方法类型 | 局限性 |
|----------|--------|
| RL训练专家 | 训练后确定性策略，缺乏多样性，难以泛化到新物体 |
| 优化-based合成 | 开环方法，忽略手臂运动学，动态场景能力差 |
| 学习-based合成 | 需要初始数据，主要针对单手设置 |
| 现有数据集 | 主要关注平行夹爪或单手灵巧抓取，缺乏双手机器人多策略数据 |

### 本文的动机

人类能够根据物体特性自主选择抓取策略，而机器人抓取能力远远落后。作者认为：
- 大规模高质量数据是突破瓶颈的关键
- 整合优化与规划方法可生成物理合理的演示
- 简单的策略架构配合大规模数据可实现强泛化

---

## 方法详解

### 数据生成流程

UltraDexGrasp 采用分层数据生成架构：

- **输入**: 物体mesh + 机器人URDF
- **场景初始化**: 导入物体和机器人模型，随机化相机位姿和关节阻抗
- **抓取合成**: 优化-based方法生成可行抓取
- **演示生成**: 运动规划生成协调的双臂轨迹
- **输出**: 闭环运动轨迹数据

### 抓取合成

#### 抓取位姿定义

双手机器人的抓取位姿定义为：

$$
g = \left\{ \left( \boldsymbol{t}_h, \boldsymbol{R}_h, \boldsymbol{q}_h \right) \mid h = 0, 1 \right\}
$$

**符号说明**:
- $h \in \{0, 1\}$: 手的索引
- $\boldsymbol{t}_h \in \mathbb{R}^3$: 第$h$只手的平移
- $\boldsymbol{R}_h \in SO(3)$: 第$h$只手的旋转
- $\boldsymbol{q}_h \in \mathbb{R}^n$: 手的关节位置

#### 摩擦锥约束

使用硬手指接触模型，摩擦锥定义为：

$$
\mathcal{F} = \left\{ \boldsymbol{f} \;\middle|\; \|\boldsymbol{f}_{\mathrm{tan}}\| \leq \mu \|\boldsymbol{f}_{\mathrm{n}}\|, \;\; f_z \geq 0 \right\}
$$

**符号说明**:
- $\mu$: 静摩擦系数
- $\boldsymbol{f}_{\mathrm{n}} = [0, 0, f_z]$: 法向力分量
- $\boldsymbol{f}_{\mathrm{tan}} = [f_x, f_y, 0]$: 切向力分量

#### Grasp Wrench Space

每个接触点施加的wrench为：

$$
\boldsymbol{w}_i = \begin{bmatrix} \boldsymbol{f}_i \\ \alpha(\boldsymbol{d}_i \times \boldsymbol{f}_i) \end{bmatrix}
$$

Grasp map定义为：

$$
\boldsymbol{G}_i = \begin{bmatrix} \mathbf{I}_{3 \times 3} \\ (\boldsymbol{p}_i - \boldsymbol{m})_{\times} \end{bmatrix} \boldsymbol{O}_i
$$

Grasp wrench space为：

$$
\mathcal{W} := \left\{ \boldsymbol{w} \;\middle|\; \boldsymbol{w} = \sum_{i=1}^{k} \boldsymbol{G}_i \boldsymbol{f}_i,~\boldsymbol{f}_i \in \mathcal{F}_i, \;\; i = 1, \ldots, k \right\}
$$

**符号说明**:
- $\boldsymbol{f}_i \in \mathbb{R}^3$: 第$i$个接触点的力
- $\boldsymbol{d}_i$: 接触点相对于物体质心的位置
- $\boldsymbol{p}_i$: 接触位置
- $\boldsymbol{m}$: 物体质心
- $\boldsymbol{O}_i$: 从局部接触坐标系到物体坐标系的旋转矩阵

#### 抓取合成优化问题

$$
\min_{\boldsymbol{g}, \{\boldsymbol{f}_{\boldsymbol{c}}\}} \quad \kappa_w \sum_{j=1}^{J} \Big\| \lambda \boldsymbol{w}_j - \sum_{\boldsymbol{c} \in \mathcal{C}} \boldsymbol{G}_{\boldsymbol{c}}(\boldsymbol{g}) \boldsymbol{f}_{\boldsymbol{c}} \Big\|_2^2
$$

$$
\quad + \kappa_{\mathrm{con}} \sum_{\boldsymbol{c} \in \mathcal{C}} \psi\big(d_M(\boldsymbol{p}_{\boldsymbol{c}})\big)
$$

$$
\quad + \kappa_{\mathrm{coll}} \Phi_M(\boldsymbol{g}) + \kappa_{\mathrm{hh}} \Phi_{\mathrm{hh}}(\boldsymbol{g})
$$

**约束条件**:

$$
\boldsymbol{q}_{h, \min} \leq \boldsymbol{q}_h \leq \boldsymbol{q}_{h, \max}, \quad h \in \{0, 1\}
$$

$$
\boldsymbol{f}_{\boldsymbol{c}} \in \mathcal{F}, \quad \boldsymbol{c} \in \mathcal{C}
$$

$$
\boldsymbol{R}_h \in SO(3), \quad h \in \{0, 1\}
$$

**符号说明**:
- $\kappa_w, \kappa_{\mathrm{con}}, \kappa_{\mathrm{coll}}, \kappa_{\mathrm{hh}}$: 权重系数
- $\psi(d)$: 距离能量，衡量接触点与物体表面的距离
- $\Phi_M(\boldsymbol{g})$: 手-物碰撞能量
- $\Phi_{\mathrm{hh}}(\boldsymbol{g})$: 手-手穿透能量
- $d_M(\cdot)$: 到物体表面的有符号距离

**求解方法**: 采用双层优化框架，底层QP优化接触力，顶层梯度下降更新手部位姿。使用[[cuRobo]]和GPU-based QP求解器加速。

#### SE(3)距离

用于选择最优抓取：

$$
d(\boldsymbol{T}_1, \boldsymbol{T}_2) = \|\boldsymbol{t}_1 - \boldsymbol{t}_2\|_2 + \lambda \cdot d_{\mathrm{rot}}(\boldsymbol{R}_1, \boldsymbol{R}_2)
$$

旋转距离：

$$
d_{\mathrm{rot}}(\boldsymbol{R}_1, \boldsymbol{R}_2) = \arccos\left(\frac{\mathrm{trace}(\boldsymbol{R}_1^{-1} \boldsymbol{R}_2) - 1}{2}\right)
$$

### 多策略支持

论文支持四种抓取策略，通过选择不同的手指接触点实现：

| 策略 | 适用物体 | 描述 |
|------|----------|------|
| **Bimanual Hold** | 大型/重物体 | 双手协调抓取 |
| **Whole-hand Grasp** | 中等物体 | 单手五指全握 |
| **Two-finger Pinch** | 小物体 | 拇指+食指捏取 |
| **Three-finger Tripod** | 小物体 | 三指三脚架式抓取 |

### 抓取策略架构

#### 模型架构

UltraDexGrasp 策略采用 **Encoder-Transformer** 架构：

- **输入**: 点云 $\mathcal{P} \in \mathbb{R}^{N \times 3}$
- **点云编码器**: 将点云编码为特征序列
- **单向注意力**: 聚合场景特征（[[Transformer]] decoder）
- **输出**: 控制命令 $\boldsymbol{a} \in \mathbb{R}^{d}$
- **预测方式**: 有界高斯分布预测

#### 核心设计

1. **单向注意力机制**: 所有token只能attend to之前的token，防止信息泄漏
2. **有界高斯分布预测**: 预测动作的分布参数，提高多样性和鲁棒性
3. **多策略支持**: 策略自动适应物体特性选择合适的抓取方式

---

## 关键图表

### Figure 1: 系统概览

![Figure 1](https://yangsizhe.github.io/ultradexgrasp/static/images/teaser.jpg)

**说明**: UltraDexGrasp 实现双手机器人的通用灵巧抓取，支持多种抓取策略，能够处理不同形状、尺寸和重量的物体。

### Figure 2: 数据生成流程

![Figure 2](https://arxiv.org/html/2603.05312v1/x1.png)

**说明**: 完整的数据生成pipeline：场景初始化 -> 抓取合成 -> 抓取过滤与排序 -> 运动规划 -> 演示生成。

### Figure 3: 抓取策略接触点

![Figure 3](https://arxiv.org/html/2603.05312v1/x2.png)

**说明**: 不同抓取策略选择不同的手指接触点，用于计算优化问题中的能量项。从左到右依次为：双持、整手、二指、三指抓取。

### Figure 4: 策略架构

![Figure 4](https://arxiv.org/html/2603.05312v1/x3.png)

**说明**: 策略网络架构：点云输入 -> Point Encoder -> 单向注意力聚合 -> 动作预测。支持多策略并提高泛化能力。

### Figure 5: 数据规模效应

![Figure 5](https://arxiv.org/html/2603.05312v1/x4.png)

**说明**: 随着训练数据量增加，策略性能持续提升。当数据量超过1M帧时，学习到的策略显著超越数据生成时的成功率(68.5%)。

### Figure 6: 真实世界实验设置

![Figure 6](https://arxiv.org/html/2603.05312v1/x5.png)

**说明**: 真实世界实验平台：两个UR5e机器人 + 两个XHand (12-DoF灵巧手) + 两个Azure Kinect DK相机。

### Table I: 仿真基准结果

| Benchmark | Object Size | DP3 | DexGraspNet | **Ours** |
|-----------|-------------|-----|-------------|----------|
| Seen Objects | Small | 41.7 | 45.6 | **78.8** |
| Seen Objects | Medium | 54.3 | 72.0 | **84.3** |
| Seen Objects | Large | 48.5 | - | **90.4** |
| Unseen Objects | Small | 37.4 | 45.6 | **76.9** |
| Unseen Objects | Medium | 50.1 | - | **82.6** |
| Unseen Objects | Large | 44.9 | - | **89.4** |
| **Average** | - | 46.7 | 58.8 | **84.0** |

**关键发现**:
- 平均成功率84.0%，比最佳baseline高25.2个百分点（约43%相对提升）
- 对未见物体成功率83.4%，展现强泛化能力
- 比DP3高37.3个百分点
- DexGraspNet无法处理大型物体（仅支持单手抓取）

### Table II: 消融实验

| 配置 | Success Rate (%) |
|------|------------------|
| w/o Dist. Pred. | 73.5 |
| w/o Uni. Attn. | 72.8 |
| **Full Model** | **84.0** |

**关键发现**:
- 有界高斯分布预测带来10.5%提升
- 单向注意力机制带来11.2%提升
- 两个设计选择都对性能至关重要

### Table III: 真实世界基准结果

| Benchmark | DP3 | DexGraspNet | **Ours** |
|-----------|-----|-------------|----------|
| Seen Objects | 35.6 | 48.2 | **80.3** |
| Unseen Objects | 32.8 | 45.7 | **81.9** |
| **Average** | 34.2 | 47.0 | **81.2** |

**关键发现**:
- 真实世界平均成功率81.2%，验证零样本sim-to-real迁移能力
- 对未见物体成功率81.9%，略高于已见物体，说明策略学到通用抓取能力

---

## 实验

### 数据集

| 数据集 | 规模 | 特点 | 用途 |
|--------|------|------|------|
| UltraDexGrasp-20M | 20M帧/1000物体 | 多策略、双手机器人 | 训练 |
| DexGraspNet Objects | 1000物体 | 多样化几何形状 | 物体来源 |
| Simulation Test | 600物体 | 可见+不可见 | 测试 |
| Real-world Test | 多种物体 | 5g-1000g，多种尺寸 | 测试 |

### 实现细节

- **仿真器**: Isaac Gym
- **机器人**: 双UR5e + 双XHand (12-DoF灵巧手)
- **相机**: Azure Kinect DK (眼在手上)
- **优化器**: Adam
- **训练数据**: 20M帧
- **训练时间**: 未披露
- **推理**: 实时

### 测试物体范围

- **重量**: 5g - 1000g
- **尺寸**: 最长边 < 0.03m 到 最短边 > 0.5m
- **形状**: 多样化几何形状

---

## 批判性思考

### 优点

1. **数据规模突破**: 首个大规模双手机器人多策略抓取数据集，填补领域空白
2. **方法组合创新**: 优化-based + 规划-based的组合兼顾物理合理性和运动自然性
3. **强泛化能力**: 零样本sim-to-real迁移，81.2%真实世界成功率
4. **多策略支持**: 自动适应不同物体选择合适抓取策略，更接近人类抓取行为
5. **开源贡献**: 数据生成流程开源，便于社区复现和扩展

### 局限性

1. **静态场景**: 当前方法针对桌面静态物体，未涉及动态或移动目标
2. **无语义理解**: 纯几何驱动，不考虑物体功能部位或后续任务
3. **单一姿态**: 物体初始姿态固定，未测试随机姿态下的抓取
4. **仿真依赖**: 完全依赖合成数据，可能存在sim-to-real gap的隐患

### 潜在改进方向

1. **结合VLA**: 引入视觉-语言-动作模型，实现语义驱动的功能性抓取
2. **动态抓取**: 扩展到移动目标或动态场景
3. **姿态鲁棒性**: 增加随机初始姿态的训练和测试
4. ** cluttered场景**: 扩展到杂乱场景中的抓取

### 可复现性评估

- [x] 代码开源
- [ ] 预训练模型
- [x] 训练细节完整
- [x] 数据集可获取（通过pipeline生成）

---

## 关联笔记

### 基于

- [[DexGraspNet]]: 物体资产来源，单手灵巧抓取数据集
- [[BODex]]: 双层优化抓取合成方法参考
- [[cuRobo]]: 运动规划和碰撞检测工具

### 对比

- [[DP3]]: Diffusion Policy baseline，同数据集训练对比
- [[DexGraspNet]] (方法): 优化-based baseline，仅支持单手

### 方法相关

- [[Force Closure]]: 抓取稳定性评判标准
- [[Grasp Synthesis]]: 抓取姿态生成技术
- [[Sim-to-Real]]: 仿真到真实迁移
- [[Dexterous Hand]]: 灵巧手硬件
- [[Motion Planning]]: 运动规划

### 硬件/数据相关

- [[XHand]]: 12-DoF灵巧手
- [[UR5e]]: UR机械臂
- [[Azure Kinect DK]]: RGB-D相机

---

## 速查卡片

> [!summary] UltraDexGrasp
> - **核心**: 首个大规模双手机器人多策略灵巧抓取数据集+策略
> - **方法**: 优化-based抓取合成 + 规划-based演示生成
> - **数据**: 20M帧，1000物体，4种抓取策略
> - **结果**: 仿真84.0%，真实世界81.2%成功率
> - **代码**: [github.com/InternRobotics/UltraDexGrasp](https://github.com/InternRobotics/UltraDexGrasp)

---

*笔记创建时间: 2026-04-03*