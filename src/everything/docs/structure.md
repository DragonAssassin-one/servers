# Everything 服务器 - 项目结构

**[架构](architecture.md)
| 项目结构
| [启动流程](startup.md)
| [服务器功能](features.md)
| [扩展点](extension.md)
| [工作原理](how-it-works.md)**

```
src/everything
     ├── index.ts
     ├── AGENTS.md
     ├── package.json
     ├── docs
     │   ├── architecture.md
     │   ├── extension.md
     │   ├── features.md
     │   ├── how-it-works.md
     │   ├── instructions.md
     │   ├── startup.md
     │   └── structure.md
     ├── prompts
     │   ├── index.ts
     │   ├── args.ts
     │   ├── completions.ts
     │   ├── simple.ts
     │   └── resource.ts
     ├── resources
     │   ├── index.ts
     │   ├── files.ts
     │   ├── session.ts
     │   ├── subscriptions.ts
     │   └── templates.ts
     ├── server
     │   ├── index.ts
     │   ├── logging.ts
     │   └── roots.ts
     ├── tools
     │   ├── index.ts
     │   ├── echo.ts
     │   ├── get-annotated-message.ts
     │   ├── get-env.ts
     │   ├── get-resource-links.ts
     │   ├── get-resource-reference.ts
     │   ├── get-roots-list.ts
     │   ├── get-structured-content.ts
     │   ├── get-sum.ts
     │   ├── get-tiny-image.ts
     │   ├── gzip-file-as-resource.ts
     │   ├── simulate-research-query.ts
     │   ├── toggle-simulated-logging.ts
     │   ├── toggle-subscriber-updates.ts
     │   ├── trigger-elicitation-request.ts
     │   ├── trigger-elicitation-request-async.ts
     │   ├── trigger-long-running-operation.ts
     │   ├── trigger-sampling-request.ts
     │   └── trigger-sampling-request-async.ts
     └── transports
         ├── sse.ts
         ├── stdio.ts
         └── streamableHttp.ts
```

# 项目内容

## `src/everything`：

### `index.ts`

- CLI 入口点，根据第一个 CLI 参数选择并运行指定传输模块：`stdio`、`sse` 或 `streamableHttp`。

### `AGENTS.md`

- 面向代理/LLM 的指引，说明编码规范及如何正确扩展服务器。

### `package.json`

- 包元数据与脚本：
  - `build`：将 TypeScript 编译到 `dist/`，将 `docs/` 复制到 `dist/`，并将编译后的入口脚本标记为可执行。
  - `start:stdio`、`start:sse`、`start:streamableHttp`：从 `dist/` 运行已构建的传输。
- 声明对 `@modelcontextprotocol/sdk`、`express`、`cors`、`zod` 等的依赖。

### `docs/`

- `architecture.md`
  - 本文档。
- `server-instructions.md`
  - 面向人类可读的说明，旨在作为客户端/LLM 使用服务器的指引。在启动时由服务器加载，并在 "initialize" 交换中返回。

### `prompts/`

- `index.ts`
  - `registerPrompts(server)` 编排器；委托各提示文件中的提示工厂/注册方法。
- `simple.ts`
  - 注册 `simple-prompt`：无参数提示，返回单条用户消息。
- `args.ts`
  - 注册 `args-prompt`：带两个参数（`city` 必填，`state` 可选）的提示，用于组合消息。
- `completions.ts`
  - 注册 `completable-prompt`：其参数支持使用 SDK 的 `completable(...)` 辅助函数进行服务端驱动的补全（例如补全 `department` 及上下文相关的 `name`）。
- `resource.ts`
  - 暴露 `registerEmbeddedResourcePrompt(server)`，注册 `resource-prompt`——接受 `resourceType`（"Text" 或 "Blob"）和 `resourceId`（整数）的提示，并在返回的消息中嵌入按请求类型动态生成的资源。内部复用 `resources/templates.ts` 中的辅助函数。

### `resources/`

- `index.ts`
  - `registerResources(server)` 编排器；委托各资源文件中的资源工厂/注册方法。
- `templates.ts`
  - 使用 `ResourceTemplate` 注册两个动态、模板驱动的资源：
    - Text：`demo://resource/dynamic/text/{index}`（MIME：`text/plain`）
    - Blob：`demo://resource/dynamic/blob/{index}`（MIME：`application/octet-stream`，Base64 载荷）
  - `{index}` 路径变量必须是有限正整数。内容按需生成并带时间戳。
  - 暴露辅助函数 `textResource(uri, index)`、`textResourceUri(index)`、`blobResource(uri, index)` 和 `blobResourceUri(index)`，供其他模块直接构造并嵌入动态资源（例如从提示中）。
- `files.ts`
  - 为 `docs/` 文件夹中的每个文件注册静态基于文件的资源。
  - URI 遵循模式：`demo://resource/static/document/<filename>`。
  - Markdown 以 `text/markdown` 提供，`.txt` 为 `text/plain`，`.json` 为 `application/json`，其他默认为 `text/plain`。

### `server/`

- `index.ts`
  - 服务器工厂，创建带已声明能力的 `McpServer`，加载服务器说明，并注册工具、提示和资源。
  - 通过 `setSubscriptionHandlers(server)` 设置资源订阅处理器。
  - 向所选传输暴露 `{ server, cleanup }`。传输断开时，`cleanup` 停止服务器中所有运行中的间隔。
- `logging.ts`
  - 实现模拟日志。定期向已连接客户端会话发送各级别的随机日志消息。可通过专用工具按需启动/停止。

