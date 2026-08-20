---
date: 2026-08-20
tags: [工作方法, LLM-Wiki, 知识库]
review_status: unreviewed
sources:
  - raw/文档/LLM-Wiki知识库搭建指南.md
  - AGENTS.md
---

# LLM-Wiki 知识库架构

> **状态**：unreviewed
> **最后更新**：2026-08-20
> **来源**：[[raw/文档/LLM-Wiki知识库搭建指南.md]]、[[AGENTS.md]]

## 核心要点

- 本库采用 [Karpathy LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 架构：**不可变的原始资料**（`raw/`）+ **AI 可维护的提炼知识**（`wiki/`）+ **操作契约**（`AGENTS.md`）+ **目录与审计**（`index.md` / `log.md`）。
- 定位：让 LLM 当**知识库管理员**，而不是聊天机器人。
- 四大核心原则：**来源至上、诚实标记、级联维护、可逆可审计**。
- 审核状态机：`unreviewed` / `reviewed`（**仅人工可设**）/ `disputed` / `outdated`。
- 🔴 红线：LLM 绝对不得把 `review_status` 设为 `reviewed`。

## 目录与权限

| 目录 | 读取 | 写入 | 修改 | 删除 | 说明 |
|------|:----:|:----:|:----:|:----:|------|
| `raw/` | ✅ | ❌ | ❌ | ❌ | 原始资料不可变，作为证据留存 |
| `wiki/` | ✅ | ✅ | ✅ | ✅ | LLM 全权维护，index/log 每次操作必更新 |
| `raw/文档/Daily/` | ✅ | ❌ | ❌ | ❌ | 日记是人工私人空间 |
| `AGENTS.md` | ✅ | ⚠️ | ⚠️ | ❌ | 修改前需人工确认 |

## 核心机制

### 1. 审核状态机（信任的根基）

| 状态 | 含义 | 谁能设置 |
|------|------|----------|
| `unreviewed` | LLM 生成，未经人工确认 | LLM |
| `reviewed` | 人工已确认 | **仅人工** |
| `disputed` | 发现矛盾，待解决 | LLM 或人工 |
| `outdated` | 已被新资料取代 | LLM（需在 log 说明） |

> ⚠️ 这是"可信"的底线：AI 生成的内容永远不冒充人工审核过。

### 2. 四大流程

- **Ingest（摄入）**：读 `raw/` 新文件 → 识别主题/关联 → 创建/更新 `wiki/` 页面 → **级联更新**相关页面 → 更新 index + 追加 log → 报告。
- **Query（查询）**：先读 `index.md` 定位 → 综合多来源 → 区分三层信息（📌来源事实 / 🔍综合判断 / 💡LLM 推断）→ 先结论后依据、逐条标来源。
- **Write-back（回写）**：有价值问答且无页面覆盖时 → 建 `unreviewed` 页面 → 保留 Q&A 结构 → 标来源 → 更新 index/log。
- **Lint（体检）**：矛盾检查、过时检查、孤儿页面检查、缺失概念检查、来源追溯检查。

### 3. 页面模板要点

YAML frontmatter 含 `date` / `tags` / `review_status` / `sources`；正文含状态块 + 核心要点 + 详细内容 + 相关页面 + 待补充。已在 wiki 页面原文基础上补充更新块时用 `> 📝 更新（unreviewed）`。

## 当前库结构对照（2026-08-20）

> 💡 **LLM 推断**：以下为本次初始化时的结构快照，基于契约核对，未经人工审核。

```
obsidian workspace/
├── AGENTS.md                    # 契约（reviewed，人工维护）
├── raw/                         # 原始资料（不可变）
│   ├── 文档/
│   │   └── LLM-Wiki知识库搭建指南.md   # 搭建复现手册
│   └── 网页剪藏/                # 空，待投放
└── wiki/                        # LLM 维护的知识库
    ├── index.md                 # 知识目录
    ├── log.md                   # 操作日志
    └── 工作方法/
        └── LLM-Wiki知识库架构.md       # 本页
```

## 相关页面

- [[AGENTS.md]] — 正式契约（权威版本）
- [[raw/文档/LLM-Wiki知识库搭建指南.md]] — 搭建指南（含一键初始化 Prompt）
- [[wiki/工作方法/项目工作规则]] — 待建（缺失概念）
- [[wiki/AI与工具/AI知识库工作台-抖音视频]] — 待建（缺失概念）
- [[wiki/AI与工具/RAG工程化方法论串讲]] — 待建（缺失概念）

## 待补充

- [ ] 人工审核本页（`unreviewed` → `reviewed`）
- [ ] 资料入库后按领域新建页面，补全 index 统计
- [ ] 创建缺失概念页：项目工作规则、AI知识库工作台、RAG工程化方法论（需先有来源资料）
