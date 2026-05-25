# Everything 服务器 - 启动流程

**[架构](architecture.md)
| [项目结构](structure.md)
| 启动流程
| [服务器功能](features.md)
| [扩展点](extension.md)
| [工作原理](how-it-works.md)**

## 1. Everything 服务器启动器

- 用法：`node dist/index.js [stdio|sse|streamableHttp]`
- 运行指定的**传输管理器**以处理客户端连接。
- 在命令行指定传输类型（默认 `stdio`）
  - `stdio` → `transports/stdio.js`
  - `sse` → `transports/sse.js`
  - `streamableHttp` → `transports/streamableHttp.js`

## 2. 传输管理器

- 使用 `server/index.ts` 中的 `createServer()` 创建服务器实例
  - 将其连接到 MCP SDK 中选择的传输类型。
- 按所选传输的 MCP 规范处理通信。
  - **STDIO**：
    - 单一、绑定进程的连接。
    - 连接时调用 `clientConnect()`。
    - 在 `SIGINT` 时关闭并调用 `cleanup()`。
  - **SSE**：
    - 支持多个客户端连接。
    - 客户端传输映射到 `sessionId`；
    - 连接时调用 `clientConnect(sessionId)`。
    - 将服务器的 `onclose` 挂钩以清理并移除会话。
    - 暴露
      - `/sse` **GET**（SSE 流）
      - `/message` **POST**（JSON-RPC 消息）
  - **Streamable HTTP**：
    - 支持多个客户端连接。
    - 客户端传输映射到 `sessionId`；
    - 连接时调用 `clientConnect(sessionId)`。
    - 在 `/mcp` 上暴露
      - **POST**（JSON-RPC 消息）
      - **GET**（SSE 流）
      - **DELETE**（终止）
    - 使用事件存储实现可恢复性，并按 `sessionId` 存储传输。
    - 在 **DELETE** 时调用 `cleanup(sessionId)`。

## 3. 服务器工厂

- 调用 `server/index.ts` 中的 `createServer()`
- 创建新的 `McpServer` 实例，包含
  - **能力**：
    - `tools: {}`
    - `logging: {}`
    - `prompts: {}`
    - `resources: { subscribe: true }`
  - **服务器说明**
    - 从 docs 文件夹加载（`server-instructions.md`）。
  - **注册**
    - 通过 `registerTools(server)` 注册**工具**。
    - 通过 `registerResources(server)` 注册**资源**。
    - 通过 `registerPrompts(server)` 注册**提示**。
  - **其他请求处理器**
    - 通过 `setSubscriptionHandlers(server)` 设置资源订阅处理器。
    - 根目录列表变更处理器在连接后通过
  - **返回值**
    - `McpServer` 实例
    - `clientConnect(sessionId)` 回调，用于启用连接后设置
    - `cleanup(sessionId?)` 回调，用于停止活动间隔并移除会话作用域状态

## 启用多客户端

`transports` 文件夹中定义的部分传输管理器可支持多个客户端。
为此，必须将特定数据映射到会话标识符。
