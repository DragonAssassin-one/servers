# 文件系统 MCP 服务器

实现 Model Context Protocol (MCP) 文件系统操作的 Node.js 服务器。

## 功能

- 读/写文件
- 创建/列出/删除目录
- 移动文件/目录
- 搜索文件
- 获取文件元数据
- 通过 [Roots](https://modelcontextprotocol.io/docs/learn/client-concepts#roots) 动态控制目录访问

## 目录访问控制

服务器使用灵活的目录访问控制系统。可通过命令行参数或 [Roots](https://modelcontextprotocol.io/docs/learn/client-concepts#roots) 动态指定目录。

### 方法 1：命令行参数
启动服务器时指定允许的目录：
```bash
mcp-server-filesystem /path/to/dir1 /path/to/dir2
```

### 方法 2：MCP Roots（推荐）
支持 [Roots](https://modelcontextprotocol.io/docs/learn/client-concepts#roots) 的 MCP 客户端可动态更新允许的目录。

客户端通知服务器的 Roots 会完全替换服务器端配置的允许目录（当提供时）。

**重要**：若服务器启动时未提供命令行参数，且客户端不支持 roots 协议（或提供空 roots），服务器将在初始化期间抛出错误。

这是推荐方法，因为它支持通过 `roots/list_changed` 通知在运行时更新目录而无需重启服务器，提供更灵活、更现代的集成体验。

### 工作原理

服务器的目录访问控制遵循以下流程：

1. **服务器启动**
   - 服务器使用命令行参数中的目录启动（若已提供）
   - 若未提供参数，服务器以空的允许目录列表启动

2. **客户端连接与初始化**
   - 客户端连接并发送带能力的 `initialize` 请求
   - 服务器检查客户端是否支持 roots 协议（`capabilities.roots`）
   
3. **Roots 协议处理**（若客户端支持 roots）
   - **初始化时**：服务器通过 `roots/list` 向客户端请求 roots
   - 客户端返回其配置的 roots
   - 服务器用客户端的 roots 替换所有允许的目录
   - **运行时更新**：客户端可发送 `notifications/roots/list_changed`
   - 服务器请求更新的 roots 并再次替换允许的目录

4. **回退行为**（若客户端不支持 roots）
   - 服务器继续使用仅来自命令行的目录
   - 无法进行动态更新

5. **访问控制**
   - 所有文件系统操作仅限于允许的目录
   - 使用 `list_allowed_directories` 工具查看当前目录
   - 服务器需要至少一个允许的目录才能运行

**注意**：服务器仅允许在通过 `args` 或 Roots 指定的目录内进行操作。



## API

### 工具

- **read_text_file**
  - 以文本形式读取文件的完整内容
  - 输入：
    - `path` (string)
    - `head` (number, optional): 前 N 行
    - `tail` (number, optional): 后 N 行
  - 无论扩展名如何，始终将文件视为 UTF-8 文本
  - 不能同时指定 `head` 和 `tail`

- **read_media_file**
  - 读取图像或音频文件
  - 输入：
    - `path` (string)
  - 流式传输文件并返回带相应 MIME 类型的 base64 数据

- **read_multiple_files**
  - 同时读取多个文件
  - 输入：`paths` (string[])
  - 读取失败不会中止整个操作

- **write_file**
  - 创建新文件或覆盖现有文件（使用此工具时请谨慎）
  - 输入：
    - `path` (string): 文件位置
    - `content` (string): 文件内容

- **edit_file**
  - 使用高级模式匹配和格式化进行选择性编辑
  - 特性：
    - 基于行和多行内容匹配
    - 保留缩进的空白符规范化
    - 多个同时编辑且定位正确
    - 缩进风格检测与保留
    - 带上下文的 Git 风格 diff 输出
    - 试运行模式预览更改
  - 输入：
    - `path` (string): 要编辑的文件
    - `edits` (array): 编辑操作列表
      - `oldText` (string): 要搜索的文本（可为子串）
      - `newText` (string): 替换文本
    - `dryRun` (boolean): 预览更改而不应用（默认：false）
  - 试运行时返回详细 diff 和匹配信息，否则应用更改
  - 最佳实践：应用更改前始终先使用 dryRun 预览

- **create_directory**
  - 创建新目录或确保其存在
  - 输入：`path` (string)
  - 必要时创建父目录
  - 若目录已存在则静默成功

- **list_directory**
  - 列出目录内容，带 [FILE] 或 [DIR] 前缀
  - 输入：`path` (string)

- **list_directory_with_sizes**
  - 列出目录内容，带 [FILE] 或 [DIR] 前缀及文件大小
  - 输入：
    - `path` (string): 要列出的目录路径
    - `sortBy` (string, optional): 按 "name" 或 "size" 排序条目（默认："name"）
  - 返回含文件大小和汇总统计的详细列表
  - 显示文件、目录总数及合计大小

- **move_file**
  - 移动或重命名文件和目录
  - 输入：
    - `source` (string)
    - `destination` (string)
  - 若目标已存在则失败

- **search_files**
  - 递归搜索匹配或不匹配模式的文件/目录
  - 输入：
    - `path` (string): 起始目录
    - `pattern` (string): 搜索模式
    - `excludePatterns` (string[]): 排除的模式。
  - Glob 风格模式匹配
  - 返回匹配的完整路径

- **directory_tree**
  - 获取目录内容的递归 JSON 树结构
  - 输入：
    - `path` (string): 起始目录
    - `excludePatterns` (string[]): 排除的模式。支持 Glob 格式。
  - 返回：
    - JSON 数组，每个条目包含：
      - `name` (string): 文件/目录名
      - `type` ('file'|'directory'): 条目类型
      - `children` (array): 仅目录存在
        - 空目录为空数组
        - 文件省略
  - 输出使用 2 空格缩进以提高可读性
    
- **get_file_info**
  - 获取详细的文件/目录元数据
  - 输入：`path` (string)
  - 返回：
    - 大小
    - 创建时间
    - 修改时间
    - 访问时间
    - 类型（file/directory）
    - 权限

- **list_allowed_directories**
  - 列出服务器允许访问的所有目录
  - 无需输入
  - 返回：
    - 本服务器可读写的目录

### 工具注解（MCP 提示）

本服务器在每个工具上设置 [MCP ToolAnnotations](https://modelcontextprotocol.io/specification/2025-03-26/server/tools#toolannotations)，以便客户端可以：

- 区分**只读**工具与可写工具。
- 了解哪些写操作是**幂等**的（使用相同参数重试是安全的）。
- 标出可能具有**破坏性**的操作（覆盖或大量修改数据）。

文件系统工具的映射如下：

| Tool                        | readOnlyHint | idempotentHint | destructiveHint | 说明                                            |
|-----------------------------|--------------|----------------|-----------------|--------------------------------------------------|
| `read_text_file`            | `true`       | –              | –               | 纯读取                                           |
| `read_media_file`           | `true`       | –              | –               | 纯读取                                           |
| `read_multiple_files`       | `true`       | –              | –               | 纯读取                                           |
| `list_directory`            | `true`       | –              | –               | 纯读取                                           |
| `list_directory_with_sizes` | `true`       | –              | –               | 纯读取                                           |
| `directory_tree`            | `true`       | –              | –               | 纯读取                                           |
| `search_files`              | `true`       | –              | –               | 纯读取                                           |
| `get_file_info`             | `true`       | –              | –               | 纯读取                                           |
| `list_allowed_directories`  | `true`       | –              | –               | 纯读取                                           |
| `create_directory`          | `false`      | `true`         | `false`         | 重复创建同一目录为无操作                         |
| `write_file`                | `false`      | `true`         | `true`          | 覆盖现有文件                                     |
| `edit_file`                 | `false`      | `false`        | `true`          | 重复应用编辑可能失败或重复应用                   |
| `move_file`                 | `false`      | `false`        | `true`          | 删除源文件                                       |

> 注意：按 MCP 规范定义，仅当 `readOnlyHint` 为 `false` 时，`idempotentHint` 和 `destructiveHint` 才有意义。

## 在 Claude Desktop 中使用
将此配置添加到 `claude_desktop_config.json`：

注意：可将沙箱目录挂载到 `/projects` 以提供给服务器。添加 `ro` 标志将使目录对服务器为只读。

### Docker
注意：默认情况下，所有目录必须挂载到 `/projects`。

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "--mount", "type=bind,src=/Users/username/Desktop,dst=/projects/Desktop",
        "--mount", "type=bind,src=/path/to/other/allowed/dir,dst=/projects/other/allowed/dir,ro",
        "--mount", "type=bind,src=/path/to/file.txt,dst=/projects/path/to/file.txt",
        "mcp/filesystem",
        "/projects"
      ]
    }
  }
}
```

### NPX

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/Users/username/Desktop",
        "/path/to/other/allowed/dir"
      ]
    }
  }
}
```

