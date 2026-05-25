# CLAUDE.md

本文件为在 Claude Code 中处理本仓库代码时提供指引。

## 项目概览

官方 MCP 参考服务器实现。这是一个 npm workspaces 单体仓库，在 `src/` 下包含 7 个服务器（4 个 TypeScript、3 个 Python）。每个服务器都是独立包，发布到 npm 或 PyPI。

## 单体仓库结构

```
src/
  everything/          TS  @modelcontextprotocol/server-everything    （参考服务器，涵盖全部 MCP 功能）
  filesystem/          TS  @modelcontextprotocol/server-filesystem    （文件操作，带 Roots 访问控制）
  memory/              TS  @modelcontextprotocol/server-memory        （知识图谱持久化）
  sequentialthinking/  TS  @modelcontextprotocol/server-sequential-thinking  （分步推理）
  fetch/               Py  mcp-server-fetch                           （网页内容抓取）
  git/                 Py  mcp-server-git                             （Git 仓库操作）
  time/                Py  mcp-server-time                            （时区查询与转换）
```

## 构建与测试命令

### TypeScript 服务器

```bash
# 单个服务器
cd src/<server> && npm ci && npm run build && npm test

# 在根目录构建所有 TS 服务器
npm install && npm run build
```

- 构建：`tsc`（目标 ES2022，模块 Node16，严格模式）
- 测试：**vitest**，配合 `@vitest/coverage-v8`（新测试必须使用该框架）
- Node 版本：**22**

### Python 服务器

```bash
cd src/<server> && uv sync --frozen --all-extras --dev

# 运行测试（若存在 tests/ 或 test/ 目录）
uv run pytest

# 类型检查
uv run pyright

# 代码检查
uv run ruff check .
```

- 构建系统：**hatchling**（`uv build`）
- 包管理器：**uv**（不用 pip）
- Python 版本：**>= 3.10**（各服务器有 `.python-version`）
- 类型检查：**pyright**（CI 强制）
- Lint：**ruff**

## 代码风格

### TypeScript

- ES 模块，import 路径使用 `.js` 扩展名
- 函数与变量使用严格 TypeScript 类型
- 工具入参校验使用 Zod schema
- 2 空格缩进，多行对象使用尾随逗号
- 变量/函数 camelCase，类型/类 PascalCase，常量 UPPER_CASE
- 文件名及注册的工具/prompts/resources 使用 kebab-case
- 工具名以动词开头（如 `get-file-info`，而非 `file-info`）
- import 分组：先外部，后内部

### Python

- 通过 pyright 强制类型注解
- 使用 async/await（fetch 服务器尤其如此，配合 pytest-asyncio）
- 遵循各服务器现有模块布局

## 贡献指南

**欢迎：** Bug 修复、可用性改进、能展示 MCP 协议能力（Resources、Prompts、Roots，而不仅是 Tools）的增强。

**选择性接受：** 超出服务器核心职责或高度主观的新功能。

**不接受：** 新的服务器实现（请使用 [MCP Server Registry](https://github.com/modelcontextprotocol/registry)）、README 中的服务器列表变更。

## CI/CD 流水线

TypeScript 与 Python 工作流均使用 **动态包检测**（find + jq 矩阵策略）：

1. `detect-packages` — 在 `src/` 下查找所有 `package.json` / `pyproject.toml`
2. `test` — 按包运行测试
3. `build` — 按包编译与类型检查
4. `publish` — 仅在发布事件时（TS 走 npm，Python 走 PyPI 可信发布）

## MCP 协议参考

仓库通过 `.mcp.json` 配置了 MCP 文档服务器，指向 `https://modelcontextprotocol.io/mcp`。Schema 详见 `https://github.com/modelcontextprotocol/modelcontextprotocol/tree/main/schema`（含 JSON 与 TypeScript 版本化 schema）。

## 关键模式

- 各服务器通过 `registerTools(server)`、`registerResources(server)`、`registerPrompts(server)` 注册能力
- 工具注解：按 MCP 规范设置 `readOnlyHint`、`idempotentHint`、`destructiveHint`
- 传输：stdio（默认）、SSE（已弃用）、Streamable HTTP
- 所有 PR 按 [PR 模板](.github/pull_request_template.md) 检查 — 需阅读 MCP 文档、遵循安全最佳实践，并用 LLM 客户端测试变更
