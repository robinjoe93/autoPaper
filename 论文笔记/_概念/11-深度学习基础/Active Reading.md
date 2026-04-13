---
type: concept
aliases: [AR, 主动阅读]
---

# Active Reading

## 定义

主动阅读（Active Reading）是一种合成文档生成方法，通过两阶段流程生成多样化文档：先生成学习策略，再应用策略改写文档。

## 核心要点

1. **两阶段**: (1) LM 生成学习策略 (2) LM 应用策略改写
2. **策略多样性**: 通过不同学习策略增加风格多样性
3. **生成器扩展受益**: 最受益于更强生成器的文档方法
4. **SMT 选择**: 在 SMT 中作为文档生成方法

## 代表工作

- Lin et al. (2025a): 提出 Active Reading
- [[SMT]]: 选择 AR 作为文档生成组件

## Prompt 模板

Stage 1 - 策略生成：
```
What are strategies to help learn 
this document? Use ## prefix.
```

Stage 2 - 应用策略：
```
Apply this strategy to the document...
```

## 相关概念

- [[Synthetic Mixed Training]]
- [[EntiGraph]]
- [[WRAP]]
- [[Focal Rewriting]]