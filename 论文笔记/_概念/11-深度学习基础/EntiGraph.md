---
type: concept
aliases: [实体图, Entity Graph]
---

# EntiGraph

## 定义

EntiGraph 是一种合成文档生成方法，通过提取文档核心实体并生成描述实体关系的文档来增加数据多样性。

## 核心要点

1. **两阶段流程**: (1) 提取实体 (2) 生成实体关系文档
2. **主题多样性**: 通过不同实体组合覆盖更多主题
3. **风格相似**: 生成的文档风格较为统一
4. **实体条件化**: 显式条件化于实体，确保内容覆盖

## 代表工作

- Yang et al. (2025b): 提出 EntiGraph 用于 continued pretraining
- [[SMT]]: 作为文档生成 baseline 对比

## Prompt 模板

Stage 1 - 实体提取：
```
Extract entities from the article.
Format: {"summary": "...", "entities": [...]}
```

Stage 2 - 实体关系文档：
```
Discuss entity1 and entity2 interaction...
```

## 与其他方法对比

| 方法 | 主题多样性 | 风格多样性 |
|------|-----------|-----------|
| WRAP | 低 | 中 |
| EntiGraph | 高 | 低 |
| Active Reading | 中 | 高 |
| Focal Rewriting | 高 | 高 |

## 相关概念

- [[Active Reading]]
- [[WRAP]]
- [[Synthetic Mixed Training]]