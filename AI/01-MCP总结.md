# MCP协议是什么
模型上下文协议（Model Context Protocol）是一个开源协议，由Anthropic（Claud开发公司）2024年11月开发，旨在让大模型模型（LLM）能够以标准化的方式连接到外部数据源和工具。
## 标准化
MCP标准化了LLM访问外部数据的方式，简化了不同数据源和工具的集成
## 模块化
MCP促进了模块化设计，允许独立开发和维护不同组件
## 可扩展性
MCP使得添加新数据源或工具变得简单，无需大幅修改现有系统
## 安全性
MCP提供结构化的访问模式，内置验证，确保数据交互安全且受控
# MCP和Function Calling之间的区别
MCP是通用协议层的标准，类似于“AI领域的USB-C接口”，定义了LLM与外部工具/数据源的通信格式，但不绑定任何特定模型或厂商，将复杂的函数调用抽象为客户端-服务器架构。
Function Calling是大模型厂商提供的专有能力，由大模型厂商之间在接口定义和开发文档上存在差异；允许大模型直接生成调用函数，触发外部API，依赖大模型自身的上下文理解和结构化输出能力。
# MCP核心架构
MCP遵循客户端-服务器架构（client-server），其中包含以下几个核心概念：
- MCP主机（MCP Hosts）：发起请求的LLM应用程序（例如Claude Desktop、IDE或AI工具）
- MCP客户端（MCP Clients）：在主机程序内部，与MCP server保持1:1的连接
- 本地资源（Local Resources）：本地计算机中可供MCP server安全访问的资源（例如文件、数据库）
- 远程资源（Remote Resources）：MCP server可以连接到的远程资源（例如通过 API）
## MCP Client
MCP Client充当LLM和MCP server之间的桥梁，MCP client的工作流程如下：
- 1、MCP Client首先从MCP Server获取可用的工具列表
- 2、将用户的查询连同工具描述通过function calling一起发送给LLM
- 3、如果需要使用工具，MCP Client会通过MCP Server执行相应的工具调用
- 4、工具调用的结果会被发送回LLM，LLM基于所有信息生成自然语言响应。
- 5、最后将响应展示给用户
## MCP Server
MCP server是MCP架构中的关键组件，它可以提供3种主要类型的功能：
- 资源（Resources）：Resources是MCP里用来暴露数据的核心机制，相当于给LLM提供“原材料”
- 工具（Tools）：MCP使得添加新数据源或工具变得简单，无需大幅修改现有系统
- 提示（Prompts）：Prompts是MCP的“模板大师”，提供预定义的交互模式或者推理指引

可以在MCP Servers Repository和Awesome MCP Servers这两个repo中找到许多由社区实现的MCP server。使用TypeScript编写的MCP server可以通过npx命令来运行，使用Python编写的MCP server可以通过uvx命令来运行。
# 快速实现一个helloworld的MCP Server
安装mcp包扩展
pip install mcp
# 进阶概念二
## STDIO(标准输入/输出)
Stdio通过本地进程间通信实现，客户端以子进程形势启动服务器，双方通过stdin/stdio交换JSON-RPC消息，每条消息以换行符分隔
## SSE(Server-Sent Events)
SSE通过HTTP长连接实现远程通信，服务器端需提供两个端点：
- /sse (GET请求)：建立长连接，接收服务器推送的事件流。
- /messages (POST请求)：客户端发送请求至该端点。
# 参考链接
https://zhuanlan.zhihu.com/p/27327515233
