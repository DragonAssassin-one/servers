# Everything MCP 服务器
**[架构](docs/architecture.md)
| [项目结构](docs/structure.md)
| [启动流程](docs/startup.md)
| [服务器功能](docs/features.md)
| [扩展点](docs/extension.md)
| [工作原理](docs/how-it-works.md)**


本 MCP 服务器尝试演练 MCP 协议的全部功能。它并非面向实用场景，而是供 MCP 客户端构建者使用的测试服务器。它实现了提示词、工具、资源、采样等，以展示 MCP 能力。

## 工具、资源、提示词及其他功能

已注册的 MCP 基元及本服务器演示的其他协议功能的完整列表，请参阅[服务器功能](docs/features.md)文档。

## 在 Claude Desktop 中使用（使用 [stdio 传输](https://modelcontextprotocol.io/specification/2025-03-26/basic/transports#stdio)）

添加到 `claude_desktop_config.json`：

```json
{
  "mcpServers": {
    "everything": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-everything"
      ]
    }
  }
}
```

在 Windows 上，使用 `cmd /c` 启动 `npx`：

```json
{
  "mcpServers": {
    "everything": {
      "command": "cmd",
      "args": [
        "/c",
        "npx",
        "-y",
        "@modelcontextprotocol/server-everything"
      ]
    }
  }
}
```

## 在 VS Code 中使用

快速安装可使用下方一键安装按钮之一：

[![Install with NPX in VS Code](https://img.shields.io/badge/VS_Code-NPM-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=everything&config=%7B%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22%40modelcontextprotocol%2Fserver-everything%22%5D%7D) [![Install with NPX in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-NPM-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=everything&config=%7B%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22%40modelcontextprotocol%2Fserver-everything%22%5D%7D&quality=insiders)

[![Install with Docker in VS Code](https://img.shields.io/badge/VS_Code-Docker-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=everything&config=%7B%22command%22%3A%22docker%22%2C%22args%22%3A%5B%22run%22%2C%22-i%22%2C%22--rm%22%2C%22mcp%2Feverything%22%5D%7D) [![Install with Docker in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-Docker-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=everything&config=%7B%22command%22%3A%22docker%22%2C%22args%22%3A%5B%22run%22%2C%22-i%22%2C%22--rm%22%2C%22mcp%2Feverything%22%5D%7D&quality=insiders)

手动安装时，可使用以下方法之一配置 MCP 服务器：

**方法 1：用户配置（推荐）**
将配置添加到用户级 MCP 配置文件。打开命令面板（`Ctrl + Shift + P`）并运行 `MCP: Open User Configuration`。这将打开用户 `mcp.json` 文件，您可在其中添加服务器配置。

**方法 2：工作区配置**
或者，可将配置添加到工作区中的 `.vscode/mcp.json` 文件。这样可与他人共享配置。

> 有关 VS Code 中 MCP 配置的更多详情，请参阅[官方 VS Code MCP 文档](https://code.visualstudio.com/docs/copilot/customization/mcp-servers)。

#### NPX

```json
{
  "servers": {
    "everything": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-everything"]
    }
  }
}
```

在 Windows 上，使用：

```json
{
  "servers": {
    "everything": {
      "command": "cmd",
      "args": ["/c", "npx", "-y", "@modelcontextprotocol/server-everything"]
    }
  }
}
```

## 从源码运行，使用 [HTTP+SSE 传输](https://modelcontextprotocol.io/specification/2024-11-05/basic/transports#http-with-sse)（自 [2025-03-26](https://modelcontextprotocol.io/specification/2025-03-26/basic/transports) 起已弃用）

```shell
cd src/everything
npm install
npm run start:sse
```

## 从源码运行，使用 [Streamable HTTP 传输](https://modelcontextprotocol.io/specification/2025-03-26/basic/transports#streamable-http)

```shell
cd src/everything
npm install
npm run start:streamableHttp
```

## 作为已安装包运行
### 安装
```shell
npm install -g @modelcontextprotocol/server-everything@latest
````

### 运行默认（stdio）服务器
```shell
npx @modelcontextprotocol/server-everything
```

### 或显式指定 stdio
```shell
npx @modelcontextprotocol/server-everything stdio
```

### 运行 SSE 服务器
```shell
npx @modelcontextprotocol/server-everything sse
```

### 运行 streamable HTTP 服务器
```shell
npx @modelcontextprotocol/server-everything streamableHttp
```

