# Wiki 操作日志

> append-only。每次操作在顶部追加一条记录，格式：`## [YYYY-MM-DD HH:MM] <操作> | <描述>`

## [2026-09-04 11:20] restructure | raw 重构为「收件箱 + 知识归档」两段式管线
- 契约升级：AGENTS.md v2.0（收件箱只读 / 个人灵感=原 Daily 授权编辑 / 知识档案=收件箱转化层）；realclaudian systemPrompt 同步
- 目录迁移：`raw/知识文件归档/Daily/*` → `raw/知识归档/个人灵感/`（18 个 daily）
- 目录迁移：`raw/文档/LLM-Wiki知识库搭建指南.md`、刘宝红丛书、任职资格、AI产品、全局工作规则 → `raw/知识归档/知识档案/`
- 目录迁移：知识文件归档中未转化 pptx/xmind（7 件）→ `raw/收件箱/`（待转 md 入库）
- 新增配置：`.obsidian/daily-notes.json` → 每日笔记指向 `raw/知识归档/个人灵感`

## [2026-08-20 16:35] init | 搭建 LLM-Wiki 基础结构
- 创建目录：`raw/文档/`、`raw/网页剪藏/`、`wiki/`
- 归类：根目录 `LLM-Wiki知识库搭建指南.md` → `raw/文档/`（git mv 保留历史）
- 新建页面：`wiki/工作方法/LLM-Wiki知识库架构.md`（unreviewed）
- 创建索引与日志：`wiki/index.md`、`wiki/log.md`
- 缺失概念（待建）：`项目工作规则`、`AI知识库工作台-抖音视频`、`RAG工程化方法论串讲`
