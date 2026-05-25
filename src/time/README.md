# Time MCP 服务器

<!-- mcp-name: io.github.modelcontextprotocol/server-time -->

提供时间与时区转换能力的 Model Context Protocol 服务器。本服务器使 LLM 能够获取当前时间信息，并使用 IANA 时区名称进行时区转换，同时自动检测系统时区。

### 可用工具

- `get_current_time` - 获取特定时区或系统时区的当前时间。
  - 必需参数：
    - `timezone` (string)：IANA 时区名称（例如 `'America/New_York'`、`'Europe/London'`）

- `convert_time` - 在时区之间转换时间。
  - 必需参数：
    - `source_timezone` (string)：源 IANA 时区名称
    - `time` (string)：24 小时制时间（HH:MM）
    - `target_timezone` (string)：目标 IANA 时区名称

## 安装

### 使用 uv（推荐）

使用 [`uv`](https://docs.astral.sh/uv/) 时无需单独安装。我们将通过 [`uvx`](https://docs.astral.sh/uv/guides/tools/) 直接运行 *mcp-server-time*。

```bash
uvx mcp-server-time
```

### 使用 PIP

也可通过 pip 安装 `mcp-server-time`：

```bash
pip install mcp-server-time
```

安装后，可作为脚本运行：

```bash
python -m mcp_server_time
```

## 配置

### 为 Claude.app 配置

添加到 Claude 设置：

<details>
<summary>使用 uvx</summary>

```json
{
  "mcpServers": {
    "time": {
      "command": "uvx",
      "args": ["mcp-server-time"]
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
    "time": {
      "command": "docker",
      "args": ["run", "-i", "--rm", "-e", "LOCAL_TIMEZONE", "mcp/time"]
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
    "time": {
      "command": "python",
      "args": ["-m", "mcp_server_time"]
    }
  }
}
```
</details>

### 为 Zed 配置

添加到 Zed 的 settings.json：

<details>
<summary>使用 uvx</summary>

```json
"context_servers": [
  "mcp-server-time": {
    "command": "uvx",
    "args": ["mcp-server-time"]
  }
],
```
</details>

<details>
<summary>使用 pip 安装</summary>

```json
"context_servers": {
  "mcp-server-time": {
    "command": "python",
    "args": ["-m", "mcp_server_time"]
  }
},
```
</details>

### 为 VS Code 配置

快速安装可使用下方一键安装按钮之一：

[![Install with UV in VS Code](https://img.shields.io/badge/VS_Code-UV-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=time&config=%7B%22command%22%3A%22uvx%22%2C%22args%22%3A%5B%22mcp-server-time%22%5D%7D) [![Install with UV in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-UV-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=time&config=%7B%22command%22%3A%22uvx%22%2C%22args%22%3A%5B%22mcp-server-time%22%5D%7D&quality=insiders)

[![Install with Docker in VS Code](https://img.shields.io/badge/VS_Code-Docker-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=time&config=%7B%22command%22%3A%22docker%22%2C%22args%22%3A%5B%22run%22%2C%22-i%22%2C%22--rm%22%2C%22mcp%2Ftime%22%5D%7D) [![Install with Docker in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-Docker-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=time&config=%7B%22command%22%3A%22docker%22%2C%22args%22%3A%5B%22run%22%2C%22-i%22%2C%22--rm%22%2C%22mcp%2Ftime%22%5D%7D&quality=insiders)

手动安装时，将以下 JSON 块添加到 VS Code 的用户设置（JSON）文件。可按 `Ctrl + Shift + P` 并输入 `Preferences: Open User Settings (JSON)`。

也可添加到工作区的 `.vscode/mcp.json` 文件，以便与他人共享配置。

> 注意：使用 `mcp.json` 文件时需要 `mcp` 键。

<details>
<summary>使用 uvx</summary>

```json
{
  "mcp": {
    "servers": {
      "time": {
        "command": "uvx",
        "args": ["mcp-server-time"]
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
      "time": {
        "command": "docker",
        "args": ["run", "-i", "--rm", "mcp/time"]
      }
    }
  }
}
```
</details>

### 为 Zencoder 配置

1. 打开 Zencoder 菜单（...）
2. 在下拉菜单中选择 `Agent Tools`
3. 点击 `Add Custom MCP`
4. 添加名称及下方服务器配置，并务必点击 `Install` 按钮

<details>
<summary>使用 uvx</summary>

```json
{
    "command": "uvx",
    "args": ["mcp-server-time"]
  }
```
</details>

### 自定义 - 系统时区

默认情况下，服务器会自动检测系统时区。可在配置的 `args` 列表中添加 `--local-timezone` 参数进行覆盖。

示例：
```json
{
  "command": "python",
  "args": ["-m", "mcp_server_time", "--local-timezone=America/New_York"]
}
```

## 交互示例

1. 获取当前时间：
```json
{
  "name": "get_current_time",
  "arguments": {
    "timezone": "Europe/Warsaw"
  }
}
```
响应：
```json
{
  "timezone": "Europe/Warsaw",
  "datetime": "2024-01-01T13:00:00+01:00",
  "is_dst": false
}
```

2. 在时区之间转换时间：
```json
{
  "name": "convert_time",
  "arguments": {
    "source_timezone": "America/New_York",
    "time": "16:30",
    "target_timezone": "Asia/Tokyo"
  }
}
```
响应：
```json
{
  "source": {
    "timezone": "America/New_York",
    "datetime": "2024-01-01T12:30:00-05:00",
    "is_dst": false
  },
  "target": {
    "timezone": "Asia/Tokyo",
    "datetime": "2024-01-01T12:30:00+09:00",
    "is_dst": false
  },
  "time_difference": "+13.0h",
}
```

## 调试

可使用 MCP inspector 调试服务器。对于 uvx 安装：

```bash
npx @modelcontextprotocol/inspector uvx mcp-server-time
```

若已在特定目录安装包或正在本地开发：

```bash
cd path/to/servers/src/time
npx @modelcontextprotocol/inspector uv run mcp-server-time
```

## 向 Claude 提问的示例

1. "现在几点了？"（将使用系统时区）
2. "东京现在几点？"
3. "纽约下午 4 点时，伦敦是几点？"
4. "把东京时间上午 9:30 转换成纽约时间"

## 构建

Docker 构建：

```bash
cd src/time
docker build -t mcp/time .
```

## 贡献

我们欢迎贡献，以帮助扩展和改进 mcp-server-time。无论您想添加新的时间相关工具、增强现有功能还是改进文档，您的意见都很有价值。

其他 MCP 服务器及实现模式示例，请参阅：
https://github.com/modelcontextprotocol/servers

欢迎提交 Pull Request！欢迎贡献新想法、错误修复或增强，使 mcp-server-time 更强大、更有用。

## 许可证

mcp-server-time 采用 MIT 许可证。这意味着您可自由使用、修改和分发该软件，但须遵守 MIT 许可证的条款与条件。更多详情请参阅项目仓库中的 LICENSE 文件。
