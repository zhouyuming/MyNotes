# Skill介绍

重新定义AI的能力边界

Skill 是一套**按需加载**的专业知识包，通过 SKILL.md 定义，Agent 根据任务语义自动发现并加载。

“裸模型”无法胜任复杂任务

模型天生只会“理解和生成文本”，并不具备解析复杂结构（如PDF、Excel）的内置能力。

从Prompt到“按需加载”的进化

核心创新：将“能力说明”从Prompt中彻底拆分

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

## description 写法（决定触发的关键）

description 是**唯一决定 Skill 是否被触发**的因素。OpenCode使用语义理解（不是关键词匹配）来判断任务是否需要某个 Skill。

**差的写法**：

```yaml
description: 帮助处理文档
```

问题：太模糊，AI 无法判断何时触发。

**好的写法**：

```yaml
description: |
  从 PDF 中提取表格并转换为 CSV 格式，用于数据分析工作流。
  适用：填写 PDF 表单、批量处理 PDF 文档、提取 PDF 内嵌数据。
  不适用：简单 PDF 查看、基本格式转换、PDF 编辑。
```

**description 写作模板**：

```yaml
description: |
  [一句话说明核心能力]
  提供：[该 Skill 包含的资源，如表结构、公式、模板]
  适用：[触发场景1]、[触发场景2]、[触发场景3]
  不适用：[边界场景1]、[边界场景2]
```

**description 要素**：

1. **具体能力**：能做什么（提取表格、转换格式）
2. **提供资源**：包含什么（表结构、公式、模板）
3. **触发场景**：什么时候用（批量处理、表单填写）
4. **边界限制**：什么时候不用（简单查看）

### 完整示例

```markdown
---
name: sql-analysis
description: |
  用于分析业务数据：收入趋势、ARR 计算、客户分群、产品使用、销售管道。
  提供：公司表结构、指标定义公式、标准过滤器、常用查询模板。
  适用：需要写 SQL 分析业务数据、理解公司指标定义、查询数据仓库。
  不适用：数据库管理、DDL 操作、性能调优、通用 SQL 教学。
---

# SQL 分析技能

## 快速工作流程

当用户请求数据分析时：

1. **明确需求**
   - 什么时间范围？（默认当年）
   - 哪个客户分群？
   - 这个分析用于什么决策？

2. **检查现有看板**
   - 查看 `references/dashboards.md` 是否有现成报表
   - 如果有，优先引导用户使用

3. **确定数据源**
   - 优先使用汇总表而非原始事件数据
   - 查询前确认表有必需字段

4. **执行分析**
   - 应用必需过滤器（排除测试账户等）
   - 用已知基准验证结果

## 标准查询过滤器

所有收入查询必须：
- 排除测试账户：`WHERE account != 'Test'`
- 只用完整周期：`WHERE month <= DATE_TRUNC(CURRENT_DATE(), MONTH)`

## ARR 计算方式

- 月收入转 ARR：`monthly_revenue * 12`
- 7 日运行率：`rolling_7d * 52`

## 详细文档

需要表结构和查询模式时，参考：
- **收入与财务** → `references/finance.md`
- **产品使用** → `references/product.md`
- **销售管道** → `references/sales.md`
```

注意：SKILL.md 只包含工作流程和关键逻辑，详细的表结构放在 references/ 目录中。

# Skill 如何被发现和加载

## 发现机制

OpenCode 启动时扫描所有 Skill，将 name 和 description 汇总到 `skill` 工具的描述中：

```xml
<available_skills>
  <skill>
    <name>sql-analysis</name>
    <description>用于分析业务数据：收入、ARR、客户分群...</description>
  </skill>
  <skill>
    <name>code-review</name>
    <description>执行代码审查，检查规范、Bug、性能和安全</description>
  </skill>
</available_skills>
```

## 加载机制

当用户发送消息时，OpenCode根据语义判断是否需要加载某个 Skill：

```
用户消息：帮我分析上季度的收入数据

OpenCode 判断：这是数据分析任务，与 sql-analysis Skill 匹配

OpenCode 调用：skill({ name: "sql-analysis" })

结果：SKILL.md 内容加载到上下文
```

## 加载后的输出

```
## Skill: sql-analysis

**Base directory**: /path/to/.opencode/skill/sql-analysis

[SKILL.md 的内容]
```

`Base directory` 信息让 OpenCode 知道如何访问 references/ 中的相对路径文件。

