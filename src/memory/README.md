# 知识图谱记忆服务器

基于本地知识图谱的持久化记忆基础实现。这让 Claude 能够在多次对话之间记住关于用户的信息。

## 核心概念

### 实体
实体是知识图谱中的主要节点。每个实体具有：
- 唯一名称（标识符）
- 实体类型（例如 "person"、"organization"、"event"）
- 观察列表

示例：
```json
{
  "name": "John_Smith",
  "entityType": "person",
  "observations": ["Speaks fluent Spanish"]
}
```

### 关系
关系定义实体之间的有向连接。它们始终以主动语态存储，描述实体如何交互或相互关联。

示例：
```json
{
  "from": "John_Smith",
  "to": "Anthropic",
  "relationType": "works_at"
}
```
### 观察
观察是关于实体的离散信息片段。它们：

- 以字符串形式存储
- 附加到特定实体
- 可独立添加或删除
- 应为原子性（每条观察一个事实）

示例：
```json
{
  "entityName": "John_Smith",
  "observations": [
    "Speaks fluent Spanish",
    "Graduated in 2019",
    "Prefers morning meetings"
  ]
}
```

## API

### 工具
- **create_entities**
  - 在知识图谱中创建多个新实体
  - 输入：`entities`（对象数组）
    - 每个对象包含：
      - `name`（string）：实体标识符
      - `entityType`（string）：类型分类
      - `observations`（string[]）：关联的观察
  - 忽略名称已存在的实体

- **create_relations**
  - 在实体之间创建多个新关系
  - 输入：`relations`（对象数组）
    - 每个对象包含：
      - `from`（string）：源实体名称
      - `to`（string）：目标实体名称
      - `relationType`（string）：主动语态的关系类型
  - 跳过重复的关系

- **add_observations**
  - 向现有实体添加新观察
  - 输入：`observations`（对象数组）
    - 每个对象包含：
      - `entityName`（string）：目标实体
      - `contents`（string[]）：要添加的新观察
  - 返回每个实体已添加的观察
  - 若实体不存在则失败

- **delete_entities**
  - 删除实体及其关系
  - 输入：`entityNames`（string[]）
  - 级联删除关联的关系
  - 若实体不存在则静默操作

- **delete_observations**
  - 从实体中删除特定观察
  - 输入：`deletions`（对象数组）
    - 每个对象包含：
      - `entityName`（string）：目标实体
      - `observations`（string[]）：要删除的观察
  - 若观察不存在则静默操作

- **delete_relations**
  - 从图谱中删除特定关系
  - 输入：`relations`（对象数组）
    - 每个对象包含：
      - `from`（string）：源实体名称
      - `to`（string）：目标实体名称
      - `relationType`（string）：关系类型
  - 若关系不存在则静默操作

- **read_graph**
  - 读取整个知识图谱
  - 无需输入
  - 返回包含所有实体和关系的完整图谱结构

- **search_nodes**
  - 根据查询搜索节点
  - 输入：`query`（string）
  - 搜索范围包括：
    - 实体名称
    - 实体类型
    - 观察内容
  - 返回匹配的实体及其关系

- **open_nodes**
  - 按名称检索特定节点
  - 输入：`names`（string[]）
  - 返回：
    - 请求的实体
    - 请求实体之间的关系
  - 静默跳过不存在的节点

# 在 Claude Desktop 中使用

### 设置

将此配置添加到 `claude_desktop_config.json`：

#### Docker

```json
{
  "mcpServers": {
    "memory": {
      "command": "docker",
      "args": ["run", "-i", "-v", "claude-memory:/app/dist", "--rm", "mcp/memory"]
    }
  }
}
```

#### NPX
```json
{
  "mcpServers": {
    "memory": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-memory"
      ]
    }
  }
}
```

在 Windows 上，使用 `cmd /c` 启动 `npx`：

```json
{
  "mcpServers": {
    "memory": {
      "command": "cmd",
      "args": [
        "/c",
        "npx",
        "-y",
        "@modelcontextprotocol/server-memory"
      ]
    }
  }
}
```

#### 带自定义设置的 NPX

可使用以下环境变量配置服务器：