在 Windows 上，使用 `cmd /c` 启动 `npx`：

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "cmd",
      "args": [
        "/c",
        "npx",
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/Users/username/Desktop",
        "/path/to/other/allowed/dir"
      ]
    }
  }
}
```

## 在 VS Code 中使用

快速安装可点击下方安装按钮……

[![Install with NPX in VS Code](https://img.shields.io/badge/VS_Code-NPM-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=filesystem&config=%7B%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22%40modelcontextprotocol%2Fserver-filesystem%22%2C%22%24%7BworkspaceFolder%7D%22%5D%7D) [![Install with NPX in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-NPM-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=filesystem&config=%7B%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22%40modelcontextprotocol%2Fserver-filesystem%22%2C%22%24%7BworkspaceFolder%7D%22%5D%7D&quality=insiders)

[![Install with Docker in VS Code](https://img.shields.io/badge/VS_Code-Docker-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=filesystem&config=%7B%22command%22%3A%22docker%22%2C%22args%22%3A%5B%22run%22%2C%22-i%22%2C%22--rm%22%2C%22--mount%22%2C%22type%3Dbind%2Csrc%3D%24%7BworkspaceFolder%7D%2Cdst%3D%2Fprojects%2Fworkspace%22%2C%22mcp%2Ffilesystem%22%2C%22%2Fprojects%22%5D%7D) [![Install with Docker in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-Docker-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=filesystem&config=%7B%22command%22%3A%22docker%22%2C%22args%22%3A%5B%22run%22%2C%22-i%22%2C%22--rm%22%2C%22--mount%22%2C%22type%3Dbind%2Csrc%3D%24%7BworkspaceFolder%7D%2Cdst%3D%2Fprojects%2Fworkspace%22%2C%22mcp%2Ffilesystem%22%2C%22%2Fprojects%22%5D%7D&quality=insiders)

手动安装时，可使用以下方法之一配置 MCP 服务器：

**方法 1：用户配置（推荐）**
将配置添加到用户级 MCP 配置文件。打开命令面板（`Ctrl + Shift + P`）并运行 `MCP: Open User Configuration`。这将打开用户 `mcp.json` 文件，您可在其中添加服务器配置。

**方法 2：工作区配置**
或者，可将配置添加到工作区中的 `.vscode/mcp.json` 文件。这样可与他人共享配置。

> 有关 VS Code 中 MCP 配置的更多详情，请参阅[官方 VS Code MCP 文档](https://code.visualstudio.com/docs/copilot/customization/mcp-servers)。

可将沙箱目录挂载到 `/projects` 以提供给服务器。添加 `ro` 标志将使目录对服务器为只读。

### Docker
注意：默认情况下，所有目录必须挂载到 `/projects`。

```json
{
  "servers": {
    "filesystem": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "--mount", "type=bind,src=${workspaceFolder},dst=/projects/workspace",
        "mcp/filesystem",
        "/projects"
      ]
    }
  }
}
```

### NPX

```json
{
  "servers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "${workspaceFolder}"
      ]
    }
  }
}
```

在 Windows 上，使用：

```json
{
  "servers": {
    "filesystem": {
      "command": "cmd",
      "args": [
        "/c",
        "npx",
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "${workspaceFolder}"
      ]
    }
  }
}
```

## 构建

Docker 构建：

```bash
docker build -t mcp/filesystem -f src/filesystem/Dockerfile .
```

## 许可证

本 MCP 服务器采用 MIT 许可证。这意味着您可自由使用、修改和分发该软件，但须遵守 MIT 许可证的条款与条件。更多详情请参阅项目仓库中的 LICENSE 文件。
