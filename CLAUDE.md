# LLM Wiki — CLAUDE.md Schema

基于 Andrej Karpathy 的「知识编译」模式，为 Obsidian + Claude Code 构建的 LLM Wiki 规范文件。

## 这是什么

这是一个 `CLAUDE.md` 配置文件，定义了如何用 LLM 增量构建个人知识库（Wiki）的完整规范。把它放到你的 Obsidian Vault 根目录，Claude Code 就能自动按照这套规则维护你的 Wiki。

Karpathy 的核心思路：把原始资料当作"源码"，用 LLM 把它"编译"成结构化的 Markdown Wiki。人类负责筛选和提问，LLM 负责总结、交叉引用和维护。知识会像复利一样越积越强。

## 快速开始

1. 在 Obsidian Vault 根目录创建三层结构：
   ```
   your-vault/
   ├── CLAUDE.md      ← 这个文件
   ├── raw/           ← 放原始资料（文章、论文、笔记）
   └── wiki/          ← LLM 自动维护，不用手动管
       ├── index.md
       └── log.md
   ```
2. 把 `CLAUDE.md` 放到 Vault 根目录
3. 往 `raw/` 里扔资料
4. 用 Claude Code 执行 `ingest` 命令

## 规范内容涵盖

- 目录结构定义（Raw / Wiki / Schema 三层）
- 页面命名规则（英文文件名 + 中文内容）
- Frontmatter 格式（title/type/sources/tags/related）
- 三种工作流：Ingest（摄入）、Query（查询）、Lint（检查）
- 交叉引用规则（wikilink 语法）
- 质量标准（可溯源、不编造、矛盾透明）

## 参考

- 思想来自 Andrej Karpathy 的「LLM as Knowledge Compiler」实践
- Anthropic 的 Harness Engineering 设计理念
- Karpathy 原始分享：他个人的 LLM Wiki 约有 100 篇文章、40 万字，全部由 LLM 维护

## License

MIT
