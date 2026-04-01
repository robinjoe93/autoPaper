# autoPaper - 论文追踪系统

基于 Claude Code 的自动化论文追踪与笔记管理系统。每日自动抓取 arXiv 和 HuggingFace 最新论文，按关键词筛选，生成推荐报告和笔记。

## 目录结构

```
ObsidianVault/
├── DailyPapers/          # 每日论文推荐报告
│   ├── .history.json     # 已推荐论文历史（去重用）
│   └── YYYY-MM-DD-论文推荐.md
├── 论文笔记/             # 论文详细笔记
│   ├── _概念/            # 概念库（方法、技术术语）
│   └── *.md              # 单篇论文笔记
└── .gitignore
```

## 使用方法

### 日常使用

在 Claude Code 中使用以下命令：

| 命令 | 说明 |
|------|------|
| `今日论文推荐` | 一键完成：抓取 → 打分 → 推荐 → 笔记 |
| `过去3天论文推荐` | 抓取最近 3 天的论文 |
| `过去一周论文推荐` | 抓取最近 7 天的论文 |
| `论文抓取` | 仅执行抓取和打分 |
| `论文点评` | 生成推荐报告 |
| `批量笔记` | 为推荐论文生成详细笔记 |
| `读论文 <PDF路径>` | 单篇论文深度分析 |

### 单篇论文分析

提供 PDF 文件路径即可：
```
读论文 /path/to/paper.pdf
```

支持 Zotero 集成，可直接读取 Zotero 库中的论文。

## 追踪关键词

当前配置追踪以下研究方向：

**主要关键词：**
- synthetic data / data generation / data synthesis
- training data / instruction tuning / instruction data
- data quality / data curation / data evaluation
- llm training / vllm / llm inference / model serving
- domain adaptation / domain-specific llm / vertical llm
- diffusion model

**排除领域：**
- 医学影像、天气预测、金融交易
- 音频/语音生成、代码生成 Agent
- RAG 框架、OCR、文档解析

## 配置文件

配置位于 `~/.claude/skills/_shared/user-config.json`：

```json
{
  "paths": {
    "obsidian_vault": "~/ObsidianVault",
    "paper_notes_folder": "论文笔记",
    "daily_papers_folder": "DailyPapers"
  },
  "daily_papers": {
    "keywords": [...],           // 追踪关键词
    "negative_keywords": [...],  // 排除关键词
    "domain_boost_keywords": [...], // 领域加分词
    "arxiv_categories": ["cs.RO", "cs.CV", "cs.AI", "cs.LG"],
    "min_score": 2,
    "top_n": 30
  },
  "automation": {
    "auto_refresh_indexes": true,
    "git_commit": true,
    "git_push": true
  }
}
```

## 自动化功能

- **自动 Git 提交**：每次生成推荐或笔记后自动提交
- **自动推送**：同步到 GitHub 远程仓库
- **自动刷新索引**：更新 Obsidian 目录页（MOC）

## 技术栈

- [Claude Code](https://claude.ai/code) - AI 助手 CLI
- [arXiv API](https://arxiv.org) - 论文来源
- [HuggingFace Daily Papers](https://huggingface.co/papers) - 论文来源
- [Obsidian](https://obsidian.md) - 笔记管理（可选）

## License

MIT