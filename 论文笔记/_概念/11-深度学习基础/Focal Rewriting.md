---
type: concept
aliases: [焦点改写, 问题条件化改写]
---

# Focal Rewriting

## 定义

焦点改写是一种合成文档生成技术，通过显式条件化于特定问题来生成文档，提升合成文档的主题和风格多样性。

## 核心要点

1. **条件化生成**: 在改写指令中添加 `"Focus on the question {{query}}"`
2. **多样性提升**: 同时增加语义多样性（Vendi Score）和词法多样性（unique 4-gram）
3. **更陡扩展曲线**: 相比普通 AR，提供更陡峭的 log-linear 扩展趋势
4. **简单实现**: 仅需修改 prompt，无需额外架构

## 代表工作

- [[SMT]]: Han et al. (2026) 提出并与 SMT 结合使用

## 实现方式

Prompt 模板（简化）：
```
Apply this strategy to the document, 
with focus on the question: {query}
```

## 效果对比

| 方法 | 语义多样性 | 词法多样性 | 扩展效率 |
|------|-----------|-----------|---------|
| 普通 AR | 较低 | 较低 | 平缓 |
| Focal Rewriting | 更高 | 更高 | 更陡 |

## 相关概念

- [[Synthetic Mixed Training]]
- [[Active Reading]]
- [[Vendi Score]]