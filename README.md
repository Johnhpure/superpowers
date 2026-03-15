# Superpowers

Superpowers 是一套为 AI 编码助手打造的**完整软件开发工作流系统**，基于一组可组合的"技能（Skills）"和一系列初始指令构建而成，确保你的 AI 编码助手自动遵循软件工程最佳实践。

## 工作原理

从你启动 AI 编码助手的那一刻起，Superpowers 就开始发挥作用。当 AI 助手发现你要构建一个功能时，它**不会**直接跳入写代码，而是先退后一步，通过苏格拉底式提问了解你真正想做什么。

对话梳理出需求规格后，AI 助手会将设计分段展示——每段都足够短，便于你实际阅读和消化。

在你确认设计之后，AI 助手会制定一份详尽的实施计划——清晰到即使一个热情但缺乏品味、判断力和项目上下文的初级工程师也能遵循。计划强调真正的红/绿 TDD（测试驱动开发）、YAGNI（你不会需要它）和 DRY（不要重复自己）原则。

最后，当你说"开始"时，AI 助手会启动*子代理驱动开发*流程，让代理逐个处理每个工程任务，检查和审查他们的工作，然后继续推进。AI 自主工作数小时而不偏离你制定的计划是常见的事。

上述只是系统的核心。由于技能会自动触发，你无需做任何特别的事情。你的 AI 编码助手就拥有了 Superpowers。

## 赞助

