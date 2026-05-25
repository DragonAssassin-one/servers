# 参与贡献 MCP Servers

感谢你有兴趣参与贡献！以下说明如何帮助改进本仓库。

我们接受通过 [GitHub 标准流程](https://docs.github.com/zh/get-started/using-github/github-flow) 提交的变更。

## 服务器列表

README 已不再包含第三方 MCP 服务器列表 — 该列表已退役，改为使用 [MCP Server Registry](https://github.com/modelcontextprotocol/registry)。若要让服务器可被发现，请按 [快速入门指南](https://github.com/modelcontextprotocol/registry/blob/main/docs/modelcontextprotocol-io/quickstart.mdx) 发布到 Registry。

可在 [https://registry.modelcontextprotocol.io/](https://registry.modelcontextprotocol.io/) 浏览已发布的服务器。

## 服务器实现

我们欢迎：

- **Bug 修复** — 帮助我们消灭 bug。
- **可用性改进** — 让人类与 agent 更容易使用服务器。
- **展示 MCP 协议特性的增强** — 鼓励贡献能帮助参考服务器更好说明 MCP 中较少被使用的部分（不仅是 Tools，还有 Resources、Prompts、Roots 等）。例如为 filesystem-server 增加 Roots 支持，有助于展示这一重要但较少人了解的特性。

我们更有选择性地接受：

- **其他新功能** — 尤其若与服务器核心目的关系不大或高度主观。现有服务器是供社区参考的示例。若你需要特定功能，建议在 [MCP Server Registry](https://github.com/modelcontextprotocol/registry) 发布增强版本！我们认为多样化的服务器生态对所有人都有益。

我们不接受：

- **新的服务器实现** — 请改在 [MCP Server Registry](https://github.com/modelcontextprotocol/registry) 发布。

## 测试

为 TypeScript 实现的服务器添加或配置测试时，请使用 **vitest**。Vitest 对 ESM 支持更好、执行更快，测试体验更现代。

## 文档

欢迎改进现有文档 — 不过我们更希望优先做体验上的改进，而不是只记录痛点（若可能的话）。

对全新文档我们更谨慎，尤其是不具厂商中立性的内容（例如如何在特定客户端上运行某个服务器）。

## 社区

[了解 MCP 社区如何交流](https://modelcontextprotocol.io/community/communication)。

感谢你帮助让 MCP 服务器对所有人更好用！
