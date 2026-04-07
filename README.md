# LLM Wiki Schema

基于 Andrej Karpathy「知识编译」模式构建的 LLM Wiki 规范文件，配合 Obsidian + Claude Code 使用。

## 思路来源

Andrej Karpathy 分享了他的个人实践：用 LLM 把原始资料"编译"成结构化的 Markdown Wiki。他个人的 Wiki 已有约 100 篇文章、40 万字，全部由 LLM 维护，极少手动编辑。

核心洞察：传统 RAG 每次从零检索，而 LLM Wiki 持续构建交叉链接网络，知识像复利一样越积越强。

## 这是什么

一个 `CLAUDE.md` 配置文件，定义了三层架构和三种工作流，让 Claude Code 能自动维护你的个人知识库。

## 三层架构

```
your-vault/
├── CLAUDE.md      ← 规范层：定义 LLM 怎么写、怎么组织
├── raw/           ← 原始资料层：只读不可变，放文章/论文/笔记
└── wiki/          ← 知识库层：LLM 自动维护，人类不用手动管
    ├── index.md   ← 内容目录
    └── log.md     ← 操作日志
```

## 三种工作流

| 工作流 | 做什么 | 触发方式 |
|---|---|---|
| **Ingest** | 读 raw → 写摘要 → 建交叉链接 → 更新索引 → 记日志 | 往 raw/ 扔资料后告诉 Claude "ingest" |
| **Query** | 读索引 → 读相关页面 → 综合回答 | 直接向知识库提问 |
| **Lint** | 检查孤立页/断链/未入索引/矛盾/缺 frontmatter | 说 "lint" 或设置定时检查 |

## 快速开始

1. 把 `claude-md-schema.md` 复制到你的 Obsidian Vault 根目录，重命名为 `CLAUDE.md`
2. 创建 `raw/` 和 `wiki/` 目录，在 `wiki/` 下创建 `index.md` 和 `log.md`
3. 往 `raw/` 里放资料（文章、论文、视频笔记等）
4. 用 Claude Code 打开 Vault 目录，说 "ingest" 开始构建 Wiki

## 规范要点

- **文件名**：小写英文 + 连字符（如 `harness-engineering.md`）
- **页面内容**：中文
- **每个页面必有 frontmatter**：title / type / created / updated / sources / tags / related
- **交叉引用**：用 Obsidian `[[wikilink]]` 语法
- **矛盾处理**：发现矛盾不静默覆盖，用 `> [!warning]` 标记
- **可溯源**：每个事实断言都能追溯到 raw/ 中的源文件

## License

MIT