### `tools/`

- `index.ts`
  - `registerTools(server)` 编排器；委托各工具文件中的工具工厂/注册方法。
- `echo.ts`
  - 注册 `echo` 工具，接受消息并返回 `Echo: {message}`。
- `get-annotated-message.ts`
  - 注册 `get-annotated-message` 工具，演示内容级注解。发出主 `text` 消息，其内容 `annotations`（`priority`、`audience`）随 `messageType`（`"error" | "success" | "debug"`）变化；当 `includeImage` 为 true 时可选包含带注解的 `image`（小型 PNG）。本服务器中所有工具均包含工具级注解（`readOnlyHint`、`destructiveHint`、`idempotentHint`、`openWorldHint`）。
- `get-env.ts`
  - 注册 `get-env` 工具，将当前进程环境变量以格式化 JSON 文本返回；便于调试配置。
- `get-resource-links.ts`
  - 注册 `get-resource-links` 工具，返回引导性 `text` 块及多个 `resource_link` 项。
- `get-resource-reference.ts`
  - 注册 `get-resource-reference` 工具，返回所选动态资源的引用。
- `get-roots-list.ts`
  - 注册 `get-roots-list` 工具，返回客户端发送的最近一次根目录列表。
- `gzip-file-as-resource.ts`
  - 注册 `gzip-file-as-resource` 工具，从 URL 或 data URI 抓取内容、压缩，然后：
    - 返回指向会话作用域资源的 `resource_link`（默认），或
    - 返回内联 `resource`（含 gzip 数据）。该资源在会话存续期间仍可通过 `resources/list` 发现。
  - 使用 `resources/session.ts` 将 gzip blob 注册为按会话资源，URI 形如 `demo://resource/session/<name>`，`mimeType: application/gzip`。
  - 环境变量控制：
    - `GZIP_MAX_FETCH_SIZE`（字节，默认 10 MiB）
    - `GZIP_MAX_FETCH_TIME_MILLIS`（毫秒，默认 30000）
    - `GZIP_ALLOWED_DOMAINS`（逗号分隔允许列表；为空表示允许所有域名）
- `simulate-research-query.ts`
  - 注册基于任务的 `simulate-research-query` 工具，演示 MCP Tasks 功能（SEP-1686）。模拟多阶段研究操作并带进度更新。若查询被标记为模糊且客户端支持引导，则在执行中途通过 `elicitation/create` 暂停以请求澄清。使用 `server.experimental.tasks.registerToolTask()`，配置 `execution: { taskSupport: "required" }`。
- `trigger-elicitation-request.ts`
  - 注册 `trigger-elicitation-request` 工具，向客户端/LLM 发送 `elicitation/create` 请求并返回引导结果。
- `trigger-elicitation-request-async.ts`
  - 注册 `trigger-elicitation-request-async` 工具，演示用于引导的双向 MCP 任务。发送带任务元数据的引导请求，然后轮询客户端的 `tasks/get` 端点获取完成状态，再获取最终结果。
- `trigger-sampling-request.ts`
  - 注册 `trigger-sampling-request` 工具，向客户端/LLM 发送 `sampling/createMessage` 请求并返回采样结果。
- `trigger-sampling-request-async.ts`
  - 注册 `trigger-sampling-request-async` 工具，演示用于采样的双向 MCP 任务。发送带任务元数据的采样请求，然后轮询客户端的 `tasks/get` 端点获取完成状态，再获取最终结果。
- `get-structured-content.ts`
  - 注册 `get-structured-content` 工具，演示 structuredContent 块响应。
- `get-sum.ts`
  - 注册 `get-sum` 工具，使用 Zod 输入 schema 对两个数 `a` 和 `b` 求和并返回结果。
- `get-tiny-image.ts`
  - 注册 `get-tiny-image` 工具，以 `image` 内容项返回小型 PNG MCP 徽标，前后附带描述性 `text` 项。
- `trigger-long-running-operation.ts`
  - 注册 `trigger-long-running-operation` 工具，在指定 `duration`（秒）和 `steps` 数量下模拟长时间运行任务；当客户端提供 `progressToken` 时发出 `notifications/progress` 更新。
- `toggle-simulated-logging.ts`
  - 注册 `toggle-simulated-logging` 工具，为调用会话启动或停止模拟日志。
- `toggle-subscriber-updates.ts`
  - 注册 `toggle-subscriber-updates` 工具，为调用会话启动或停止模拟资源订阅更新检查。

### `transports/`

- `stdio.ts`
  - 启动 `StdioServerTransport`，通过 `createServer()` 创建服务器并连接。
  - 处理 `SIGINT` 以干净关闭，并调用 `cleanup()` 移除所有活动间隔。
- `sse.ts`
  - Express 服务器暴露：
    - `GET /sse` 为每个会话建立 SSE 连接。
    - `POST /message` 用于客户端消息。
  - 通过传输映射管理多个已连接客户端。
  - 启动 `SSEServerTransport`，通过 `createServer()` 创建服务器并连接到新传输。
  - 服务器断开时调用 `cleanup()` 移除所有活动间隔。
- `streamableHttp.ts`
  - Express 服务器在单一 `/mcp` 端点上暴露 POST（JSON-RPC）、GET（SSE 流）和 DELETE（会话终止），使用 `StreamableHTTPServerTransport`。
  - 使用 `InMemoryEventStore` 实现可恢复会话，并按 `sessionId` 跟踪传输。
  - 在初始化 POST 时连接新的服务器实例，后续请求复用该传输。
