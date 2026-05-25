# MCP "Everything" 服务器 - 开发指南

## 构建、测试与运行命令

- 构建：`npm run build` - 将 TypeScript 编译为 JavaScript
- 监视模式：`npm run watch` - 监视变更并自动重新构建
- 运行 STDIO 服务器：`npm run start:stdio` - 使用 stdio 传输启动 MCP 服务器
- 运行 SSE 服务器：`npm run start:sse` - 使用 SSE 传输启动 MCP 服务器
- 运行 StreamableHttp 服务器：`npm run start:streamableHttp` - 使用 StreamableHttp 传输启动 MCP 服务器
- 准备发布：`npm run prepare` - 为发布构建项目

## 代码风格指南

- 在 import 路径中使用 ES 模块及 `.js` 扩展名
- 使用 TypeScript 为所有函数和变量提供严格类型
- 工具入参校验遵循 zod schema 模式
- 优先使用 async/await，而非回调或 Promise 链
- 将所有 import 放在文件顶部，按外部依赖、内部模块分组
- 使用能清楚表明用途的描述性变量名
- 在服务器关闭时正确清理定时器和资源
- 使用 try/catch 处理错误，并提供清晰的错误消息
- 使用一致的缩进（2 空格）及多行对象尾随逗号
- 与各自文件夹中现有的代码风格、import 顺序和模块布局保持一致
- 变量/函数使用 camelCase
- 类型/类使用 PascalCase
- 常量使用 UPPER_CASE
- 文件名及注册的工具、prompts、resources 使用 kebab-case
- 工具名以动词开头，例如 `get-annotated-message` 而非 `annotated-message`

## 扩展服务器

Everything 服务器设计为可在明确定义的扩展点上进行扩展。
参见 [扩展点](docs/extension.md) 与 [项目结构](docs/structure.md)。
服务器工厂位于 `src/everything/server/index.ts`，在启动时注册所有功能，并处理连接后的设置。

### 概览

- 工具位于 `src/everything/tools/`，通过 `registerTools(server)` 注册。
- 资源位于 `src/everything/resources/`，通过 `registerResources(server)` 注册。
- 提示位于 `src/everything/prompts/`，通过 `registerPrompts(server)` 注册。
- 订阅与模拟更新例程位于 `src/everything/resources/subscriptions.ts`。
- 日志辅助函数位于 `src/everything/server/logging.ts`。
- 传输管理器位于 `src/everything/transports/`。

### 添加新功能时

- 遵循其文件夹中现有的文件/模块模式（命名、导出和注册函数）。
- 导出 `registerX(server)` 函数，以与现有模块相同的方式向 MCP SDK 注册新项。
- 将新模块接入中央索引（例如更新 `tools/index.ts`、`resources/index.ts` 或 `prompts/index.ts`）。
- 确保（工具的）schema 为准确的 JSON Schema，并包含有用的描述和示例。
  `server/index.ts` 以及 `logging.ts` 和 `subscriptions.ts` 中的用法。
- 若添加或修改值得注意的功能，请保持 `src/everything/docs/` 中的文档为最新。
