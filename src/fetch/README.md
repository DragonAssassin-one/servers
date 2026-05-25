# Fetch MCP 服务器

<!-- mcp-name: io.github.modelcontextprotocol/server-fetch -->

提供网页内容抓取能力的 Model Context Protocol 服务器。本服务器使 LLM 能够从网页检索并处理内容，将 HTML 转换为 markdown 以便阅读。

> [!CAUTION]
> 本服务器可访问本地/内网 IP 地址，可能带来安全风险。使用此 MCP 服务器时请谨慎，确保不会暴露任何敏感数据。

`fetch` 工具会截断响应，但可通过 `start_index` 参数指定从何处开始提取内容。这样模型可以分块阅读网页，直到找到所需信息。

### 可用工具

- `fetch` - 从互联网获取 URL 并将其内容提取为 markdown。
    - `url` (string, required)：要获取的 URL
    - `max_length` (integer, optional)：返回的最大字符数（默认：5000）
    - `start_index` (integer, optional)：从此字符索引开始提取内容（默认：0）
    - `raw` (boolean, optional)：获取未经 markdown 转换的原始内容（默认：false）

### 提示词

- **fetch**
  - 获取 URL 并将其内容提取为 markdown
  - 参数：
    - `url` (string, required)：要获取的 URL

## 安装

可选：安装 node.js，这将使 fetch 服务器使用更稳健的 HTML 简化器。

### 使用 uv（推荐）

使用 [`uv`](https://docs.astral.sh/uv/) 时无需单独安装。我们将通过 [`uvx`](https://docs.astral.sh/uv/guides/tools/) 直接运行 *mcp-server-fetch*。

### 使用 PIP

也可通过 pip 安装 `mcp-server-fetch`：

```
pip install mcp-server-fetch
```

安装后，可作为脚本运行：

```
python -m mcp_server_fetch
```

## 配置

### 为 Claude.app 配置

添加到 Claude 设置：

<details>
<summary>使用 uvx</summary>

```json
{
  "mcpServers": {
    "fetch": {
      "command": "uvx",
      "args": ["mcp-server-fetch"]
    }
  }
}
```
</details>

<details>
<summary>使用 docker</summary>

```json
{
  "mcpServers": {
    "fetch": {
      "command": "docker",
      "args": ["run", "-i", "--rm", "mcp/fetch"]
    }
  }
}
```
</details>

<details>
<summary>使用 pip 安装</summary>

```json
{
  "mcpServers": {
    "fetch": {
      "command": "python",
      "args": ["-m", "mcp_server_fetch"]
    }
  }
}
```
</details>

### 为 VS Code 配置

快速安装可使用下方一键安装按钮之一：

[![Install with UV in VS Code](https://img.shields.io/badge/VS_Code-UV-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=fetch&config=%7B%22command%22%3A%22uvx%22%2C%22args%22%3A%5B%22mcp-server-fetch%22%5D%7D) [![Install with UV in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-UV-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=fetch&config=%7B%22command%22%3A%22uvx%22%2C%22args%22%3A%5B%22mcp-server-fetch%22%5D%7D&quality=insiders)

[![Install with Docker in VS Code](https://img.shields.io/badge/VS_Code-Docker-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=fetch&config=%7B%22command%22%3A%22docker%22%2C%22args%22%3A%5B%22run%22%2C%22-i%22%2C%22--rm%22%2C%22mcp%2Ffetch%22%5D%7D) [![Install with Docker in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-Docker-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=fetch&config=%7B%22command%22%3A%22docker%22%2C%22args%22%3A%5B%22run%22%2C%22-i%22%2C%22--rm%22%2C%22mcp%2Ffetch%22%5D%7D&quality=insiders)

手动安装时，将以下 JSON 块添加到 VS Code 的用户设置（JSON）文件。可按 `Ctrl + Shift + P` 并输入 `Preferences: Open User Settings (JSON)`。

也可添加到工作区的 `.vscode/mcp.json` 文件，以便与他人共享配置。

> 注意：使用 `mcp.json` 文件时需要 `mcp` 键。

<details>
<summary>使用 uvx</summary>

```json
{
  "mcp": {
    "servers": {
      "fetch": {
        "command": "uvx",
        "args": ["mcp-server-fetch"]
      }
    }
  }
}
```
</details>

<details>
<summary>使用 Docker</summary>

```json
{
  "mcp": {
    "servers": {
      "fetch": {
        "command": "docker",
        "args": ["run", "-i", "--rm", "mcp/fetch"]
      }
    }
  }
}
```
</details>

### 自定义 - robots.txt

默认情况下，若请求来自模型（通过工具），服务器会遵守网站的 robots.txt；若请求由用户发起（通过提示词），则不会遵守。可在配置的 `args` 列表中添加 `--ignore-robots-txt` 参数以禁用此行为。

### 自定义 - User-agent

默认情况下，根据请求来自模型（通过工具）还是用户发起（通过提示词），服务器将使用以下 user-agent 之一：
```
ModelContextProtocol/1.0 (Autonomous; +https://github.com/modelcontextprotocol/servers)
```
或
```
ModelContextProtocol/1.0 (User-Specified; +https://github.com/modelcontextprotocol/servers)
```

可在配置的 `args` 列表中添加 `--user-agent=YourUserAgent` 参数进行自定义。

### 自定义 - 代理

可通过 `--proxy-url` 参数配置服务器使用代理。

## Windows 配置

若在 Windows 上遇到超时问题，可能需要设置 `PYTHONIOENCODING` 环境变量以确保正确的字符编码：

<details>
<summary>Windows 配置（uvx）</summary>

```json
{
  "mcpServers": {
    "fetch": {
      "command": "uvx",
      "args": ["mcp-server-fetch"],
      "env": {
        "PYTHONIOENCODING": "utf-8"
      }
    }
  }
}
```
</details>

<details>
<summary>Windows 配置（pip）</summary>

```json
{
  "mcpServers": {
    "fetch": {
      "command": "python",
      "args": ["-m", "mcp_server_fetch"],
      "env": {
        "PYTHONIOENCODING": "utf-8"
      }
    }
  }
}
```
</details>

这可解决在 Windows 系统上可能导致服务器超时的字符编码问题。

## 调试

可使用 MCP inspector 调试服务器。对于 uvx 安装：

```
npx @modelcontextprotocol/inspector uvx mcp-server-fetch
```

若已在特定目录安装包或正在本地开发：

```
cd path/to/servers/src/fetch
npx @modelcontextprotocol/inspector uv run mcp-server-fetch
```

## 贡献

我们欢迎贡献，以帮助扩展和改进 mcp-server-fetch。无论您想添加新工具、增强现有功能还是改进文档，您的意见都很有价值。

其他 MCP 服务器及实现模式示例，请参阅：
https://github.com/modelcontextprotocol/servers

欢迎提交 Pull Request！欢迎贡献新想法、错误修复或增强，使 mcp-server-fetch 更强大、更有用。

## 许可证

mcp-server-fetch 采用 MIT 许可证。这意味着您可自由使用、修改和分发该软件，但须遵守 MIT 许可证的条款与条件。更多详情请参阅项目仓库中的 LICENSE 文件。
