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

# 安装与初始化

## 安装OpenSpec

npm install -g @fission-ai/openspec@latest

## 验证安装

openspec --version

## 初始化项目

openspec init

OpenCode CLI下执行/init

## 相关命令使用

### /opsx:explore

规划用的。进入探索模式——思考各种想法，研究问题，明确需求

# 参考链接

[先对齐意图再写代码！OpenSpec让AI编程不再翻车](https://www.bilibili.com/video/BV1hYwDzSE7A/?spm_id_from=333.337.search-card.all.click&vd_source=300730e0f86bed3fb2c80c0103709a12)

[从Copilot到工程化 Agent执行框架：基于OpenCode + OpenSpec的企业级 AI Coding落地实践](https://aicoding.csdn.net/6966226a6554f1331aa1b6c0.html)

[通过OpenSpec+OpenCode实践AI Specs](https://www.cnblogs.com/whuanle/p/19581835)