如果 Superpowers 帮助你完成了赚钱的项目，并且你有此意愿，我非常感谢你考虑[赞助我的开源工作](https://github.com/sponsors/obra)。

谢谢！

- Jesse

## 安装

> **注意：** 不同平台的安装方式不同。Claude Code 和 Cursor 有内置的插件市场，Codex 和 OpenCode 需要手动配置，**Antigravity IDE 使用 Junction Link + Skills 系统集成**。

### Antigravity IDE（推荐）

Antigravity IDE 通过 **Junction Link + 入口 Skill + Workflow 快捷命令** 的三层架构与 Superpowers 集成。

#### 前提条件

- Windows 操作系统
- Antigravity IDE 已安装且 skills 目录就绪（默认：`~/.gemini/antigravity/skills/`）
- 本仓库已克隆到本地

#### 安装步骤

**第 1 步：克隆仓库**

```powershell
cd D:\Github  # 或你的项目目录
git clone https://github.com/Johnhpure/superpowers.git
```

**第 2 步：创建 Junction Links（注册 Skills）**

以管理员身份运行 PowerShell，执行以下命令将所有 Superpowers 技能注册到 Antigravity：

```powershell
# 设置路径变量（请根据你的实际路径修改）
$superpowersSkills = "D:\Github\superpowers\skills"
$antigravitySkills = "$env:USERPROFILE\.gemini\antigravity\skills"

# 为每个 Superpowers 技能创建 Junction Link
Get-ChildItem -Directory $superpowersSkills | ForEach-Object {
    $linkPath = Join-Path $antigravitySkills "superpowers-$($_.Name)"
    if (-not (Test-Path $linkPath)) {
        cmd /c mklink /J "$linkPath" $_.FullName
        Write-Host "✅ 已创建: superpowers-$($_.Name)"
    } else {
        Write-Host "⏭️ 已存在: superpowers-$($_.Name)"
    }
}
```

**第 3 步：创建入口 Skill**

在 Antigravity skills 目录下创建 `superpowers-core` 文件夹和 `SKILL.md` 入口文件：

```powershell
$corePath = "$antigravitySkills\superpowers-core"
New-Item -ItemType Directory -Path $corePath -Force | Out-Null
```

然后将仓库中的 `skills/using-superpowers/references/antigravity-tools.md` 内容作为参考，创建 `SKILL.md` 入口文件。入口文件应包含：

- **工具映射表**（Claude Code 工具名 → Antigravity 等价工具）
- **平台限制说明**（子代理不可用，回退到 executing-plans 模式）
- **完整技能索引**

> 仓库已提供参考文件：[`skills/using-superpowers/references/antigravity-tools.md`](skills/using-superpowers/references/antigravity-tools.md)

**第 4 步：创建 Workflow 快捷命令（可选，项目级）**

在你的项目根目录下创建 `.agents/workflows/` 目录，并添加以下快捷命令文件：

```powershell
$workflowDir = ".agents\workflows"
New-Item -ItemType Directory -Path $workflowDir -Force | Out-Null
```

创建 4 个 workflow 文件：

| 文件名 | Slash 命令 | 用途 |
|--------|-----------|------|
| `brainstorm.md` | `/brainstorm` | 头脑风暴，在编码前探索需求并设计方案 |
| `write-plan.md` | `/write-plan` | 编写实施计划，将设计方案分解为细粒度任务 |
| `execute-plan.md` | `/execute-plan` | 执行实施计划，按批次完成并在检查点审查 |
| `debug.md` | `/debug` | 系统化调试（根因调查→模式分析→假设验证→修复） |

每个 workflow 文件的格式如下：

```markdown
---
description: 简短描述
---

# 工作流名称

## 步骤
1. 读取入口技能：使用 view_file 读取 superpowers-core/SKILL.md
2. 读取对应技能：使用 view_file 读取对应的 SKILL.md
3. 按照 SKILL.md 严格执行
```

#### 验证安装

安装完成后，在 Antigravity 新会话中测试：

```
# 方式 1：使用 slash 命令
/brainstorm

# 方式 2：直接请求
请使用 superpowers 的头脑风暴技能来帮我设计一个新功能

# 方式 3：自动匹配（直接描述任务，Antigravity 会自动关联相关 skill）
帮我设计一个用户认证系统
```

#### Antigravity 工具映射表

Superpowers 技能中引用的是 Claude Code 工具名称。在 Antigravity 中，请参考以下映射：

| Claude Code 工具 | Antigravity 等价工具 | 说明 |
|-----------------|---------------------|------|
| `Read` | `view_file` | 读取文件 |
| `Write` | `write_to_file` | 创建文件 |
| `Edit` | `replace_file_content` | 编辑文件 |
| `Bash` | `run_command` | 运行命令 |
| `Grep` | `grep_search` | 搜索文件内容 |
| `Glob` | `find_by_name` | 按名称搜索文件 |
| `TodoWrite` | Artifact markdown | 使用 `- [ ]` 复选框清单追踪任务 |
| `Skill` | `view_file` | 读取 SKILL.md 文件 |
| `Task` | ❌ 不支持 | 回退到 `executing-plans` 模式 |
| `WebSearch` | `search_web` | 网络搜索 |
| `WebFetch` | `read_url_content` | 获取网页内容 |

#### Antigravity 平台限制

- **无通用子代理**：Antigravity 的 `browser_subagent` 仅支持浏览器操作，不等同于 Claude Code 的 `Task` 工具。`subagent-driven-development` 和 `dispatching-parallel-agents` 技能需要回退到 `executing-plans`（单会话批量执行）模式。
- **TodoWrite 替代**：使用 Artifact markdown 文件中的 `- [ ]` 复选框语法追踪任务进度。

### Claude Code 官方市场

可通过 [Claude 官方插件市场](https://claude.com/plugins/superpowers) 获取 Superpowers：

```bash
/plugin install superpowers@claude-plugins-official
```

### Claude Code（通过插件市场）

在 Claude Code 中，先注册市场：

```bash
/plugin marketplace add obra/superpowers-marketplace
```

然后从市场安装插件：

```bash
/plugin install superpowers@superpowers-marketplace
```

### Cursor（通过插件市场）

在 Cursor Agent 对话中安装：

```text
/add-plugin superpowers
```

或在插件市场中搜索 "superpowers"。

### Codex

告诉 Codex：

```
Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.codex/INSTALL.md
```

**详细文档：** [docs/README.codex.md](docs/README.codex.md)

### OpenCode

告诉 OpenCode：

```
Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.opencode/INSTALL.md
```

**详细文档：** [docs/README.opencode.md](docs/README.opencode.md)

### Gemini CLI

```bash
gemini extensions install https://github.com/obra/superpowers
```

更新：

```bash
gemini extensions update superpowers
```

### 验证安装

在你选择的平台上启动新会话，请求一些应该触发技能的操作（例如"帮我规划这个功能"或"让我们调试这个问题"）。AI 助手应自动调用相关的 Superpowers 技能。

## 基础工作流

```
头脑风暴 → Git Worktree → 编写计划 → 执行计划 → TDD → 代码审查 → 验证 → 完成分支
```

1. **brainstorming（头脑风暴）** — 在写代码前激活。通过提问提炼粗略想法，探索替代方案，分段展示设计以供验证。保存设计文档。

2. **using-git-worktrees（使用 Git 工作树）** — 设计批准后激活。在新分支上创建隔离工作区，运行项目配置，验证测试基线。

3. **writing-plans（编写计划）** — 有了批准的设计后激活。将工作分解为小任务（每个 2-5 分钟）。每个任务都有精确的文件路径、完整代码和验证步骤。

4. **subagent-driven-development（子代理驱动开发）** 或 **executing-plans（执行计划）** — 有了计划后激活。为每个任务分派全新的子代理，进行两阶段审查（规格合规性 + 代码质量），或在批量执行中设置人工检查点。

5. **test-driven-development（测试驱动开发）** — 实施期间激活。强制执行 RED-GREEN-REFACTOR 循环：写失败测试 → 看它失败 → 写最少代码 → 看它通过 → 提交。在测试之前写的代码会被删除。

6. **requesting-code-review（请求代码审查）** — 在任务之间激活。根据计划进行审查，按严重程度报告问题。关键问题会阻止进度。

7. **finishing-a-development-branch（完成开发分支）** — 任务完成时激活。验证测试，呈现选项（合并/PR/保留/丢弃），清理工作树。

**AI 助手在执行任何任务前都会检查是否有相关技能。** 这是强制性的工作流，而非建议。

## 技能库

### 测试
- **test-driven-development** — RED-GREEN-REFACTOR 循环（包含测试反模式参考）

### 调试
- **systematic-debugging** — 四阶段根因分析流程（包含根因追踪、纵深防御、基于条件的等待等技术）
- **verification-before-completion** — 确保问题真正被修复

### 协作
- **brainstorming** — 苏格拉底式设计精炼
- **writing-plans** — 详细的实施计划
- **executing-plans** — 带检查点的批量执行
- **dispatching-parallel-agents** — 并发子代理工作流
- **requesting-code-review** — 审查前检查清单
- **receiving-code-review** — 回应反馈
- **using-git-worktrees** — 并行开发分支
- **finishing-a-development-branch** — 合并/PR 决策工作流
- **subagent-driven-development** — 两阶段审查的快速迭代（规格合规性 + 代码质量）

### 元技能
- **writing-skills** — 遵循最佳实践创建新技能（包含测试方法论）
- **using-superpowers** — 技能系统使用入门

## Antigravity 使用示例

### 示例 1：使用头脑风暴设计新功能

```
用户: /brainstorm
AI: 我正在使用 superpowers 的头脑风暴技能。让我先了解一下项目上下文。
    [读取项目文件和文档]
    你想构建什么功能？

用户: 我想为我的博客添加评论系统

AI: 让我逐个问一些澄清问题 ——
    评论系统需要支持匿名评论，还是必须登录后才能评论？
    A) 仅登录用户可评论
    B) 允许匿名评论
    C) 两种都支持

用户: A

AI: 好的。你希望评论支持嵌套回复（类似 Reddit），还是只支持平级评论？
    ...
    [继续提问直到需求明确]

AI: 根据我们的讨论，我推荐以下设计方案：
    ## 方案 1（推荐）：基于数据库的评论系统
    ...
    ## 方案 2：使用第三方服务（Disqus/Giscus）
    ...
    你更倾向哪个方案？

用户: 方案 1

AI: [分段展示详细设计文档，每段确认]
    设计文档已保存。现在我将使用 writing-plans 技能来编写实施计划。
```

### 示例 2：使用系统化调试排查 Bug

```
用户: /debug
AI: 我正在使用 superpowers 的系统化调试技能。

    ## 阶段 1：根因调查
    请描述你遇到的问题 ——
    1. 具体的错误信息是什么？
    2. 问题是否可以稳定复现？
    3. 最近做了哪些代码变更？

用户: 用户登录后页面一直加载，控制台显示 401 错误

AI: [仔细阅读错误信息]
    [检查最近的 Git 提交]
    [追踪数据流]

    ## 阶段 2：模式分析
    我找到了一个类似的可工作端点 `/api/profile`。
    与出错的 `/api/dashboard` 对比差异：
    - profile 使用了 `authMiddleware`
    - dashboard 缺少 middleware 配置

    ## 阶段 3：假设
    假设：`/api/dashboard` 路由配置缺少认证中间件。

    ## 阶段 4：修复
    [先写一个失败的测试]
    [实施最小修复]
    [验证修复]
```

### 示例 3：TDD 开发新功能

```
用户: /execute-plan

AI: 我正在使用 executing-plans 技能来实施计划。
    [读取计划文件]

    ## Task 1: 用户验证函数

    🔴 RED — 写失败测试：
    [创建 test_validator.py]

    运行测试 → 确认失败 ✅（函数不存在）

    🟢 GREEN — 写最少代码让测试通过：
    [创建 validator.py]

    运行测试 → 全部通过 ✅

    🔵 REFACTOR — 清理代码
    运行测试 → 仍然通过 ✅

    git commit -m "feat: 添加用户验证函数"

    → 继续 Task 2...
```

## 设计哲学

- **测试驱动开发** — 永远先写测试
- **系统化而非临时拼凑** — 流程优于猜测
- **降低复杂度** — 简单性是首要目标
- **证据优于断言** — 验证后再宣告成功

了解更多：[Superpowers for Claude Code](https://blog.fsck.com/2025/10/09/superpowers/)

## 贡献

技能直接存放在本仓库中。贡献方式：

1. Fork 本仓库
2. 为你的技能创建分支
3. 遵循 `writing-skills` 技能来创建和测试新技能
4. 提交 PR

详见 `skills/writing-skills/SKILL.md` 完整指南。

## 更新

### Antigravity IDE

Junction Link 会自动反映源文件的最新内容。只需更新仓库即可：

```powershell
cd D:\Github\superpowers
git pull
# 无需额外操作，所有 skill 自动更新
```

### 其他平台

技能会在更新插件时自动更新：

```bash
/plugin update superpowers
```

## 卸载

### Antigravity IDE

```powershell
# 移除 Junction Links
$antigravitySkills = "$env:USERPROFILE\.gemini\antigravity\skills"
Get-ChildItem $antigravitySkills -Directory -Filter "superpowers-*" | Remove-Item -Force

# 移除入口 Skill
Remove-Item "$antigravitySkills\superpowers-core" -Recurse -Force

# 移除 Workflow 文件（项目级，可选）
Remove-Item ".agents\workflows\brainstorm.md" -Force -ErrorAction SilentlyContinue
Remove-Item ".agents\workflows\write-plan.md" -Force -ErrorAction SilentlyContinue
Remove-Item ".agents\workflows\execute-plan.md" -Force -ErrorAction SilentlyContinue
Remove-Item ".agents\workflows\debug.md" -Force -ErrorAction SilentlyContinue
```

## 许可证

MIT License - 详见 LICENSE 文件

## 支持

- **Issues**: https://github.com/obra/superpowers/issues
- **Marketplace**: https://github.com/obra/superpowers-marketplace
