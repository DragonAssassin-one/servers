# Everything 服务器 – 架构

**架构
| [项目结构](structure.md)
| [启动流程](startup.md)
| [服务器功能](features.md)
| [扩展点](extension.md)
| [工作原理](how-it-works.md)**

本文档概括 `src/everything` 包的当前布局与运行时架构。
说明服务器如何启动、传输如何接入、工具/提示/资源在何处注册，以及如何扩展系统。

## 高层概览

### 目的

一个最小化、模块化的 MCP 服务器，用于展示核心 Model Context Protocol 功能。它暴露简单的工具、提示和资源，并可通过多种传输（STDIO、SSE 和 Streamable HTTP）运行。

### 设计

小型「服务器工厂」构造 MCP 服务器并注册功能。
传输是独立的入口点，负责创建/连接服务器并处理网络相关事项。
工具、提示和资源各自组织在独立子模块中。

### 多客户端

服务器支持多个并发客户端。通过资源订阅和模拟日志演示按会话跟踪数据。

## 构建与分发

- TypeScript 源码通过 `npm run build` 编译到 `dist/`。
- `build` 脚本将 `docs/` 复制到 `dist/`，使说明文件与编译后的服务器一并分发。
- CLI 可执行文件在 `package.json` 中配置为 `mcp-server-everything` → `dist/index.js`。

## [项目结构](structure.md)

## [启动流程](startup.md)

## [服务器功能](features.md)

## [扩展点](extension.md)

## [工作原理](how-it-works.md)
