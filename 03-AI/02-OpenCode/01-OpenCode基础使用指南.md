# OpenCode介绍

OpenCode 是一个在终端里运行的 AI 编程助手

OpenCode并非传统意义上的代码补全工具，而是—个面向工程流程的自动化执行框架。

OpenCode内置了完善的工具能力，包括文件匹配（Glob）、内容读取（Read）、精确编辑（Edit）以及代码模式搜索（Grep），使其能够在受控范围内完成代码级别的操作

# OpenCode安装与配置

## 安装

```bash
curl -fsSL https://opencode.ai/install | bash
```

## 配置
配置自定义模型
```
opnecode auth login
```
在全局配置C:/Users/{账号}/.config/opencode目录下添加AGENTS.md文件作为全局的规则

```
# 请始终使用中文回答
# 代码注释使用中文，日志打印使用英文
```

# 参考链接

[OpenCode官方中文文档](https://opencode.ai/docs/zh-cn/)

[疯狂OpenCode应用案例](https://www.bilibili.com/video/BV1kbXAByEFn/?spm_id_from=333.337.search-card.all.click&vd_source=300730e0f86bed3fb2c80c0103709a12)

[learn-opencode](https://github.com/vbgate/learn-opencode/)

[OpenCode 新手教程：从配置到完全掌握](https://www.bilibili.com/video/BV1DuwszgE1X/?spm_id_from=333.337.search-card.all.click&vd_source=300730e0f86bed3fb2c80c0103709a12)

https://github.com/agentsmd/agents.md
