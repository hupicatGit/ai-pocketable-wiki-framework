# Wiki Framework — 项目知识库框架

一个专为 AI Agent 设计的结构化项目知识库框架。将本目录下的 `wiki/` 复制到任何项目根即可使用。

## 核心设计

传统项目文档散落各处，AI 上下文装不下。本框架采用 **分层摘要金字塔**：

```
sources（原始资料摘要）
    ↓ 提炼
concepts（概念）+ entities（实体）
    ↓ 聚合
overview（综合概述）
    ↓ 导航
index（总目录）
    ↓ 追记
log（变更日志）
```

每一层是上一层的摘要和提炼。AI 从 overview 获知全局，按需 drill-down。

## 快速开始

### 1. 复制到项目

```bash
cp -r ai-pocketable-wiki-framework/wiki/ <your-project>/wiki/
```

### 2. 让 AI 知道它的存在（两层保障）

**第一层 — 长期记忆注册（跨 session 生效，最关键）：**

Agent 读到 `AI-OPERATION-SPEC.md` 后，会自动将 wiki 路径写入自己的长期记忆。这样下次 session 无需重新发现。

**第二层 — 显式注册（推荐，双保险）：**

在 Agent 的项目配置文件中加入对该文件的引用：

| Agent | 注册方式 |
|-------|---------|
| Claude Code | 在 `.claude/CLAUDE.md` 中加入：`处理任务前先读 wiki/AI-OPERATION-SPEC.md` |
| Codex | 在 `.codex.md` 中加入同上规则 |
| Cursor | 在 `.cursorrules` 中加入同上规则 |
| Hermes | 由 AI-OPERATION-SPEC.md 第 0 步自动处理，无需手动操作 |

### 3. 初始化项目信息

填写 `wiki/overview-综合概述.md` 的「项目核心」——这是 AI 理解项目的最短路径。

### 4. 随用随长

每次任务 AI 发现新信息，按 AI-OPERATION-SPEC.md 的流程自动写入 wiki，并同步更新长期记忆中的 wiki 摘要。

## 目录结构

```
ai-pocketable-wiki-framework/
├── README.md               ← 本文件（给人看）
└── wiki/                   ← 知识库（复制到项目中使用）
    ├── AI-OPERATION-SPEC.md  ← AI 操作规范（Agent 先读这个）
    ├── index-总目录.md
    ├── log-变更日志.md
    ├── overview-综合概述.md
    ├── sources/            ← 原始资料摘要
    ├── concepts/           ← 概念条目
    ├── entities/           ← 实体条目
    └── comparisons/        ← 横向对比
```

## Agent 发现机制（三层保障）

1. **长期记忆（最强）**：Agent 读到 `AI-OPERATION-SPEC.md` 后，将 wiki 路径写入自身长期记忆。每次 session 自动注入，无需重新发现
2. **自动发现**：Agent 扫描 `wiki/` 目录时，`AI-OPERATION-SPEC.md` 作为第一个文件被读到，Agent 从中获知全部规则
3. **显式注册（推荐）**：在 Agent 的项目配置文件中加入对该文件的引用

三层保障确保无论 Agent 以什么方式、什么时间接触项目，都能快速找到知识库。

## 页面分类说明

| 分类 | 放什么 | 示例 |
|------|--------|------|
| sources | 原始资料摘要 | 代码调研记录、会议纪要、文档摘要 |
| concepts | 可复用的知识单元 | 配置流程、ID 规则、字段类型 |
| entities | 具体的事物 | 项目概览、团队成员、外部服务 |
| comparisons | 对比分析 | 方案 A vs 方案 B |

## 兼容性

- **Obsidian**：完全兼容，`[[链接]]` 和 frontmatter 直接可用
- **GitHub / GitLab**：Markdown 正常渲染
- **任何 AI Agent**：能读文件的 Agent 都能使用

## 许可

无版权限制，自由用于任何项目。
