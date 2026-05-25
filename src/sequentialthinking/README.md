# Sequential Thinking MCP Server

提供结构化思考流程工具的 MCP 服务器实现，用于动态、反思式地解决问题。

## 特性

- 将复杂问题拆解为可管理的步骤
- 随着理解加深修订与精炼思路
- 分支探索替代推理路径
- 动态调整思考总步数
- 生成并验证解决方案假设

## 工具

### sequential_thinking

为问题求解与分析提供详细的逐步思考过程。

**输入：**
- `thought`（string）：当前思考步骤
- `nextThoughtNeeded`（boolean）：是否还需要下一步思考
- `thoughtNumber`（integer）：当前步骤编号
- `totalThoughts`（integer）：预估所需总步数
- `isRevision`（boolean，可选）：是否修订先前思考
- `revisesThought`（integer，可选）：正在重新考虑的步骤编号
- `branchFromThought`（integer，可选）：分支起点步骤编号
- `branchId`（string，可选）：分支标识
- `needsMoreThoughts`（boolean，可选）：是否需要更多思考步骤

## 用法

Sequential Thinking 工具适用于：

- 将复杂问题拆解为步骤
- 可修订的规划与设计
- 可能需要纠偏的分析
- 初始范围不清晰的任务
- 需在多步中保持上下文的任务
- 需要过滤无关信息的场景

实践中，除非客户端暴露原始工具调用，否则通常不会手动调用 `sequential_thinking`。应将服务器连接到支持 MCP 的主机，让模型逐步思考问题；主机可在工作时多次调用该工具。

### 使用时的表现

通常适合使用该工具的示例提示：

- `规划从 PostgreSQL 14 迁移到 16，列出风险；若停机超过 5 分钟则修订计划。`
- `逐步推理：为何该部署仅在生产环境失败。`
- `比较文件同步引擎的三种架构；若某假设错误则分支讨论。`

### 如何判断正在工作

若主机或 inspector 显示工具活动，应看到对 `sequential_thinking` 的重复调用，字段包括：

- `thought`
- `thoughtNumber`
- `totalThoughts`
- `nextThoughtNeeded`

当推理改变方向时，还可能看到 `isRevision`、`revisesThought`、`branchFromThought` 或 `branchId` 等修订/分支字段。

### 快速手动验证

在 MCP 主机中安装服务器后：

1. 重启或重新加载主机以重新连接服务器。
2. 确认主机的 MCP 工具列表或 inspector 中出现 `sequential_thinking`。
3. 让主机以逐步方式解决一个非平凡问题。
4. 确认主机多次调用该工具，而非一次性返回答案。

## 配置

### 在 Claude Desktop 中使用

添加到 `claude_desktop_config.json`：

#### npx

```json
{
  "mcpServers": {
    "sequential-thinking": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-sequential-thinking"
      ]
    }
  }
}
```

在 Windows 上，用 `cmd /c` 启动 `npx`：

```json
{
  "mcpServers": {
    "sequential-thinking": {
      "command": "cmd",
      "args": [
        "/c",
        "npx",
        "-y",
        "@modelcontextprotocol/server-sequential-thinking"
      ]
    }
  }
}
```

#### docker

```json
{
  "mcpServers": {
    "sequentialthinking": {
      "command": "docker",
      "args": [
        "run",
        "--rm",
        "-i",
        "mcp/sequentialthinking"
      ]
    }
  }
}
```

要禁用思考信息日志，将环境变量 `DISABLE_THOUGHT_LOGGING` 设为 `true`。

### 在 VS Code 中使用

快速安装可点击下方安装按钮之一……

[![Install with NPX in VS Code](https://img.shields.io/badge/VS_Code-NPM-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=sequentialthinking&config=%7B%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22%40modelcontextprotocol%2Fserver-sequential-thinking%22%5D%7D) [![Install with NPX in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-NPM-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=sequentialthinking&config=%7B%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22%40modelcontextprotocol%2Fserver-sequential-thinking%22%5D%7D&quality=insiders)

[![Install with Docker in VS Code](https://img.shields.io/badge/VS_Code-Docker-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=sequentialthinking&config=%7B%22command%22%3A%22docker%22%2C%22args%22%3A%5B%22run%22%2C%22--rm%22%2C%22-i%22%2C%22mcp%2Fsequentialthinking%22%5D%7D) [![Install with Docker in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-Docker-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=sequentialthinking&config=%7B%22command%22%3A%22docker%22%2C%22args%22%3A%5B%22run%22%2C%22--rm%22%2C%22-i%22%2C%22mcp%2Fsequentialthinking%22%5D%7D&quality=insiders)

手动安装可用以下方式配置 MCP 服务器：

**方式 1：用户级配置（推荐）**
将配置添加到用户级 MCP 配置文件。打开命令面板（`Ctrl + Shift + P`），运行 `MCP: Open User Configuration`，在 `mcp.json` 中添加服务器配置。

**方式 2：工作区配置**
也可将配置写入工作区的 `.vscode/mcp.json`，便于与他人共享。

> VS Code 中 MCP 配置的更多说明见 [官方 VS Code MCP 文档](https://code.visualstudio.com/docs/copilot/customization/mcp-servers)。

NPX 安装：

```json
{
  "servers": {
    "sequential-thinking": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-sequential-thinking"
      ]
    }
  }
}
```

Windows 上使用：

```json
{
  "servers": {
    "sequential-thinking": {
      "command": "cmd",
      "args": [
        "/c",
        "npx",
        "-y",
        "@modelcontextprotocol/server-sequential-thinking"
      ]
    }
  }
}
```

Docker 安装：

```json
{
  "servers": {
    "sequential-thinking": {
      "command": "docker",
      "args": [
        "run",
        "--rm",
        "-i",
        "mcp/sequentialthinking"
      ]
    }
  }
}
```

### 在 Codex CLI 中使用

运行：

#### npx

```bash
codex mcp add sequential-thinking npx -y @modelcontextprotocol/server-sequential-thinking
```

## 构建

Docker：

```bash
docker build -t mcp/sequentialthinking -f src/sequentialthinking/Dockerfile .
```

## 许可证

本 MCP 服务器采用 MIT 许可证。你可自由使用、修改和分发，须遵守 MIT 许可条款。详见项目仓库中的 LICENSE 文件。
