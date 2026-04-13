---
type: concept
aliases: [Omni-Comp Benchmark, 3D Completion Benchmark]
---

# Omni-Comp

## 定义
LaS-Comp 论文中提出的综合 3D shape completion benchmark，结合真实扫描和合成数据，涵盖多种 partial patterns。

## 组成

1. **真实扫描**: 来自 Redwood、KITTI、ScanNet
2. **合成数据**: Objaverse shapes

## Partial Patterns

- **Random Crops**: 随机裁剪，模拟传感器遮挡
- **Single-View Scans**: 单视角扫描，模拟深度相机
- **Semantic Parts Missing**: 语义部件缺失，模拟结构损坏

## 核心要点
1. **多样化**: 涵盖多种 realistic partial patterns
2. **真实性**: 包含真实场景扫描数据
3. **挑战性**: 更 thorough 和 realistic 的评估

## 代表工作
- [[LaS-Comp]]: 提出 Omni-Comp

## 相关概念
- [[Shape Completion]]
- [[Redwood Dataset]]
- [[KITTI]]
- [[ScanNet]]