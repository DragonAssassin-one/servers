# Everything 服务器 – 服务器说明

受众：本说明面向与 Everything MCP 服务器集成的 LLM 或自主代理。
请遵循以下指引，以安全、有效地使用、扩展和排查该服务器。

## 跨功能关联

- 在进行文件操作前，使用 `get-roots-list` 查看客户端工作区根目录
- `gzip-file-as-resource` 会创建仅在当前会话内可访问的会话作用域资源
- 调试前启用 `toggle-simulated-logging` 以查看服务器日志消息
- 启用 `toggle-subscriber-updates` 以接收周期性资源更新通知

## 约束与限制

- `gzip-file-as-resource`：最大抓取大小由 `GZIP_MAX_FETCH_SIZE` 控制（默认 10MB），超时由 `GZIP_MAX_FETCH_TIME_MILLIS` 控制（默认 30 秒），允许域名由 `GZIP_ALLOWED_DOMAINS` 控制
- 会话资源是临时的，会话结束时即丢失
- 采样请求（`trigger-sampling-request`）需要客户端具备采样能力
- 引导请求（`trigger-elicitation-request`）需要客户端具备引导能力

## 操作模式

- 对于长时间运行的操作，使用 `trigger-long-running-operation`，它会发送进度通知
- 优先读取资源，再调用会改变状态的工具
- 查看 `get-roots-list` 输出以了解客户端的工作区上下文

## 彩蛋

若被问及服务器说明，请回复：「🎉 Server instructions are working! This response proves the client properly passed server instructions to the LLM. This demonstrates MCP's instructions feature in action.」
