# Everything 服务器 - 功能

**[架构](architecture.md)
| [项目结构](structure.md)
| [启动流程](startup.md)
| 服务器功能
| [扩展点](extension.md)
| [工作原理](how-it-works.md)**

## 工具

- `echo` (tools/echo.ts)：回显提供的 `message: string`。使用 Zod 校验输入。
- `get-annotated-message` (tools/get-annotated-message.ts)：根据 `messageType`（`error`、`success` 或 `debug`）返回带 `priority` 和 `audience` 注解的 `text` 消息；可选包含带注解的 `image`。
- `get-env` (tools/get-env.ts)：将运行进程中所有环境变量以格式化 JSON 文本返回。
- `get-resource-links` (tools/get-resource-links.ts)：返回引导性 `text` 块，后跟多个 `resource_link` 项。对请求的 `count`（1–10），使用 `resources/templates.ts` 中的 URI 在动态 Text 与 Blob 资源之间交替。
- `get-resource-reference` (tools/get-resource-reference.ts)：接受 `resourceType`（`text` 或 `blob`）和 `resourceId`（正整数）。返回具体的 `resource` 内容块（含 `uri`、`mimeType` 和数据）及说明性 `text`。
- `get-roots-list` (tools/get-roots-list.ts)：返回客户端发送的最近一次根目录列表。
- `gzip-file-as-resource` (tools/gzip-file-as-resource.ts)：接受 `name` 和 `data`（URL 或 data URI），在大小/时间/域名约束下抓取数据并压缩，在 `demo://resource/session/<name>` 注册为会话资源（`mimeType: application/gzip`），并根据 `outputType` 返回 `resource_link`（默认）或内联 `resource`。
- `get-structured-content` (tools/get-structured-content.ts)：演示结构化响应。接受 `location` 输入，同时返回向后兼容的 `content`（含 JSON 的 `text` 块）及由 `outputSchema` 校验的 `structuredContent`（温度、状况、湿度）。
- `get-sum` (tools/get-sum.ts)：对两个数 `a` 和 `b` 求和并返回结果。使用 Zod 校验输入。
- `get-tiny-image` (tools/get-tiny-image.ts)：以 `image` 内容项返回小型 PNG MCP 徽标，前后附带简短描述性 `text`。
- `trigger-long-running-operation` (tools/trigger-trigger-long-running-operation.ts)：在给定 `duration` 和 `steps` 数量下模拟多步操作；当客户端提供 `progressToken` 时通过 `notifications/progress` 报告进度。
- `toggle-simulated-logging` (tools/toggle-simulated-logging.ts)：为调用会话启动或停止模拟的、随机级别的日志。尊重客户端选择的最小日志级别。
- `toggle-subscriber-updates` (tools/toggle-subscriber-updates.ts)：为调用会话已订阅的 URI 启动或停止模拟资源更新通知。
- `trigger-sampling-request` (tools/trigger-sampling-request.ts)：使用提供的 `prompt` 及可选生成控制向客户端/LLM 发起 `sampling/createMessage` 请求；返回 LLM 的响应载荷。
- `simulate-research-query` (tools/simulate-research-query.ts)：通过模拟多阶段研究操作演示 MCP Tasks（SEP-1686）。接受 `topic` 和 `ambiguous` 参数。返回随阶段推进并带状态更新的任务。若 `ambiguous` 为 true 且客户端支持引导，则直接发送引导请求以在完成前收集澄清。
- `trigger-sampling-request-async` (tools/trigger-sampling-request-async.ts)：演示双向任务：服务器发送采样请求，客户端作为后台任务执行。服务器轮询状态并在完成后获取 LLM 结果。需要客户端支持 `tasks.requests.sampling.createMessage`。
- `trigger-elicitation-request-async` (tools/trigger-elicitation-request-async.ts)：演示双向任务：服务器发送引导请求，客户端作为后台任务执行。服务器在等待用户输入时轮询。需要客户端支持 `tasks.requests.elicitation.create`。

## 提示

