# Obsiwiki

语言：[English](README.md) | 简体中文

`Obsiwiki` 是一个与 agent 无关的 workflow / skill 包，用来把 Obsidian vault 维护成一个 LLM Wiki。

它为不同 agent 提供一套共享的操作模型，用于：

- ingest 链接、文章、论文、转录稿、会议记录和原始笔记
- 将原始材料保存在 `raw/`
- 将稳定知识编译到 `wiki/`
- 从对话中捕获可复用结论
- 基于已有 wiki 页面回答问题
- lint vault，检查孤立页面、缺失来源、重复主题、过期页面和附件放置问题

这个包包含可复用的 `SKILL.md`、参考规则和 starter vault 骨架。它不包含个人笔记或私有知识内容。

## 目录

- [Agent 使用速查](#agent-使用速查)
- [为 Codex 安装](#为-codex-安装)
- [为 Claude Code 安装](#为-claude-code-安装)
- [为 Hermes 安装](#为-hermes-安装)
- [为 OpenClaw 安装](#为-openclaw-安装)
- [添加 Starter Vault 骨架](#添加-starter-vault-骨架)
- [Vault 目录结构](#vault-目录结构)
- [Schema 优先级](#schema-优先级)
- [核心工作流](#核心工作流)
- [页面类型](#页面类型)
- [最小 Frontmatter](#最小-frontmatter)
- [给其它 Agent 的说明](#给其它-agent-的说明)
- [隐私](#隐私)
- [更新 Skill](#更新-skill)

## 致谢

Obsiwiki 受到 Andrej Karpathy 的 [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) idea file 启发。感谢 Karpathy 清晰描述了使用 LLM agent 递增维护持久、互联 wiki 的模式。

## 仓库结构

```text
.
├── SKILL.md
├── adapters/
│   ├── claude-code.md
│   ├── hermes.md
│   └── openclaw.md
├── references/
│   ├── lint-checklist.md
│   ├── page-types.md
│   └── schema.md
└── starter-vault/
    ├── System/
    │   ├── Agents/
    │   └── Schema/
    ├── assets/
    ├── raw/
    └── wiki/
```

## Agent 使用速查

| Agent | 如何加载 Obsiwiki | 典型用法 |
| --- | --- | --- |
| Codex | 让 Codex 读取本 README，把必要文件安装到它保存 skills 的位置，然后重启 Codex。 | `Use Obsiwiki to ingest this source: https://example.com/article` |
| Claude Code | 让 Claude Code 读取本 README，把必要文件安装到它保存项目说明或 skills 的位置，并使用 `adapters/claude-code.md`。 | `Use Obsiwiki to ingest this source: https://example.com/article` |
| Hermes | 让 Hermes 读取本 README，把必要文件安装到它保存 skills/plugins/context 的位置，并使用 `adapters/hermes.md`。 | `Use Obsiwiki to ingest this source: https://example.com/article` |
| OpenClaw | 让 OpenClaw 读取本 README，把必要文件安装到它保存 commands/plugins/skills 的位置，并暴露 `/obsiwiki` 命令。 | `/obsiwiki ingest https://example.com/article` |

常用工作流动词：

- `ingest`：将来源保存到 `raw/`，在 `wiki/sources/` 创建 digest，并更新 maps/index/log。
- `capture`：从对话中提取可复用结论，并更新最合适的目标页面。
- `query`：从 `wiki/index.md`、maps 和正式 wiki 页面中回答问题。
- `lint`：检查孤立页面、缺失来源、重复主题、过期页面和附件放置问题。

规范工作流提示：

```text
Use Obsiwiki to ingest this source: https://example.com/article
Use Obsiwiki to capture reusable conclusions from this conversation.
Use Obsiwiki to answer this from the vault: <question>
Use Obsiwiki to lint this vault.
```

## 为 Codex 安装

让 Codex 从本仓库安装 Obsiwiki。它应该读取本 README，判断当前环境中 Codex 保存 skills 的位置，把必要文件复制或 clone 到那里，并告诉你如何重新加载 skill。

Agent-friendly 安装提示：

```text
Read https://github.com/TengShao/Obsiwiki and install Obsiwiki for Codex.

Decide where Codex should keep reusable skills in this environment, copy or clone the necessary Obsiwiki files there, and make sure the installed skill is named obsiwiki.

After installation, tell me which files you installed, where you installed them, and how to restart or reload Codex so the skill is picked up.
```

使用方式与其它 agent 相同。在 Codex 中，如果希望显式调用 skill，也可以使用 `$obsiwiki`。

```text
Use Obsiwiki to ingest this source: https://example.com/article
Use Obsiwiki to capture reusable conclusions from this conversation.
Use Obsiwiki to answer this from the vault: What is the relationship between A2A and MCP?
Use Obsiwiki to lint this vault.
```

## 为 Claude Code 安装

让 Claude Code 从本仓库安装 Obsiwiki。它应该读取本 README，选择 Claude 可访问的 instruction、skill 或 project context 位置，复制必要源文件，并遵循下面的 schema 优先级规则。

Agent-friendly 安装提示：

```text
Read https://github.com/TengShao/Obsiwiki and install Obsiwiki for Claude Code.

Decide where Claude Code should keep reusable project instructions or skills in this environment, copy the necessary Obsiwiki files there, and set up the target vault so Claude Code reads adapters/claude-code.md and follows Obsiwiki schema precedence.

After installation, tell me which files you copied and where.
```

Claude Code 应使用这个 adapter：

```text
adapters/claude-code.md
```

推荐的 vault-local 设置：复制 starter vault 文件，然后在 vault 根目录创建 `CLAUDE.md`，让 Claude Code 指向 vault-local schema：

```text
System/Schema/vault-schema.md
System/Schema/workflows.md
System/Schema/page-contracts.md
System/Agents/claude-code.md
```

典型 Claude Code 提示：

```text
Use Obsiwiki to ingest this source: https://example.com/article
Use Obsiwiki to capture reusable conclusions from this conversation.
Use Obsiwiki to answer this from the vault: ...
Use Obsiwiki to lint this vault.
```

## 为 Hermes 安装

让 Hermes 从本仓库安装 Obsiwiki。它应该读取本 README，判断 Hermes 在当前环境中保存 reusable skills、plugins 或 long-lived context 的位置，并复制必要源文件。

Agent-friendly 安装提示：

```text
Read https://github.com/TengShao/Obsiwiki and install Obsiwiki for Hermes.

Decide where Hermes should keep reusable skills, plugins, or context in this environment, copy the necessary Obsiwiki files there, and configure Hermes to load adapters/hermes.md together with the Obsiwiki references and schema.

After installation, tell me which files you copied and where.
```

Hermes 应使用这个 adapter：

```text
adapters/hermes.md
```

给 Hermes 加载这些文件作为上下文：

- `SKILL.md`
- `references/schema.md`
- `references/page-types.md`
- `references/lint-checklist.md`
- `starter-vault/System/Schema/`

典型 Hermes 提示：

```text
Use Obsiwiki to ingest this source: https://example.com/article
Use Obsiwiki to capture reusable conclusions from this conversation.
Use Obsiwiki to answer this from the vault: ...
Use Obsiwiki to lint this vault.
```

## 为 OpenClaw 安装

让 OpenClaw 从本仓库安装 Obsiwiki。它应该读取本 README，判断 OpenClaw 在当前环境中保存 slash commands、plugins 或 skills 的位置，复制必要源文件，并暴露 Obsiwiki command workflow。

Agent-friendly 安装提示：

```text
Read https://github.com/TengShao/Obsiwiki and install Obsiwiki for OpenClaw.

Decide where OpenClaw should keep reusable slash commands, plugins, or skills in this environment, copy the necessary Obsiwiki files there, and expose the /obsiwiki command workflow using adapters/openclaw.md.

After installation, tell me which files you copied and where.
```

OpenClaw 应使用这个 adapter：

```text
adapters/openclaw.md
```

推荐 slash-command 形态：

```text
/obsiwiki ingest <url-or-source>
/obsiwiki capture
/obsiwiki capture <target-page>
/obsiwiki capture synthesis
/obsiwiki query <question>
/obsiwiki lint
/obsiwiki help
```

每个命令都应该进入同一套由本 skill 和 active schema 定义的 `ingest / capture / query / lint` 工作流。

## 添加 Starter Vault 骨架

如果你正在从一个新的或结构较轻的 Obsidian vault 开始，可以把 starter 文件复制到 vault 根目录：

```bash
cp -R starter-vault/* /path/to/your/obsidian-vault/
```

starter vault 会创建这样的知识架构：

```text
raw/        原始来源材料
assets/     二进制文件、截图、PDF 和可复用视觉资产
wiki/       可供 agent 查询和综合的稳定知识页面
System/     schema、workflow、page contracts 和 agent adapters
```

如果你的 vault 已经有笔记，不要先批量移动它们。让 agent 使用 lint workflow 逐步提出增量调整建议。

## Vault 目录结构

starter vault 使用四个主要层次：

```text
raw/
├── articles/        网页文章、摘录、教程和实践指南
├── papers/          论文、PDF、报告和研究材料
├── transcripts/     转录稿和长篇对话
└── meeting-notes/   会议、访谈和现场笔记

assets/
├── raw/             属于某个 raw source 的文件，按 source slug 分组
├── wiki/            长期复用的知识资产
├── projects/        项目相关资产
└── shared/          跨主题复用的资产

wiki/
├── concepts/        稳定概念、协议、框架和方法
├── entities/        工具、产品、公司、人物和命名系统
├── sources/         raw source 的一对一 digest
├── syntheses/       跨来源或跨对话的综合分析
├── maps/            主题地图 / MOC，用来保持页面连接
├── index.md         顶层导航入口
└── log.md           追加式维护日志

System/
├── Schema/          可选的 vault-local 规则和 workflow source of truth
└── Agents/          Codex、Claude Code、Hermes、OpenClaw 或其它 agent 的 adapter notes
```

目录意图：

- `raw/` 尽量保留来源材料的原始形态。
- `assets/` 避免二进制文件散落在笔记目录里。
- `wiki/` 是 agent 应该查询和更新的稳定知识层。
- `wiki/maps/` 是主要的 anti-orphan 机制。
- `System/Schema/` 存在时，是 vault-local source of truth；agent-specific 文件应该适配它，而不是 fork 它。

## Schema 优先级

默认使用已安装的 Obsiwiki `references/` 和 `starter-vault/System/Schema/` 作为 schema。

如果目标 vault 包含 `System/Schema/`，则将该 vault-local schema 视为 source of truth。已安装 schema 只用于对照、更新建议和迁移。不要静默覆盖 vault-local schema 自定义内容。

Agent adapters 只负责把同一套 schema 翻译给不同 agent 使用。它们不能 fork 目录语义、页面契约或 workflow 行为。

## 核心工作流

### Ingest

当你给 agent 一个 URL、文章、PDF、截图包、转录稿、会议记录或 raw note 时使用。

预期行为：

1. 将 raw source 保存或摘要到 `raw/`。
2. 将附件保存到 `assets/raw/<source-slug>/`。
3. 在 `wiki/sources/` 下创建或更新一对一 digest。
4. 在有用时更新已有的 `wiki/concepts/`、`wiki/entities/` 或 `wiki/syntheses/`。
5. 将新增或更新的知识挂到至少一个 `wiki/maps/` 页面。
6. 在确认修改后更新 `wiki/index.md`，并追加 `wiki/log.md`。

示例：

```text
Use Obsiwiki to ingest this source: https://example.com/article
```

### Capture

当一次对话产生了可复用结论、决策、定义、权衡或 workflow 时使用。

预期行为：

1. 只提取可复用知识。
2. 默认不保存完整聊天记录。
3. 建议最合适的目标页面。
4. 确认后更新目标页面。
5. 仅当答案跨多个主题或来源时，才创建 `wiki/syntheses/` 页面。

示例：

```text
Use Obsiwiki to capture reusable conclusions from this conversation.
```

### Query

当你希望 agent 从 vault 中回答问题时使用。

预期行为：

1. 从 `wiki/index.md` 开始。
2. 跟随相关 `wiki/maps/`。
3. 只读取必要的 `concepts`、`entities`、`sources` 和 `syntheses`。
4. 只有当答案具有广泛复用价值时，才建议新增或更新 `synthesis`。

示例：

```text
Use Obsiwiki to answer this from the vault: What does my knowledge base say about the relationship between prompt engineering and UXD?
```

### Lint

当你想做 vault 健康检查时使用。

预期行为：

- 标记未被 maps 或 index 覆盖的 wiki 页面
- 标记没有正式链接的 source 页面
- 标记重复或近似重复主题
- 标记缺失 `last_updated`、缺失 sources 或缺失 `Related Links`
- 标记 raw attachment 放置问题
- 标记仍依赖 raw-only assets、应提升到 `assets/wiki/` 的 wiki 页面

示例：

```text
Use Obsiwiki to lint this vault.
```

## 页面类型

正式 wiki 页面使用这些 `type` 值：

- `concept`：稳定概念、协议、框架或方法
- `entity`：工具、产品、公司、人物或命名系统
- `source`：raw source 的一对一 digest
- `synthesis`：跨来源或跨讨论的综合页面
- `map`：主题地图 / MOC

## 最小 Frontmatter

正式 wiki 页面应包含：

```yaml
---
title:
type:
tags: []
sources: []
last_updated: YYYY-MM-DD
aliases: []
---
```

Tags 应描述主题，而不是文件夹或 workflow 状态。优先使用 lowercase English `kebab-case` tags。

## 给其它 Agent 的说明

这个 skill 与 agent 无关。对于不能直接加载 Codex skills 的 agent，请让它们读取：

- `SKILL.md`
- 相关 adapter：`adapters/claude-code.md`、`adapters/hermes.md` 或 `adapters/openclaw.md`
- `references/schema.md`
- `references/page-types.md`
- `references/lint-checklist.md`
- `starter-vault/System/Schema/`

然后使用同样的规范提示：

```text
Use Obsiwiki to ingest this source: https://example.com/article
Use Obsiwiki to capture reusable conclusions from this conversation.
Use Obsiwiki to answer this from the vault: <question>
Use Obsiwiki to lint this vault.
```

## 隐私

本仓库有意只包含可复用规则和空的 starter structure。不要发布你的个人 `raw/`、`wiki/`、`Projects/`、`Work/`、`Opinions/` 或 `Journal/` 内容，除非你已经审查过其中的私密或敏感信息。

## 更新 Skill

让目标 agent 读取本 README，找到已有 Obsiwiki 安装位置，并从最新仓库版本刷新源文件。

Agent-friendly 更新提示：

```text
Read https://github.com/TengShao/Obsiwiki and update the existing Obsiwiki installation for this agent.

Find where Obsiwiki was installed in this environment, refresh the copied or cloned source files from the latest repository version, preserve any vault-local System/Schema/ customizations, and tell me what changed.
```

确保 agent 重新加载刷新后的文件：

- `SKILL.md`
- `references/schema.md`
- `references/page-types.md`
- `references/lint-checklist.md`
- `starter-vault/System/Schema/` 或 vault-local `System/Schema/`
- 位于 `adapters/` 的相关 adapter

如果目标 vault 已经有自定义的 `System/Schema/`，请将其与最新 schema 协调，而不是静默替换用户编辑过的文件。
