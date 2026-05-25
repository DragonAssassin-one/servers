# Everything 服务器 - 工作原理

**[架构](architecture.md)
| [项目结构](structure.md)
| [启动流程](startup.md)
| [服务器功能](features.md)
| [扩展点](extension.md)
| 工作原理**

# 条件式工具注册

### 模块：`server/index.ts`

- 部分工具需要客户端支持其所演示的能力。这些工具为：
  - `get-roots-list`
  - `trigger-elicitation-request`
  - `trigger-sampling-request`
- 客户端能力在初始化握手完成前是未知的。
- 大多数工具在服务器工厂执行期间、客户端连接之前立即注册。
- 为在已知客户端能力后再注册这些命令，从 `onintitialized` 处理器中调用 `registerConditionalTools(server)`。

## 资源订阅

### 模块：`resources/subscriptions.ts`

- 按 URI 跟踪订阅者：`Map<uri, Set<sessionId>>`。
- 通过 `setSubscriptionHandlers(server)` 安装处理器，处理订阅/取消订阅请求并维护映射。
- 由 `toggle-subscriber-updates` 工具按需启动/停止更新，该工具调用 `beginSimulatedResourceUpdates(server, sessionId)` 和 `stopSimulatedResourceUpdates(sessionId)`。
- `cleanup(sessionId?)` 调用 `stopSimulatedResourceUpdates(sessionId)` 以清除间隔并移除会话作用域状态。

## 会话作用域资源

### 模块：`resources/session.ts`

- `getSessionResourceURI(name: string)`：构建会话资源 URI：`demo://resource/session/<name>`。
- `registerSessionResource(server, resource, type, payload)`：以给定的 `uri`、`name` 和 `mimeType` 注册资源，并返回 `resource_link`。内容仅在会话存续期间从内存提供。支持 `type: "text" | "blob"`，并在对应字段返回数据。
- 预期用法：工具可创建并暴露按会话的制品，而无需持久化。例如 `tools/gzip-file-as-resource.ts` 压缩抓取的内容，以 `mimeType: application/gzip` 注册为会话资源，并根据 `outputType` 返回 `resource_link` 或内联 `resource`。

## 模拟日志

### 模块：`server/logging.ts`

- 定期以不同级别发送随机日志消息。消息可包含会话 ID，便于演示时区分。
- 通过 `toggle-simulated-logging` 工具按需启动/停止，该工具调用 `beginSimulatedLogging(server, sessionId?)` 和 `stopSimulatedLogging(sessionId?)`。注意传输断开时会触发 `cleanup()`，也会停止所有活动间隔。
- 使用 `server.sendLoggingMessage({ level, data }, sessionId?)`，以便 SDK 尊重客户端配置的最小日志级别。
