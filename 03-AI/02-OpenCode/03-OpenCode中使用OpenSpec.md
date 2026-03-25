# OpenSpec介绍

用OpenSpec对齐AI编程意图

AI写代码最崩溃的不是写不出来，而是写出来的和你想的完全不一样

OpenSpec vs SpecKit：两种SDD哲学

定义-->计划-->任务-->实施

核心工作流：三步闭环

提案(Proposal): 在投入编码前，开发者与AI共同明确需求，创建—份详细、无歧义的规范文档

执行(Apply): AI根据已批准的规范，有条不紊地分步生成代码、测试和文档

归档(Archive): 任务完成后，将此次变更的规范合并入项目的主规范库，使其成为—份随代码演进的“活文档”

扩展工作流：应对复杂场景

这种结合将开发过程从传统的“指令-响应”模式，提升为更高效的“监督-自主”模式

 LLMs is already smart enough — intelligence is not the bottleneck, context is

 从一次性生成到可持续演化

# 安装与初始化

## 安装OpenSpec

npm install -g @fission-ai/openspec@latest

## 验证安装

openspec --version

## 初始化项目

### 使用OpenCode初始化项目生成AGENTS.md

进入到项目页面打开OpenCode

输入命令/init即可初始化项目生成该项目的AGENTS.md文件，此时agent会扫描项目的结构规范等生成该文件，即为该项目的规则文件，需要添加的规则可在该文件下添加

如果觉得这个文件描述不够清晰，我们可以让agent借助skill-creator，按自己需要生成一个辅助生成项目AGENTS.md文件的skill，并使用该skill来辅助初始化AGENTS.md文件

提示词参考：

请分析这个代码库，并创建一个名为 AGENTS.md 的文件，通过AGENTS.md文档描述系统边界划分、模块层级设计、技术栈选型及编程规范要求，为需求分析与软件实现设计提供支撑；

如果已经存在一个AGENTS.md文件，并且该文件位于xxx目录下，请对其内容进行改进。

### 使用OpenSpec初始化项目

openspec init 或 openspec init-cn

# 开始使用OpenSpec做需求

## /opsx:explore（可选）

规划用的。进入探索模式——思考各种想法，研究问题，明确需求

## 首先明确需求，使用/opsx-new命令创建新的变更

这时候我们可以看到openspec/changes/目录下创建了对应变更名称的目录

## 接着ai准备创建proposal.md

proposal.md用来描述这个变更为什么做，要做什么，目标是什么

输入命令/opsx-continue继续

## 创建design.md

design.md主要是就是描述ai是如何对这个需求进行设计的

## 生成spec.md

## 生成tasks.md

task.md即描述AI即将要做哪些任务，后面执行时会根据tasks.md上的任务一步一步执行

## 用/opsx-apply命令执行

## 输入/opsx-archive执行变更的归档

# 参考链接

[先对齐意图再写代码！OpenSpec让AI编程不再翻车](https://www.bilibili.com/video/BV1hYwDzSE7A/?spm_id_from=333.337.search-card.all.click&vd_source=300730e0f86bed3fb2c80c0103709a12)

[从Copilot到工程化 Agent执行框架：基于OpenCode + OpenSpec的企业级 AI Coding落地实践](https://aicoding.csdn.net/6966226a6554f1331aa1b6c0.html)

[通过OpenSpec+OpenCode实践AI Specs](https://www.cnblogs.com/whuanle/p/19581835)

[拥抱AI编程，用中文进行规范驱动开发：OpenSpec汉化版正式发布](https://cloud.tencent.com/developer/article/2600274)
