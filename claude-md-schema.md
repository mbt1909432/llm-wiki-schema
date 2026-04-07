# elon-wiki: LLM Knowledge Base

基于 Karpathy 式「知识编译」模式的个人知识库。三层架构：Raw（只读原始资料）→ Wiki（LLM 维护的 Markdown 知识库）→ Schema（本文件，约束 LLM 行为）。人类负责筛选、提问、决策；LLM 负责总结、交叉引用、维护。

---

## 目录结构

```
raw/           # 原始资料层 — 不可变、只读。永远不要修改、删除或移动其中的文件。
wiki/          # 知识库层 — LLM 的领地。所有 wiki 页面由 LLM 创建和维护。
CLAUDE.md      # 规范层 — 定义命名、格式、工作流、质量标准。
```

**铁律：绝对不要修改 `raw/` 中的任何文件。**

---

## 命名规则

- wiki 页面文件名：**小写英文、连字符分隔**，最长 50 字符
  - 正确：`harness-engineering.md`、`ai-agent-failure-modes.md`
  - 错误：`Harness Engineering.md`、`AI Agent失败模式.md`
- 页面内容语言：**中文**（与原始资料保持一致）
- frontmatter 键名：**英文**
- 使用最具辨识度的英文名词短语，不用疑问句或动词短语

---

## 页面格式

每个 wiki 页面**必须**包含以下 frontmatter：

```yaml
---
title: "页面标题（中文）"
type: topic | concept | entity
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources:
  - "raw/文件名.md"
tags:
  - tag1
  - tag2
related:
  - "[[其他wiki页面]]"
---
```

### type 说明

| type | 用途 | 必含章节 |
|---|---|---|
| `topic` | 一个完整主题/领域的综述 | Summary → Key Points → Details → Cross-References → Sources |
| `concept` | 一个可复用的概念/方法论 | Definition → Explanation → Evidence/Examples → Connections → Sources |
| `entity` | 具体的人、组织、工具 | Overview → Key Facts → Relevance → Sources |

### callout 用法

- `> [!abstract]` — 页面顶部的执行摘要
- `> [!warning]` — 矛盾标记（新信息与已有内容冲突时，**绝不静默覆盖**）
- `> [!tip]` — 实践建议或关键洞察
- `> [!info]` — 补充说明

---

## 交叉引用

- 使用 Obsidian `[[wikilink]]` 语法链接 wiki 页面
- 当一个概念/实体出现在 **2 个及以上页面**时，应为它创建独立页面或确保交叉链接存在
- 在 frontmatter 的 `related` 字段和正文末尾的 Cross-References / Connections 章节都要列出关联页面
- 引用 raw 来源时使用格式：`> Sources: [[raw/文件名]]` 或在正文中注明出处

---

## 工作流：Ingest（摄入）

当人类提供新资料或要求摄入已有 raw 文件时执行：

1. **读取原始资料** — 完整阅读 `raw/` 中的源文件
2. **提取关键信息** — 概念、实体、论点、数据点
3. **写入摘要页面** — 在 `wiki/` 创建新页面，含完整 frontmatter、摘要、正文、出处
4. **更新已有页面** — 将新信息融入相关的已有概念/实体页
5. **标记矛盾** — 新数据与已有内容冲突时，用 `> [!warning] Contradiction` 标记，说明双方立场
6. **更新索引** — 在 `wiki/index.md` 对应分类下添加条目
7. **追加日志** — 在 `wiki/log.md` 添加时间戳条目

---

## 工作流：Query（查询）

当人类向知识库提问时执行：

1. 阅读 `wiki/index.md` 了解知识库全貌
2. 阅读与问题相关的 wiki 页面
3. 综合多个页面的信息，给出有依据的回答
4. 如果综合产生了有价值的新知识，回写为新的 wiki 页面（然后走 Ingest 的 6-7 步更新索引和日志）

---

## 工作流：Lint（检查）

定期或应要求执行知识库健康检查：

- **孤立页面**：没有任何页面链接到它的 wiki 页面
- **断链**：wikilink 指向不存在的页面
- **未入索引**：有 wiki 页面但未出现在 `index.md` 中
- **过期数据**：长期未更新的页面（标记 `updated` 日期过旧）
- **未解决矛盾**：仍带有 `[!warning] Contradiction` 标记的页面
- **缺标签/缺 frontmatter**：不符合页面格式的页面

修复后必须更新 `log.md`。

---

## 质量标准

1. **可溯源**：每个事实断言必须能追溯到 `raw/` 中的源文件
2. **不编造**：不确定的信息标注为"待验证"，不凭空填充
3. **简洁有力**：用表格、列表、callout 代替大段散文
4. **矛盾透明**：发现矛盾时必须标记，不静默覆盖任何一方
5. **日志完整**：每次 wiki 变更（创建/更新/删除页面）都必须在 `log.md` 中记录

---

## index.md 维护规则

- 分三类列出：`## Topics`、`## Concepts`、`## Entities`
- 每个条目格式：`- [[页面名]] -- 一句话摘要`
- 页面头部注明最后更新日期和总页面数
- 新页面创建后立即添加，不要遗漏

## log.md 维护规则

- 每次操作用 `## YYYY-MM-DD -- 简述` 作为标题
- 条目格式：`- **[tag]** 具体操作，影响哪些页面`
- tag 类型：`[ingest]`、`[query]`、`[lint]`、`[init]`
- 按时间倒序排列（最新操作在最上面）
