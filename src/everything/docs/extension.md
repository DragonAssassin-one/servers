# Everything 服务器 - 扩展点

**[架构](architecture.md)
| [项目结构](structure.md)
| [启动流程](startup.md)
| [服务器功能](features.md)
| 扩展点
| [工作原理](how-it-works.md)**

## 添加工具

- 在 `tools/` 下新建文件，实现 `registerXTool(server)` 函数，通过 `server.registerTool(...)` 注册工具。
- 在 `tools/index.ts` 的 `registerTools(server)` 中导出并调用该函数。

## 添加提示

- 在 `prompts/` 下新建文件，实现 `registerXPrompt(server)` 函数，通过 `server.registerPrompt(...)` 注册提示。
- 在 `prompts/index.ts` 的 `registerPrompts(server)` 中导出并调用该函数。

## 添加资源

- 在 `resources/` 下新建文件，实现 `registerXResources(server)` 函数，使用 `server.registerResource(...)`（可选配合 `ResourceTemplate`）。
- 在 `resources/index.ts` 的 `registerResources(server)` 中导出并调用该函数。
