# Skill介绍

Skill 是一套**按需加载**的专业知识包，通过 SKILL.md 定义，Agent 根据任务语义自动发现并加载。

## 核心设计理念

```
┌─────────────────────────────────────────────────────────┐
│ 第一层：name + description（~100 词）                    │
│ → 始终可见，用于判断是否需要加载                           │
├─────────────────────────────────────────────────────────┤
│ 第二层：SKILL.md 正文                                    │
│ → 任务匹配时加载，包含主要指令                             │
├─────────────────────────────────────────────────────────┤
│ 第三层：references/ 目录中的详细文档                      │
│ → 仅在需要具体细节时加载                                  │
└─────────────────────────────────────────────────────────┘
```

### 为什么这么设计？

上下文窗口是宝贵资源。如果把所有专业知识都塞进去，会导致：

- Token 消耗过大
- 模型注意力分散
- 不相关内容干扰输出

渐进式披露让 OpenCode 只加载当前任务需要的内容。

# Skill目录结构

## 基础结构

```
.opencode/
└── skills/
    └── code-review/
        └── SKILL.md      # 技能定义文件（必须大写）
```

## 推荐的完整结构

```
.opencode/
└── skills/
    └── sql-analysis/
        ├── SKILL.md              # 主文件：工作流程和关键逻辑
        └── references/           # 详细文档（按需加载）
            ├── finance.md        # 财务表结构
            ├── product.md        # 产品表结构
            └── examples.md       # 查询示例
```

**原则**：SKILL.md 保持精简，详细内容放到 references/，OpenCode需要时再读取。

# OpenCode搜素位置

| 位置                                          | 作用范围 | 说明            |
| --------------------------------------------- | -------- | --------------- |
| `.opencode/skills/<name>/SKILL.md`          | 当前项目 | 项目专属技能    |
| `~/.config/opencode/skills/<name>/SKILL.md` | 全局     | 所有项目可用    |
| `.claude/skills/<name>/SKILL.md`            | 当前项目 | Claude 兼容格式 |
| `~/.claude/skills/<name>/SKILL.md`          | 全局     | Claude 兼容格式 |

项目路径会从当前目录向上遍历到 git 根目录。

通过 `OPENCODE_CONFIG_DIR` 环境变量可以指定额外的 Skill 搜索路径：

```
export OPENCODE_CONFIG_DIR="/path/to/custom/config"
```

OpenCode 会同时扫描以下位置：

- 默认配置目录：`~/.config/opencode/skill/`
- 自定义配置目录：`$OPENCODE_CONFIG_DIR/skill/`

这对于团队共享 Skill 或在不同环境中使用不同的 Skill 集合非常有用。

# SKILL.md 格式

## 必需的 Frontmatter

```
---
name: sql-analysis
description: 用于分析业务数据：收入、ARR、客户分群、产品使用、销售管道。提供表结构、指标定义、必需过滤器和查询模式。
---
```

| 字段              | 必需 | 说明                               |
| ----------------- | ---- | ---------------------------------- |
| `name`          | 是   | 技能标识符，用于调用               |
| `description`   | 是   | 触发条件描述（**最重要！**） |
| `license`       | 否   | 许可证信息                         |
| `compatibility` | 否   | 兼容性标记                         |
| `metadata`      | 否   | 自定义键值对                       |

## name 命名规范

官方建议遵循以下规范（源码不强制验证，但建议遵循以保持兼容性）：

```
✓ code-review
✓ sql-analysis
✓ git-release
✗ Code_Review    ← 不要用大写
✗ sql--analysis  ← 不要用连续横杠
✗ -review        ← 不要以横杠开头
```

正则参考：`^[a-z0-9]+(-[a-z0-9]+)*$`