# 权限配置

## 全局权限

在 `opencode.json` 中配置：

```jsonc
{
  "permission": {
    "skill": {
      "pr-review": "allow",        // 立即加载
      "internal-*": "deny",        // 隐藏，拒绝访问
      "experimental-*": "ask",     // 加载前询问用户
      "*": "allow"                 // 其他默认允许
    }
  }
}
```

| 权限值    | 行为                            |
| --------- | ------------------------------- |
| `allow` | Skill 立即加载                  |
| `deny`  | Skill 对 Agent 隐藏，访问被拒绝 |
| `ask`   | 加载前提示用户确认              |

> 支持通配符：`internal-*` 匹配 `internal-docs`、`internal-tools` 等。

## 按 Agent 覆盖权限

**在 Markdown Agent 中**：

```yaml
---
permission:
  skill:
    "documents-*": "allow"
---
```

**在 opencode.json 中**：

```jsonc
{
  "agent": {
    "plan": {
      "permission": {
        "skill": {
          "internal-*": "allow"
        }
      }
    }
  }
}
```

## 禁用 Skill 工具

对于不需要 Skill 的 Agent，可以完全禁用：

**Markdown 方式**：

```yaml
---
tools:
  skill: false
---
```

**JSON 方式**：

```jsonc
{
  "agent": {
    "plan": {
      "tools": {
        "skill": false
      }
    }
  }
}
```

禁用后，`<available_skills>` 部分将完全不出现在该 Agent 的工具描述中。

# 简单示例

## 翻译技能

```markdown
---
name: translate
description: 专业翻译，保留格式和术语。用于翻译技术文档、API 文档、代码注释。
---

# 翻译技能

## 翻译规范

1. 保持原文格式和段落结构
2. 专有名词保留原文并标注
3. 技术术语查阅术语表
4. 翻译后进行通读润色

## 输出格式

翻译结果用代码块包裹：

```

{译文}

```

对于不确定的翻译，用括号标注原文。
```

## 品牌指南技能

```markdown
---
name: brand-guidelines
description: 应用公司官方品牌色和排版规范。用于创建需要公司视觉风格的文档、演示文稿、界面设计。
---

# 品牌规范技能

## 颜色

**主色**：
- 深色：`#141413` - 主要文字和深色背景
- 浅色：`#faf9f5` - 浅色背景和深色上的文字
- 中灰：`#b0aea5` - 次要元素

**强调色**：
- 橙色：`#d97757` - 主强调色
- 蓝色：`#6a9bcc` - 次强调色
- 绿色：`#788c5d` - 第三强调色

## 字体

- **标题**：Poppins（备选 Arial）
- **正文**：Lora（备选 Georgia）

## 应用规则

- 标题（24pt 及以上）使用 Poppins
- 正文使用 Lora
- 根据背景智能选择文字颜色
```

# Agent Skill封装

我们刚才的对话已经磨合出了完整的工作流程和输出标准。请现在将这个过程整理成一个标准的Agent Skill，要求如下：

1、创建完整的Skill文件夹结构

2、Skill写清楚：Skill职责、触发场景、执行步骤、输出标准

3、references放入我们确认过的所有格式要求和内容标准

4、可自动化的步骤写入scripts

5、assets放入需要复用的模板文件

输出一个我可以直接安装使用的Skill文件夹

# 下载skill

anthropics官方仓库：[https://github.com/anthropics/skills]()

Skills社群网站：[https://skillsmp.com/zh]()

优秀开源集合：[https://github.com/ComposioHQ/awesome-claude-skills]()

视频制作skill：[https://github.com/remotion-dev/skills]()

ui-ux-pro-max：[https://github.com/nextlevelbuilder/ui-ux-pro-max-skill]()

colleague-skill：[https://github.com/titanwings/colleague-skill]()

awesome-design-md：[https://github.com/VoltAgent/awesome-design-md]()

# 参考链接

[AI大模型Agent Skills完全解析](https://www.bilibili.com/video/BV1je6FBhECT/?spm_id_from=333.337.search-card.all.click&vd_source=300730e0f86bed3fb2c80c0103709a12)

[魔塔skills市场](https://www.modelscope.cn/skills)

[Agent Skills保姆级教程](https://www.bilibili.com/video/BV1ahFmzqE9z?spm_id_from=333.788.player.switch&vd_source=300730e0f86bed3fb2c80c0103709a12)
