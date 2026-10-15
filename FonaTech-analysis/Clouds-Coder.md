# Clouds-Coder：项目与 Harness 技术分析

## 一、项目定位

Clouds-Coder 是一个本地优先的通用任务智能体运行时：把 Web/IDE 交互面与 CLI 执行面分离，由模型规划任务、调用受治理的工具、在工作目录中执行操作，并把过程事件、文件变更、工件和诊断回传到浏览器。它不仅服务编码，也通过 skills、RAG、MCP 和协作模块承载研究、文档和多步骤自动化。

源码快照的主要实现集中在 `Code_Structure/`，`Clouds_Coder.py` 是兼容入口/打包形态，`split_coder.py` 负责把单文件形态拆成可维护的模块。

## 二、Harness 究竟做了什么

这里的 harness 不是单一提示词，而是包住模型调用的“任务执行控制平面”：

1. **启动与配置**：`config/bootstrap.py`、`config/settings.py`、`llm/constants.py` 解析命令行、环境变量和 LLM profile，统一 OpenAI-compatible、Anthropic、Ollama、vLLM、LM Studio 等端点；`app/context.py` 组装全局服务。
2. **会话与状态**：`session/manager.py`、`session/state.py` 管理项目、会话、消息、上下文预算、压缩/恢复、工件和检查点。会话不是一次 HTTP 请求，而是可持续、可恢复的运行时对象。
3. **模型-工具循环**：`agent/tools.py` 定义工具 schema 和参数规范；`llm/client.py` 负责流式模型请求、工具调用解析和兼容性修复；`agent/process.py` 执行命令/子进程；循环根据模型输出决定继续思考、调用工具、提交结果或恢复。
4. **安全执行边界**：`ide/sandbox.py`、进程管理和 worktree 模块限制工作目录、shell、超时、输出大小和后台任务；文件读写、命令、预览、媒体等能力经过工具层而不是让模型直接获得宿主机 API。
5. **可观测回传**：`agent/events.py`、`agent/bus.py`、`server/http.py` 和 IDE handler 将 token、工具开始/结束、文件差异、诊断、todo 和状态通过流式事件发送到 Web 面板；UI 可以显示阶段进度而不必等待整轮请求结束。
6. **失败治理**：代码中有超时模式、不完整 HTTP read 处理、截断/预算控制、thinking-only recovery、动态 agent loop 和 circuit breaker 测试，用于处理模型空转、流被截断、工具参数不完整、上下文过长和重复失败。

## 三、核心能力矩阵

| 能力 | 代码落点 | 作用 |
| --- | --- | --- |
| 编码执行 | `agent/process.py`, `ide/sandbox.py` | 在受控工作目录运行 shell/脚本，捕获 stdout/stderr、退出码和超时 |
| 文件与工件 | `utils/files.py`, `ide/handler.py`, `ide/preview.py` | 读写、差异、预览、媒体和下载，向 UI 展示可审阅结果 |
| 计划与进度 | `agent/todo.py`, `agent/tasks.py`, session state | 将长任务拆分为 todo/任务节点，记录状态并支持恢复 |
| 多代理协作 | `collaboration/core.py`, `watcher.py`, `agent/background.py`, `worktree.py` | 成员、代理、黑板任务、证据、消息和隔离工作树；支持后台代理与协作状态治理 |
| Skills | `skills/store.py`, `provisioning.py`, `studio.py` | 内置/上传技能发现、按需注入、审核和 Skills Studio 编辑，而不是把全部知识永久塞入上下文 |
| RAG/搜索 | `rag/ingestion.py`, `parsers.py`, `index.py`, `store.py`, `web_search.py` | 解析本地资料、建立索引、检索证据和受控公共网页发现 |
| MCP | `mcp/driver.py`, `service.py` | 读取 MCP 配置、发现外部工具/资源并纳入会话服务边界 |
| 管理与认证 | `admin/auth.py`, `admin/config.py`, collaboration store | 管理配置、设备/会话、密码哈希、CSRF/TTL、项目成员和撤销 |
| IDE 工作区 | `ide/handler.py`, `server/http.py`, `web/assets.py` | 浏览器编辑器、代码历史、Agent 轨迹、诊断、预览和资源操作 |

## 四、典型执行时序

用户提交目标后，HTTP/IDE 层创建或恢复 session；运行时把系统约束、项目上下文、技能入口和历史摘要交给 LLM。模型返回文本或结构化 tool call，工具层校验名称/参数，sandbox/process 层执行；结果被截断、编码和标记后写入 session/event bus，再流式推到 UI。模型根据结果继续调用、更新 todo、请求用户输入或结束。上下文接近预算时触发摘要/压缩；进程异常、网络截断或思考空转则走重试、恢复或 circuit breaker。

因此 harness 的价值在于“闭环”：模型只负责提出下一步意图，运行时负责权限、执行、状态、反馈、预算、恢复和最终可审阅性。

## 五、工程判断与边界

- 它是通用 agent runtime，不是沙箱虚拟机；实际隔离强度取决于宿主操作系统、sandbox 后端、工作目录和部署配置。
- 多代理协作有数据库黑板、租约、冲突和证据机制，但它不等于自动解决所有代码合并冲突，仍需要项目级验证。
- RAG 和 Web search 提供检索通道与溯源结构，不保证外部来源正确；模型仍可能误解证据。
- `Clouds_Coder.py` 与拆分包并存，`split_coder.py` 的自检负责验证生成模块和单文件形态的一致性，说明项目重视可打包分发和源码可维护性。
- 测试目录覆盖动态 agent loop、上下文自动压缩、工具历史兼容、MCP workspace trust、shell timeout、IDE v2、skills studio、协作和恢复等关键控制面；这比单纯的聊天 UI 更接近生产 harness，但不构成对任意环境的安全认证。

## 六、结论

Clouds-Coder 的核心不是“让模型写代码”的前端，而是一套把 LLM 变成可管理执行者的运行时：工具契约把能力结构化，sandbox/进程层把执行约束化，session/event 层把过程持久化和可观测化，skills/RAG/MCP 扩展知识与工具，todo/上下文压缩/恢复机制让长任务能够收敛。它因此具备编码、研究、文档和多代理任务的共同底座；最终可靠性仍取决于部署隔离、模型质量、权限配置和人工验收。
