# Model Context Protocol 服务器

本仓库汇集了 [Model Context Protocol](https://modelcontextprotocol.io/)（MCP）的*参考实现*，以及社区构建的服务器与相关资源的引用。

> [!重要]
> 若你在寻找 MCP 服务器列表，可在 [MCP Registry](https://registry.modelcontextprotocol.io/) 浏览已发布的服务器。本 README 所对应的仓库仅用于托管由 MCP 指导小组维护的少量参考服务器。

> [!警告]
> 本仓库中的服务器旨在作为**参考实现**，用于演示 MCP 特性与 SDK 用法。它们面向希望学习如何构建自有 MCP 服务器的开发者，而非可直接用于生产的方案。开发者应结合自身安全需求，并依据具体威胁模型与使用场景实施相应防护措施。

本仓库中的服务器展示了 MCP 的多样性与可扩展性，说明如何借助 MCP 为大型语言模型（LLM）提供对工具与数据源的安全、可控访问。
通常，每个 MCP 服务器都基于某个 MCP SDK 实现：

- [C# MCP SDK](https://github.com/modelcontextprotocol/csharp-sdk)
- [Go MCP SDK](https://github.com/modelcontextprotocol/go-sdk)
- [Java MCP SDK](https://github.com/modelcontextprotocol/java-sdk)
- [Kotlin MCP SDK](https://github.com/modelcontextprotocol/kotlin-sdk)
- [PHP MCP SDK](https://github.com/modelcontextprotocol/php-sdk)
- [Python MCP SDK](https://github.com/modelcontextprotocol/python-sdk)
- [Ruby MCP SDK](https://github.com/modelcontextprotocol/ruby-sdk)
- [Rust MCP SDK](https://github.com/modelcontextprotocol/rust-sdk)
- [Swift MCP SDK](https://github.com/modelcontextprotocol/swift-sdk)
- [TypeScript MCP SDK](https://github.com/modelcontextprotocol/typescript-sdk)

## 🌟 参考服务器

这些服务器旨在演示 MCP 特性及官方 SDK。

- **[Everything](src/everything)** - 包含 prompts、resources 与 tools 的参考/测试服务器。
- **[Fetch](src/fetch)** - 抓取网页内容并转换，便于 LLM 高效使用。
- **[Filesystem](src/filesystem)** - 带可配置访问控制的安全文件操作。
- **[Git](src/git)** - 用于读取、搜索与操作 Git 仓库的工具。
- **[Memory](src/memory)** - 基于知识图谱的持久化记忆系统。
- **[Sequential Thinking](src/sequentialthinking)** - 通过思维序列进行动态、反思式的问题求解。
- **[Time](src/time)** - 时间与时区转换能力。

### 已归档

以下参考服务器已归档，可在 [servers-archived](https://github.com/modelcontextprotocol/servers-archived) 查看。

- **[AWS KB Retrieval](https://github.com/modelcontextprotocol/servers-archived/tree/main/src/aws-kb-retrieval-server)** - 通过 Bedrock Agent Runtime 从 AWS Knowledge Base 检索。
- **[Brave Search](https://github.com/modelcontextprotocol/servers-archived/tree/main/src/brave-search)** - 使用 Brave Search API 进行网页与本地搜索。已由 [官方服务器](https://github.com/brave/brave-search-mcp-server) 替代。
- **[EverArt](https://github.com/modelcontextprotocol/servers-archived/tree/main/src/everart)** - 使用多种模型进行 AI 图像生成。
- **[GitHub](https://github.com/modelcontextprotocol/servers-archived/tree/main/src/github)** - 仓库管理、文件操作与 GitHub API 集成。
- **[GitLab](https://github.com/modelcontextprotocol/servers-archived/tree/main/src/gitlab)** - GitLab API，支持项目管理。
- **[Google Drive](https://github.com/modelcontextprotocol/servers-archived/tree/main/src/gdrive)** - Google Drive 的文件访问与搜索能力。
- **[Google Maps](https://github.com/modelcontextprotocol/servers-archived/tree/main/src/google-maps)** - 位置服务、路线与地点详情。
- **[PostgreSQL](https://github.com/modelcontextprotocol/servers-archived/tree/main/src/postgres)** - 只读数据库访问与 schema 检查。
- **[Puppeteer](https://github.com/modelcontextprotocol/servers-archived/tree/main/src/puppeteer)** - 浏览器自动化与网页抓取。
- **[Redis](https://github.com/modelcontextprotocol/servers-archived/tree/main/src/redis)** - 与 Redis 键值存储交互。
- **[Sentry](https://github.com/modelcontextprotocol/servers-archived/tree/main/src/sentry)** - 从 Sentry.io 获取并分析问题。
- **[Slack](https://github.com/modelcontextprotocol/servers-archived/tree/main/src/slack)** - 频道管理与消息能力。现由 [Zencoder](https://github.com/zencoderai/slack-mcp-server) 维护
- **[SQLite](https://github.com/modelcontextprotocol/servers-archived/tree/main/src/sqlite)** - 数据库交互与商业智能能力。

## 📚 框架

这些是便于构建 MCP 服务器或客户端的高层框架。

### 面向服务器

* **[Anubis MCP](https://github.com/zoedsoupe/anubis-mcp)** (Elixir) - 高性能、高层次的 Elixir Model Context Protocol（MCP）实现，可理解为 MCP 版的「Live View」。
* **[ModelFetch](https://github.com/phuctm97/modelfetch/)** (TypeScript) - 与运行时无关的 SDK，可在任何运行 TypeScript/JavaScript 的环境创建并部署 MCP 服务器
* **[EasyMCP](https://github.com/zcaceres/easy-mcp/)** (TypeScript)
* **[FastAPI to MCP auto generator](https://github.com/tadata-org/fastapi_mcp)** – 由 **[Tadata](https://tadata.com/)** 提供的零配置工具，自动将 FastAPI 端点暴露为 MCP 工具
* **[FastMCP](https://github.com/punkpeye/fastmcp)** (TypeScript)
* **[Foobara MCP Connector](https://github.com/foobara/mcp-connector)** - 通过 MCP 将用 Ruby 编写的 Foobara 命令轻松暴露为工具
* **[Foxy Contexts](https://github.com/strowk/foxy-contexts)** – 由 **[strowk](https://github.com/strowk)** 提供的 Golang MCP 服务器构建库
* **[Higress MCP Server Hosting](https://github.com/alibaba/higress/tree/main/plugins/wasm-go/mcp-servers)** - 通过 wasm 插件扩展 API 网关（基于 Envoy）以托管 MCP 服务器的方案。
* **[MCP Declarative Java SDK](https://github.com/codeboyzhou/mcp-declarative-java-sdk)** 基于注解的 Java MCP 服务器开发，无需 Spring Framework，尽可能减少依赖。
* **[MCP-Framework](https://mcp-framework.com)** 用 TypeScript 优雅、快速地构建 MCP 服务器。附带 CLI，可用 `mcp create app` 创建项目。由 **[Alex Andru](https://github.com/QuantGeekDev)** 提供，可在 5 分钟内启动首个服务器
* **[MCP Plexus](https://github.com/Super-I-Tech/mcp_plexus)**: 安全、**多租户**且多用户的 MCP Python 服务器框架，通过 OAuth 2.1 与外部服务轻松集成，为复杂 AI 应用提供可扩展、稳健的解决方案。
* **[mcp_sse (Elixir)](https://github.com/kEND/mcp_sse)** Elixir 中的 SSE 实现，用于快速创建 MCP 服务器。
* **[mxcp](https://github.com/raw-labs/mxcp)** (Python) - 开源框架，仅用 YAML、SQL 与 Python 即可构建企业级 MCP 服务器，内置认证、监控、ETL 与策略执行。
* **[Next.js MCP Server Template](https://github.com/vercel-labs/mcp-for-next.js)** (Typescript) - Next.js 入门项目，使用 MCP Adapter 让 MCP 客户端连接并访问资源。
* **[PayMCP](https://github.com/blustAI/paymcp)** (Python & TypeScript) - MCP 服务器的轻量支付层：用两行装饰器将工具变为付费端点。[PyPI](https://pypi.org/project/paymcp/) · [npm](https://www.npmjs.com/package/paymcp) · [TS 仓库](https://github.com/blustAI/paymcp-ts)
* **[Perl SDK](https://github.com/mojolicious/mojo-mcp)** - 使用 Perl 构建 MCP 服务器与客户端的 SDK。
* **[Quarkus MCP Server SDK](https://github.com/quarkiverse/quarkus-mcp-server)** (Java)
- **[R mcptools](https://github.com/posit-dev/mcptools)** - 用于创建基于 R 的 MCP 服务器，并将第三方 MCP 服务器功能作为 R 函数调用的 R SDK。
* **[SAP ABAP MCP Server SDK](https://github.com/abap-ai/mcp)** - 构建基于 SAP ABAP 的 MCP 服务器。基于 ABAP 7.52，7.02 向下兼容；运行于 R/3 与 S/4HANA 本地环境，目前尚不支持云部署。
* **[Spring AI MCP Server](https://docs.spring.io/spring-ai/reference/api/mcp/mcp-server-boot-starter-docs.html)** - 为 Spring Boot 应用配置 MCP 服务器提供自动配置。
* **[Template MCP Server](https://github.com/mcpdotdirect/template-mcp-server)** - CLI 工具，用于创建支持 TypeScript、双传输选项与可扩展结构的新 Model Context Protocol 服务器项目
* **[AgentR Universal MCP SDK](https://github.com/universal-mcp/universal-mcp)** - 由 **[Agentr](https://agentr.dev/home)** 提供的 Python SDK，用于构建带内置凭证管理的 MCP 服务器
* **[Vercel MCP Adapter](https://github.com/vercel/mcp-adapter)** (TypeScript) - 简单包，可在 Next、Nuxt、Svelte 等主流 JS 元框架上提供 MCP 服务器。
* **[PHP MCP Server](https://github.com/php-mcp/server)** (PHP) - Model Context Protocol（MCP）服务器的核心 PHP 实现

### 面向客户端

* **[codemirror-mcp](https://github.com/marimo-team/codemirror-mcp)** - CodeMirror 扩展，为资源提及与 prompt 命令实现 Model Context Protocol（MCP）
* **[llm-analysis-assistant](https://github.com/xuzexin-hz/llm-analysis-assistant)** <img height="12" width="12" src="https://raw.githubusercontent.com/xuzexin-hz/llm-analysis-assistant/refs/heads/main/src/llm_analysis_assistant/pages/html/imgs/favicon.ico" alt="Langfuse Logo" /> - 精简的 mcp 客户端，支持调用与监控 stdio/sse/streamableHttp，也可通过 /logs 页面查看请求响应；支持 ollama/openai 接口的监控与模拟。
* **[MCP-Agent](https://github.com/lastmile-ai/mcp-agent)** - 由 **[LastMile AI](https://www.lastmileai.dev)** 提供的简单、可组合的框架，用于基于 Model Context Protocol 构建智能体
* **[Spring AI MCP Client](https://docs.spring.io/spring-ai/reference/api/mcp/mcp-client-boot-starter-docs.html)** - 为 Spring Boot 应用中的 MCP 客户端功能提供自动配置。
* **[MCP CLI Client](https://github.com/vincent-pli/mcp-cli-host)** - CLI 宿主应用，使大型语言模型（LLM）通过 Model Context Protocol（MCP）与外部工具交互。
* **[OpenMCP Client](https://github.com/LSTM-Kirigaya/openmcp-client/)** - 面向 vscode/trae/cursor 的一体化 MCP 服务器调试插件。[文档](https://kirigaya.cn/openmcp/) & [OpenMCP SDK](https://kirigaya.cn/openmcp/sdk-tutorial/)。
* **[PHP MCP Client](https://github.com/php-mcp/client)** - Model Context Protocol（MCP）客户端的核心 PHP 实现
* **[Runbear](https://runbear.io/solutions/integrations/slack/mcp)** - 面向 Slack、Microsoft Teams、Discord 等团队聊天平台的无代码 MCP 客户端。

## 📚 资源

关于 MCP 的更多资源。

- **[A2A-MCP Java Bridge](https://github.com/vishalmysore/a2ajava)** - A2AJava 将强大的 A2A-MCP 集成直接带入 Java 应用。开发者可为标准 Java 方法添加注解，即可将其暴露为 MCP 服务器、A2A 可发现的操作——无需样板代码或服务注册开销。
- **[AiMCP](https://www.aimcp.info)** - 由 **[Hekmon](https://github.com/hekmon8)** 整理的 MCP 客户端与服务器集合，便于找到合适的 mcp 工具
- **[Awesome Crypto MCP Servers by badkk](https://github.com/badkk/awesome-crypto-mcp-servers)** - 由 **[Luke Fan](https://github.com/badkk)** 整理的 MCP 服务器精选列表
- **[Awesome MCP Servers by appcypher](https://github.com/appcypher/awesome-mcp-servers)** - 由 **[Stephen Akinyemi](https://github.com/appcypher)** 整理的 MCP 服务器精选列表
- **[Awesome MCP Servers by punkpeye](https://github.com/punkpeye/awesome-mcp-servers)** (**[网站](https://glama.ai/mcp/servers)**) - 由 **[Frank Fiegel](https://github.com/punkpeye)** 整理的 MCP 服务器精选列表
- **[Awesome MCP Servers by wong2](https://github.com/wong2/awesome-mcp-servers)** (**[网站](https://mcpservers.org)**) - 由 **[wong2](https://github.com/wong2)** 整理的 MCP 服务器精选列表
- **[Awesome Remote MCP Servers by JAW9C](https://github.com/jaw9c/awesome-remote-mcp-servers)** - 由 **[JAW9C](https://github.com/jaw9c)** 整理的**远程** MCP 服务器精选列表，含各服务的认证支持说明
- **[Discord Server](https://glama.ai/mcp/discord)** – 由 **[Frank Fiegel](https://github.com/punkpeye)** 运营的 MCP 社区 Discord 服务器
- **[Install This MCP](https://installthismcp.com)** - 通过精美的安装指南降低安装门槛
- <img height="12" width="12" src="https://raw.githubusercontent.com/klavis-ai/klavis/main/static/klavis-ai.png" alt="Klavis Logo" /> **[Klavis AI](https://www.klavis.ai)** - 开源 MCP 基础设施。在 Slack 与 Discord 上提供托管 MCP 服务器与 MCP 客户端。
- **[MCP Badges](https://github.com/mcpx-dev/mcp-badges)** – 由 **[Ironben](https://github.com/nanbingxyz)** 提供，用清晰醒目的徽章快速展示你的 MCP 项目
- <img height="12" width="12" src="https://mcpproxy.app/favicon.svg" alt="MCPProxy Logo" /> **[MCPProxy](https://github.com/smart-mcp-proxy/mcpproxy-go)** - 开源本地应用，通过 MCP 协议智能发现多个 MCP 服务器与数千种工具，在隔离环境中运行服务器，并对恶意工具提供自动隔离防护。
- **[MCPRepository.com](https://mcprepository.com/)** - 索引并整理所有 MCP 服务器，便于发现的仓库。
- **[mcp-cli](https://github.com/wong2/mcp-cli)** - 由 **[wong2](https://github.com/wong2)** 提供的 Model Context Protocol CLI 检查工具
- **[mcp-dockmaster](https://mcp-dockmaster.com)** - 开源 UI，用于在 Windows、Linux 与 macOS 上安装并管理 MCP 服务器。
- **[mcp-get](https://mcp-get.com)** - 由 **[Michael Latman](https://github.com/michaellatman)** 提供的命令行工具，用于安装与管理 MCP 服务器
- **[mcp-guardian](https://github.com/eqtylab/mcp-guardian)** - 由 **[EQTY Lab](https://eqtylab.io)** 提供的 GUI 应用与工具，用于代理/管理 MCP 服务器的控制
- **[MCP Linker](https://github.com/milisp/mcp-linker)** - 跨平台 Tauri GUI 工具，一键配置与管理 MCP 服务器，支持 Claude Desktop、Cursor、Windsurf、VS Code、Cline 与 Neovim。
- **[mcp-manager](https://github.com/zueai/mcp-manager)** - 由 **[Zue](https://github.com/zueai)** 提供的简单 Web UI，为 Claude Desktop 安装并管理 MCP 服务器
- **[MCP Marketplace Web Plugin](https://github.com/AI-Agent-Hub/mcp-marketplace)** MCP Marketplace 是与 AI 应用集成的小型 Web UX 插件，支持多种 MCP 服务器 API 端点（如 pulsemcp.com、deepnlp.org 等）。用户可按类别浏览、分页并选择各类 MCP 服务器。[Pypi](https://pypi.org/project/mcp-marketplace) | [维护者](https://github.com/AI-Agent-Hub) | [网站](http://www.deepnlp.org/store/ai-agent/mcp-server)
- **[mcp.natoma.ai](https://mcp.natoma.ai)** – 由 **[Natoma Labs](https://www.natoma.ai)** 提供的托管 MCP 平台，用于发现、安装、管理与部署 MCP 服务器
- **[mcp.run](https://mcp.run)** - 托管注册表与控制平面，用于安装并运行安全、可移植的 MCP 服务器。
- **[MCPHub](https://www.mcphub.com)** - 列出高质量 MCP 服务器及真实用户评价的网站，并为常用 LLM 模型提供带 MCP 服务器支持的在线聊天机器人。
- **[MCP Router](https://mcp-router.net)** – 由 **[MCP Router](https://github.com/mcp-router/mcp-router)** 提供的免费 Windows 与 macOS 应用，简化 MCP 管理，并提供无缝应用认证与强大的日志可视化
- **[MCP Servers Hub](https://github.com/apappascs/mcp-servers-hub)** (**[网站](https://mcp-servers-hub-website.pages.dev/)**) - 由 **[apappascs](https://github.com/apappascs)** 整理的 MCP 服务器精选列表
- **[MCPServers.com](https://mcpservers.com)** - 不断扩充的高质量 MCP 服务器目录，为多种 MCP 客户端提供清晰设置指南。由 **[Highlight MCP client](https://highlightai.com/)** 团队打造
- **[MCP Servers Rating and User Reviews](http://www.deepnlp.org/store/ai-agent/mcp-server)** - 为 MCP 服务器评分、撰写真实用户评价的网站，并提供 [agent 与 mcp 搜索引擎](http://www.deepnlp.org/search/agent)
- **[MCP Sky](https://bsky.app/profile/brianell.in/feed/mcp)** - 由 **[@brianell.in](https://bsky.app/profile/brianell.in)** 提供的 MCP 相关新闻与讨论的 Bluesky 信息流
- **[MCP X Community](https://x.com/i/communities/1861891349609603310)** – 由 **[Xiaoyi](https://x.com/chxy)** 运营的 MCP X 社区
- **[MCPHub](https://github.com/Jeamee/MCPHub-Desktop)** – 由 **[Jeamee](https://github.com/jeamee)** 提供的开源 macOS 与 Windows GUI 桌面应用，用于发现、安装与管理 MCP 服务器
- **[mcpm](https://github.com/pathintegral-institute/mcpm.sh)** ([网站](https://mcpm.sh)) - MCP Manager（MCPM）是类 Homebrew 的服务，由 **[Pathintegral](https://github.com/pathintegral-institute)** 提供，用于跨客户端管理 Model Context Protocol（MCP）服务器
- **[MCPVerse](https://mcpverse.dev)** - 用于创建与托管经认证的 MCP 服务器并安全连接的门户。
- **[MCP Servers Search](https://github.com/atonomus/mcp-servers-search)** - 提供查询与发现本列表中可用 MCP 服务器工具的 MCP 服务器。
- **[Search MCP Server](https://github.com/krzysztofkucmierz/search-mcp-server)** - 通过搜索本 README 文件，根据客户端查询推荐最相关的 MCP 服务器。
- **[MCPWatch](https://github.com/kapilduraphe/mcp-watch)** - 面向 Model Context Protocol（MCP）服务器的全面安全扫描器，可检测 MCP 服务器实现中的漏洞与安全问题。
- <img height="12" width="12" src="https://mkinf.io/favicon-lilac.png" alt="mkinf Logo" /> **[mkinf](https://mkinf.io)** - 开源托管 MCP 服务器注册表，加速 AI 智能体工作流。
- **[Open-Sourced MCP Servers Directory](https://github.com/chatmcp/mcp-directory)** - 由 **[mcpso](https://mcp.so)** 整理的 MCP 服务器精选列表
- <img height="12" width="12" src="https://opentools.com/favicon.ico" alt="OpenTools Logo" /> **[OpenTools](https://opentools.com)** - 由 **[opentoolsteam](https://github.com/opentoolsteam)** 提供的开放注册表，用于查找、安装 MCP 服务器并基于其构建
- **[Programmatic MCP Prototype](https://github.com/domdomegg/programmatic-mcp-prototype)** - 由 **[Adam Jones](https://github.com/domdomegg)** 提供的实验性智能体原型，演示程序化 MCP 工具组合、渐进式工具发现、状态持久化，以及通过 TypeScript 代码执行构建技能
- **[PulseMCP](https://www.pulsemcp.com)** ([API](https://www.pulsemcp.com/api)) - 由 **[Tadas Antanavicius](https://github.com/tadasant)**、**[Mike Coughlin](https://github.com/macoughl)** 与 **[Ravina Patel](https://github.com/ravinahp)** 运营的社区中心与周刊，用于发现 MCP 服务器、客户端、文章与新闻
- **[r/mcp](https://www.reddit.com/r/mcp)** – 由 **[Frank Fiegel](https://github.com/punkpeye)** 运营的 MCP Reddit 社区
- **[MCP.ing](https://mcp.ing/)** - 由 **[iiiusky](https://github.com/iiiusky)** 提供的 MCP 服务列表，用于在社区中发现 MCP 服务器，并为 MCP 服务提供便捷搜索
- **[MCP Hunt](https://mcp-hunt.com)** - 实时平台，用于发现热门 MCP 服务器，含热度追踪、投票与社区讨论——类似 MCP 版的 Product Hunt 遇上 Reddit
- **[Smithery](https://smithery.ai/)** - 由 **[Henry Mao](https://github.com/calclavia)** 提供的 MCP 服务器注册表，为 LLM 智能体找到合适工具
- **[Toolbase](https://gettoolbase.ai)** - 由 **[gching](https://github.com/gching)** 提供的桌面应用，几次点击即可管理工具与 MCP 服务器——无需编码
- **[ToolHive](https://github.com/StacklokLabs/toolhive)** - 由 **[StacklokLabs](https://github.com/StacklokLabs)** 提供的轻量工具，通过容器化简化 MCP 服务器的部署与管理，确保易用、一致与安全
- **[NetMind](https://www.netmind.ai/AIServices)** - 通过简单 API 或 MCP 服务器访问强大 AI 服务，提升生产力。
- **[Webrix MCP Gateway](https://github.com/webrix-ai/secure-mcp-gateway)** - 企业级 MCP 网关，提供 SSO、RBAC、审计追踪与令牌保险库，实现安全、集中的 AI 智能体访问控制。可通过 Helm chart 本地或云端部署。[webrix.ai](https://webrix.ai)



## 🚀 入门

### 使用本仓库中的 MCP 服务器
本仓库中基于 TypeScript 的服务器可直接通过 `npx` 使用。

例如，以下命令将启动 [Memory](src/memory) 服务器：
```sh
npx -y @modelcontextprotocol/server-memory
```

本仓库中基于 Python 的服务器可直接通过 [`uvx`](https://docs.astral.sh/uv/concepts/tools/) 或 [`pip`](https://pypi.org/project/pip/) 使用。为便于安装与使用，推荐使用 `uvx`。

例如，以下命令将启动 [Git](src/git) 服务器：
```sh
# With uvx
uvx mcp-server-git

# With pip
pip install mcp-server-git
python -m mcp_server_git
```

请按[此处](https://docs.astral.sh/uv/getting-started/installation/)说明安装 `uv` / `uvx`，按[此处](https://pip.pypa.io/en/stable/installation/)说明安装 `pip`。

### 使用 MCP 客户端
单独运行服务器用途有限，应将其配置到 MCP 客户端中。例如，以下 Claude Desktop 配置可使用上述服务器：

```json
{
  "mcpServers": {
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"]
    }
  }
}
```

在 Windows 上，需用 `cmd /c` 包装 `npx`：

```json
{
  "mcpServers": {
    "memory": {
      "command": "cmd",
      "args": ["/c", "npx", "-y", "@modelcontextprotocol/server-memory"]
    }
  }
}
```

更多将 Claude Desktop 用作 MCP 客户端的示例如下：

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/allowed/files"]
    },
    "git": {
      "command": "uvx",
      "args": ["mcp-server-git", "--repository", "path/to/git/repo"]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "<YOUR_TOKEN>"
      }
    },
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres", "postgresql://localhost/mydb"]
    }
  }
}
```

在 Windows 上，对上述每个基于 `npx` 的条目，将 `"command"` 改为 `"cmd"`，并在现有 `args` 前加上 `"/c", "npx"`。基于 `uvx` 的条目保持不变。

## 🛠️ 创建你自己的服务器

想创建自己的 MCP 服务器？请访问官方文档 [modelcontextprotocol.io](https://modelcontextprotocol.io/introduction)，获取实现 MCP 服务器的完整指南、最佳实践与技术细节。

## 🤝 贡献

有关如何向本仓库贡献，请参阅 [CONTRIBUTING.md](CONTRIBUTING.md)。

## 🔒 安全

有关报告安全漏洞，请参阅 [SECURITY.md](SECURITY.md)。

## 📜 许可证

本项目对新贡献采用 Apache License, Version 2.0，既有代码采用 MIT——详见 [LICENSE](LICENSE) 文件。

## 💬 社区

- [GitHub 讨论](https://github.com/orgs/modelcontextprotocol/discussions)

## ⭐ 支持

若你认为 MCP 服务器有用，欢迎为仓库点 Star，并贡献新服务器或改进！

---

由 Anthropic 管理，但与社区共同构建。Model Context Protocol 是开源的，我们鼓励每个人贡献自己的服务器与改进！
