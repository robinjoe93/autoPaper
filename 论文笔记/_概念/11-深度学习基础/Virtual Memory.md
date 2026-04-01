---
type: concept
aliases: [Virtual Memory, 虚拟内存, Paging]
---

# Virtual Memory

## 定义

虚拟内存是操作系统的内存管理技术，将逻辑地址空间映射到物理内存，通过分页机制实现非连续存储和内存共享。

## 核心要点

1. **分页**: 内存划分为固定大小的页
2. **页表**: 逻辑页到物理页的映射
3. **按需分配**: 页面按需加载，避免预分配浪费
4. **共享支持**: 多进程可共享同一物理页

## 代表工作

- [[PagedAttention]]: 将虚拟内存思想迁移到 KV cache 管理

## 相关概念

- [[KV Cache]]
- [[PagedAttention]]
- [[Memory Management]]