# mcp-server-git：Git MCP 服务器

<!-- mcp-name: io.github.modelcontextprotocol/server-git -->

## 概览

用于 Git 仓库交互与自动化的 Model Context Protocol 服务器。本服务器提供工具，供大语言模型读取、搜索和操作 Git 仓库。

请注意，mcp-server-git 目前仍处于早期开发阶段。随着持续开发与改进，功能与可用工具可能会变更和扩展。

### 工具

1. `git_status`
   - 显示工作树状态
   - 输入：
     - `repo_path` (string)：Git 仓库路径
   - 返回：工作目录当前状态的文本输出

2. `git_diff_unstaged`
   - 显示工作目录中尚未暂存的更改
   - 输入：
     - `repo_path` (string)：Git 仓库路径
     - `context_lines` (number, optional)：显示的上下文行数（默认：3）
   - 返回：未暂存更改的 diff 输出

3. `git_diff_staged`
   - 显示已暂存待提交的更改
   - 输入：
     - `repo_path` (string)：Git 仓库路径
     - `context_lines` (number, optional)：显示的上下文行数（默认：3）
   - 返回：已暂存更改的 diff 输出

4. `git_diff`
   - 显示分支或提交之间的差异
   - 输入：
     - `repo_path` (string)：Git 仓库路径
     - `target` (string)：用于比较的目标分支或提交
     - `context_lines` (number, optional)：显示的上下文行数（默认：3）
   - 返回：将当前状态与目标进行比较的 diff 输出

5. `git_commit`
   - 将更改记录到仓库
   - 输入：
     - `repo_path` (string)：Git 仓库路径
     - `message` (string)：提交说明
   - 返回：包含新提交哈希的确认信息

6. `git_add`
   - 将文件内容添加到暂存区
   - 输入：
     - `repo_path` (string)：Git 仓库路径
     - `files` (string[])：要暂存的文件路径数组
   - 返回：已暂存文件的确认信息

7. `git_reset`
   - 取消暂存所有已暂存的更改
   - 输入：
     - `repo_path` (string)：Git 仓库路径
   - 返回：重置操作的确认信息

8. `git_log`
   - 显示提交日志，支持可选的日期筛选
   - 输入：
     - `repo_path` (string)：Git 仓库路径
     - `max_count` (number, optional)：显示的最大提交数（默认：10）
     - `start_timestamp` (string, optional)：筛选提交的起始时间戳。接受 ISO 8601 格式（例如 `'2024-01-15T14:30:25'`）、相对日期（例如 `'2 weeks ago'`、`'yesterday'`）或绝对日期（例如 `'2024-01-15'`、`'Jan 15 2024'`）
     - `end_timestamp` (string, optional)：筛选提交的结束时间戳。接受 ISO 8601 格式（例如 `'2024-01-15T14:30:25'`）、相对日期（例如 `'2 weeks ago'`、`'yesterday'`）或绝对日期（例如 `'2024-01-15'`、`'Jan 15 2024'`）
   - 返回：包含哈希、作者、日期和说明的提交条目数组

9. `git_create_branch`
   - 创建新分支
   - 输入：
     - `repo_path` (string)：Git 仓库路径
     - `branch_name` (string)：新分支名称
     - `base_branch` (string, optional)：创建所基于的基础分支（默认为当前分支）
   - 返回：分支创建的确认信息
10. `git_checkout`
   - 切换分支
   - 输入：
     - `repo_path` (string)：Git 仓库路径
     - `branch_name` (string)：要检出的分支名称
   - 返回：分支切换的确认信息
11. `git_show`
   - 显示某次提交的内容
   - 输入：
     - `repo_path` (string)：Git 仓库路径
     - `revision` (string)：要显示的修订版本（提交哈希、分支名、标签）
   - 返回：指定提交的内容

12. `git_branch`
   - 列出 Git 分支
   - 输入：
     - `repo_path` (string)：Git 仓库路径
     - `branch_type` (string)：列出本地分支（`'local'`）、远程分支（`'remote'`）或全部分支（`'all'`）
     - `contains` (string, optional)：分支应包含的提交 SHA。若未指定提交 SHA，请勿向此参数传入任何值
     - `not_contains` (string, optional)：分支不应包含的提交 SHA。若未指定提交 SHA，请勿向此参数传入任何值
   - 返回：分支列表

## 安装

### 使用 uv（推荐）