```json
{
  "mcpServers": {
    "memory": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-memory"
      ],
      "env": {
        "MEMORY_FILE_PATH": "/path/to/custom/memory.jsonl"
      }
    }
  }
}
```

在 Windows 上，使用：

```json
{
  "mcpServers": {
    "memory": {
      "command": "cmd",
      "args": [
        "/c",
        "npx",
        "-y",
        "@modelcontextprotocol/server-memory"
      ],
      "env": {
        "MEMORY_FILE_PATH": "/path/to/custom/memory.jsonl"
      }
    }
  }
}
```

- `MEMORY_FILE_PATH`：记忆存储 JSONL 文件的路径（默认：服务器目录下的 `memory.jsonl`）

# VS Code 安装说明

快速安装可使用下方一键安装按钮之一：

[![Install with NPX in VS Code](https://img.shields.io/badge/VS_Code-NPM-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=memory&config=%7B%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22%40modelcontextprotocol%2Fserver-memory%22%5D%7D) [![Install with NPX in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-NPM-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=memory&config=%7B%22command%22%3A%22npx%22%2C%22args%22%3A%5B%22-y%22%2C%22%40modelcontextprotocol%2Fserver-memory%22%5D%7D&quality=insiders)

[![Install with Docker in VS Code](https://img.shields.io/badge/VS_Code-Docker-0098FF?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=memory&config=%7B%22command%22%3A%22docker%22%2C%22args%22%3A%5B%22run%22%2C%22-i%22%2C%22-v%22%2C%22claude-memory%3A%2Fapp%2Fdist%22%2C%22--rm%22%2C%22mcp%2Fmemory%22%5D%7D) [![Install with Docker in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-Docker-24bfa5?style=flat-square&logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=memory&config=%7B%22command%22%3A%22docker%22%2C%22args%22%3A%5B%22run%22%2C%22-i%22%2C%22-v%22%2C%22claude-memory%3A%2Fapp%2Fdist%22%2C%22--rm%22%2C%22mcp%2Fmemory%22%5D%7D&quality=insiders)

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
    "memory": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-memory"
      ]
    }
  }
}
```

在 Windows 上，使用：

```json
{
  "servers": {
    "memory": {
      "command": "cmd",
      "args": [
        "/c",
        "npx",
        "-y",
        "@modelcontextprotocol/server-memory"
      ]
    }
  }
}
```

#### Docker

```json
{
  "servers": {
    "memory": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "-v",
        "claude-memory:/app/dist",
        "--rm",
        "mcp/memory"
      ]
    }
  }
}
```

### 系统提示词

使用记忆的提示词取决于用例。更改提示词有助于模型确定创建记忆的频率和类型。

以下是用于聊天个性化的示例提示词。您可将此提示词用于 [Claude.ai Project](https://www.anthropic.com/news/projects) 的「Custom Instructions」字段。

```
每次交互请遵循以下步骤：

1. 用户识别：
   - 假定您正在与 default_user 交互
   - 若尚未识别 default_user，请主动尝试识别。

2. 记忆检索：
   - 每次对话开始时仅说「Remembering...」，并从知识图谱中检索所有相关信息
   - 始终将知识图谱称为您的「memory」

3. 记忆
   - 与用户交谈时，留意属于以下类别的新信息：
     a) 基本身份（年龄、性别、地点、职位、教育程度等）
     b) 行为（兴趣、习惯等）
     c) 偏好（沟通风格、首选语言等）
     d) 目标（目标、志向等）
     e) 关系（个人与职业关系，最多三层关系）

4. 记忆更新：
   - 若交互中收集到新信息，请按以下方式更新记忆：
     a) 为反复出现的组织、人物和重要事件创建实体
     b) 使用关系将它们与当前实体连接
     c) 将相关事实存储为观察
```

## 构建

Docker：

```sh
docker build -t mcp/memory -f src/memory/Dockerfile . 
```

请注意：先前的 mcp/memory 卷包含一个 `index.js` 文件，可能被新容器覆盖。若使用 docker 卷存储，请在启动新容器前删除旧 docker 卷中的 `index.js` 文件。

## 许可证

本 MCP 服务器采用 MIT 许可证。这意味着您可自由使用、修改和分发该软件，但须遵守 MIT 许可证的条款与条件。更多详情请参阅项目仓库中的 LICENSE 文件。
