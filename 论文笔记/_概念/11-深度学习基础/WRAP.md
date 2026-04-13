---
type: concept
aliases: [改写, Rephrasing]
---

# WRAP

## 定义

WRAP 是一种简单的合成文档生成方法，直接使用语言模型改写原始文档。

## 核心要点

1. **最简单方法**: 仅需一个改写 prompt
2. **风格多样性**: 改写产生风格变化
3. **主题有限**: 不显式条件化，主题覆盖可能重复
4. **扩展效率最低**: 在生成器扩展实验中效率最差

## 代表工作

- Maini et al. (2024): 提出 rephrasing 用于预训练
- [[SMT]]: 作为文档生成 baseline 对比

## Prompt 模板

```
Rewrite the document to help 
understand better.
<document>
{document}
</document>
```

## 扩展特性

- 8B 生成器: 效率较低
- 70B 生成器: 效率提升最小（相比其他方法）

## 相关概念

- [[Active Reading]]
- [[EntiGraph]]
- [[Synthetic Mixed Training]]