使用 [`uv`](https://docs.astral.sh/uv/) 时无需单独安装。我们将通过 [`uvx`](https://docs.astral.sh/uv/guides/tools/) 直接运行 *mcp-server-git*。

### 使用 PIP

也可通过 pip 安装 `mcp-server-git`：

```
pip install mcp-server-git
```

安装后，可作为脚本运行：

```
python -m mcp_server_git
```

## 配置

### 在 Claude Desktop 中使用

将以下内容添加到 `claude_desktop_config.json`：

<details>
<summary>使用 uvx</summary>

```json
"mcpServers": {
  "git": {
    "command": "uvx",
    "args": ["mcp-server-git", "--repository", "path/to/git/repo"]
  }
}
```
</details>

<details>
<summary>使用 docker</summary>

* 注意：将 `/Users/username` 替换为您希望此工具可访问的路径

```json
"mcpServers": {
  "git": {
    "command": "docker",
    "args": ["run", "--rm", "-i", "--mount", "type=bind,src=/Users/username,dst=/Users/username", "mcp/git"]
  }
}
```
</details>

<details>
<summary>使用 pip 安装</summary>

```json
"mcpServers": {
  "git": {
    "command": "python",
    "args": ["-m", "mcp_server_git", "--repository", "path/to/git/repo"]
  }
}
```
</details>

### 在 VS Code 中使用

快速安装可使用下方一键安装按钮之一：

[![Install with UV in VS Code](https://img.shields.io/badge/VS_Code-UV-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=git&config=%7B%22command%22%3A%22uvx%22%2C%22args%22%3A%5B%22mcp-server-git%22%5D%7D) [![Install with UV in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-UV-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=git&config=%7B%22command%22%3A%22uvx%22%2C%22args%22%3A%5B%22mcp-server-git%22%5D%7D&quality=insiders)

[![Install with Docker in VS Code](https://img.shields.io/badge/VS_Code-Docker-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=git&config=%7B%22command%22%3A%22docker%22%2C%22args%22%3A%5B%22run%22%2C%22--rm%22%2C%22-i%22%2C%22--mount%22%2C%22type%3Dbind%2Csrc%3D%24%7BworkspaceFolder%7D%2Cdst%3D%2Fworkspace%22%2C%22mcp%2Fgit%22%5D%7D) [![Install with Docker in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-Docker-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=git&config=%7B%22command%22%3A%22docker%22%2C%22args%22%3A%5B%22run%22%2C%22--rm%22%2C%22-i%22%2C%22--mount%22%2C%22type%3Dbind%2Csrc%3D%24%7BworkspaceFolder%7D%2Cdst%3D%2Fworkspace%22%2C%22mcp%2Fgit%22%5D%7D&quality=insiders)

手动安装时，可使用以下方法之一配置 MCP 服务器：

**方法 1：用户配置（推荐）**
将配置添加到用户级 MCP 配置文件。打开命令面板（`Ctrl + Shift + P`）并运行 `MCP: Open User Configuration`。这将打开用户 `mcp.json` 文件，您可在其中添加服务器配置。

**方法 2：工作区配置**
或者，可将配置添加到工作区中的 `.vscode/mcp.json` 文件。这样可与他人共享配置。

> 有关 VS Code 中 MCP 配置的更多详情，请参阅[官方 VS Code MCP 文档](https://code.visualstudio.com/docs/copilot/customization/mcp-servers)。

```json
{
  "servers": {
    "git": {
      "command": "uvx",
      "args": ["mcp-server-git"]
    }
  }
}
```

Docker 安装：

```json
{
  "mcp": {
    "servers": {
      "git": {
        "command": "docker",
        "args": [
          "run",
          "--rm",
          "-i",
          "--mount", "type=bind,src=${workspaceFolder},dst=/workspace",
          "mcp/git"
        ]
      }
    }
  }
}
```

### 在 [Zed](https://github.com/zed-industries/zed) 中使用

添加到 Zed 的 settings.json：

<details>
<summary>使用 uvx</summary>

```json
"context_servers": [
  "mcp-server-git": {
    "command": {
      "path": "uvx",
      "args": ["mcp-server-git"]
    }
  }
],
```
</details>

<details>
<summary>使用 pip 安装</summary>

```json
"context_servers": {
  "mcp-server-git": {
    "command": {
      "path": "python",
      "args": ["-m", "mcp_server_git"]
    }
  }
},
```
</details>

### 在 [Zencoder](https://zencoder.ai) 中使用

1. 打开 Zencoder 菜单（...）
2. 在下拉菜单中选择 `Agent Tools`
3. 点击 `Add Custom MCP`
4. 添加名称（例如 git）及下方服务器配置，并务必点击 `Install` 按钮

<details>
<summary>使用 uvx</summary>

```json
{
    "command": "uvx",
    "args": ["mcp-server-git", "--repository", "path/to/git/repo"]
}
```
</details>

## 调试

可使用 MCP inspector 调试服务器。对于 uvx 安装：

```
npx @modelcontextprotocol/inspector uvx mcp-server-git
```

若已在特定目录安装包或正在本地开发：

```
cd path/to/servers/src/git
npx @modelcontextprotocol/inspector uv run mcp-server-git
```

运行 `tail -n 20 -f ~/Library/Logs/Claude/mcp*.log` 可查看服务器日志，有助于排查问题。

## 开发

若进行本地开发，有两种方式测试更改：

1. 运行 MCP inspector 测试更改。运行说明见[调试](#调试)。

2. 使用 Claude 桌面应用测试。将以下内容添加到 `claude_desktop_config.json`：

### Docker

```json
{
  "mcpServers": {
    "git": {
      "command": "docker",
      "args": [
        "run",
        "--rm",
        "-i",
        "--mount", "type=bind,src=/Users/username/Desktop,dst=/projects/Desktop",
        "--mount", "type=bind,src=/path/to/other/allowed/dir,dst=/projects/other/allowed/dir,ro",
        "--mount", "type=bind,src=/path/to/file.txt,dst=/projects/path/to/file.txt",
        "mcp/git"
      ]
    }
  }
}
```

### UVX
```json
{
"mcpServers": {
  "git": {
    "command": "uv",
    "args": [
      "--directory",
      "/<path to mcp-servers>/mcp-servers/src/git",
      "run",
      "mcp-server-git"
    ]
    }
  }
}
```

## 构建

Docker 构建：

```bash
cd src/git
docker build -t mcp/git .
```

## 许可证

本 MCP 服务器采用 MIT 许可证。这意味着您可自由使用、修改和分发该软件，但须遵守 MIT 许可证的条款与条件。更多详情请参阅项目仓库中的 LICENSE 文件。
