# 什么是Harness Engineering？

Harness Engineering是指围绕AI Agent（特别是 Coding Agent）设计和构建约束机制、反馈回路、工作流控制和持续改进循环的系统工程实践。它解决的核心问题是：当AI Agent拥有了强大的代码生成能力后，如何确保其输出的可靠性、一致性和长期可维护性。

## 三层工程概念的关系

Harness Engineering并不是凭空出现的，它是Prompt Engineering和Context Engineering的自然延伸，三者构成嵌套关系。

# 为什么需要Harness Engineering？

模型能力不是瓶颈

真正卡你的不是Agent写代码的能力，而是围绕它的结构、工具和反馈机制跟不上

# Harness Engineering的四大支柱

## 上下文架构（Context Architecture）

核心原则：Agent 应当恰好获得当前任务所需的上下文——不多不少

## Agent专业化（Agent Specialization）

核心原则：专注于特定领域、拥有受限工具的Agent优于拥有全部权限的通用Agent

## 持久化记忆（Persistent Memory）

核心原则：进度持久化在文件系统上，而非上下文窗口中。每次新Agent会话从零开始，通过文件系统制品重建上下文

## 结构化执行（Structured Execution）

核心原则：将思考与执行分离。研究和规划在受控阶段进行，执行基于验证过的计划，验证通过自动化反馈（测试、Linter、CI）和人类审查完成

所有团队都施加了刻意的执行序列：理解 → 规划 → 执行 → 验证

# 五大Harness原则

原则 1：设计环境，而非编写代码

原则 2：机械化地执行架构约束

原则 3：将代码仓库作为唯一事实源

原则 4：将可观测性连接到Agent

原则 5：对抗熵

自定义Linter的巧妙设计：当Agent违反架构约束时，错误消息不仅标记违规——还告诉Agent如何修复

# 参考链接

[Harness Engineering深度解析](https://zhuanlan.zhihu.com/p/2014014859164026634)