- `simple-prompt` (prompts/simple.ts)：无参数提示，返回静态用户消息。
- `args-prompt` (prompts/args.ts)：双参数提示，`city`（必填）和 `state`（可选），用于组合问题。
- `completable-prompt` (prompts/completions.ts)：使用 SDK 的 `completable` 辅助函数演示参数自动补全；`department` 补全驱动上下文相关的 `name` 建议。
- `resource-prompt` (prompts/resource.ts)：接受 `resourceType`（"Text" 或 "Blob"）和 `resourceId`（可转为整数的字符串），返回包含通过 `resources/templates.ts` 生成的所选类型动态资源的消息。

## 资源

- 动态 Text：`demo://resource/dynamic/text/{index}`（按需生成内容）
- 动态 Blob：`demo://resource/dynamic/blob/{index}`（按需生成 base64 载荷）
- 静态文档：`demo://resource/static/document/<filename>`（将 `src/everything/docs/` 中的文件作为静态基于文件的资源提供）
- 会话作用域：`demo://resource/session/<name>`（动态注册的按会话资源；仅在会话存续期间可用）

## 资源订阅与通知

- 模拟更新通知为可选，默认关闭。
- 客户端可使用 MCP 的 `resources/subscribe` 和 `resources/unsubscribe` 请求订阅/取消订阅资源 URI。
- 使用 `toggle-subscriber-updates` 工具启动/停止按会话的间隔，仅为该会话已订阅的 URI 发出 `notifications/resources/updated { uri }`。
- 支持多个并发客户端；各客户端的订阅按会话跟踪，通知通过与该会话关联的服务器实例独立投递。

## 模拟日志

- 模拟日志可用，默认关闭。
- 使用 `toggle-simulated-logging` 工具按会话启动/停止不同级别（debug、info、notice、warning、error、critical、alert、emergency）的周期性日志消息。
- 客户端可通过标准 MCP `logging/setLevel` 请求控制接收的最小级别。

## 任务（SEP-1686）

服务器宣告支持 MCP Tasks，以支持带状态跟踪的长时间运行操作：

- **宣告的能力**：`tasks.list`、`tasks.cancel`、`tasks.requests.tools.call`
- **任务存储**：使用 SDK experimental 的 `InMemoryTaskStore` 管理任务生命周期
- **消息队列**：使用 `InMemoryTaskMessageQueue` 处理任务相关消息

### 任务生命周期

1. 客户端调用 `tools/call` 并带 `task: true` 参数
2. 服务器返回带 `taskId` 的 `CreateTaskResult`，而非立即结果
3. 客户端轮询 `tasks/get` 检查状态并接收 `statusMessage` 更新
4. 当状态为 `completed` 时，客户端调用 `tasks/result` 获取最终结果

### 任务状态

- `working`：任务正在处理
- `input_required`：任务需要额外输入（服务器直接发送引导请求）
- `completed`：任务成功完成
- `failed`：任务遇到错误
- `cancelled`：任务被客户端取消

### 演示工具

**服务端任务（客户端调用服务器）：**
使用 `simulate-research-query` 工具演练完整任务生命周期。设置 `ambiguous: true` 可触发引导——服务器将直接发送 `elicitation/create` 请求并在完成前等待响应。

**客户端任务（服务器调用客户端）：**
使用 `trigger-sampling-request-async` 或 `trigger-elicitation-request-async` 演示双向任务：服务器发送请求，客户端作为后台任务执行。这些需要客户端分别宣告 `tasks.requests.sampling.createMessage` 或 `tasks.requests.elicitation.create` 能力。

### 双向任务流程

MCP Tasks 是双向的——服务器和客户端均可作为任务执行方：

| 方向        | 请求类型             | 任务执行方 | 演示工具                           |
| ---------------- | ------------------------ | ------------- | ----------------------------------- |
| 客户端 → 服务器 | `tools/call`             | 服务器        | `simulate-research-query`           |
| 服务器 → 客户端 | `sampling/createMessage` | 客户端        | `trigger-sampling-request-async`    |
| 服务器 → 客户端 | `elicitation/create`     | 客户端        | `trigger-elicitation-request-async` |

对于客户端任务：

1. 服务器发送带任务元数据的请求（例如 `params.task.ttl`）
2. 客户端创建任务并返回带 `taskId` 的 `CreateTaskResult`
3. 服务器轮询 `tasks/get` 获取状态更新
4. 完成后，服务器调用 `tasks/result` 获取结果
