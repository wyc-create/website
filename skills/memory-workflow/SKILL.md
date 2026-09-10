---
name: memory-workflow
version: 2.1.1
description: 跨会话长期记忆的标准化工作流。方向化记忆树（主干摘要+分枝详情）、temp 双层登记、分支身份卡片、AGENTS.md 项目指南、Git/worktree 规范、过程文件管理。提供初始化、会话收尾、会话恢复、定期维护、记忆健康检查、上下文压缩六套流程。当用户说「会话收尾 / 写记忆 / 记一下 / 同步记忆」「下次继续 / 接着做 / 恢复记忆 / 上次进度」「整理记忆 / 维护记忆 / 记忆体检 / 检查记忆 / 审计记忆」「压缩上下文 / compact」「初始化记忆 / 初始化 memory」时使用。
---

# 长期记忆管理工作流

## 触发条件

六套流程可独立调用，以自然语言触发（不依赖斜杠命令）：

| 流程 | 自然语言触发词 | 用途 |
|------|----------------|------|
| 初始化 | 初始化记忆 / 初始化 memory | 首次使用或重置：结构体检→记忆树→Git→AGENTS.md |
| 会话收尾 | 会话收尾 / 写记忆 / 记一下 / 同步记忆 | 会话结束前同步有价值信息 |
| 会话恢复 | 继续上次 / 接着做 / 恢复记忆 / 上次进度 | 新会话按两级协议加载上下文 |
| 定期维护 | 维护记忆 / 整理记忆 | 按问题清单动手清理 |
| 记忆健康检查 | 记忆体检 / 检查记忆 / 审计记忆 | 按同一清单只出报告，不动文件 |
| 上下文压缩 | 压缩上下文 / 压缩记忆 / compact | 对话中压缩上下文 |

## 记忆树（结构与加载）

记忆系统是一棵树：主干默认加载、受上限约束；方向分枝默认禁读、上限豁免。

```
memory/
├── MEMORY.md            # 主干索引，固定三节：概况 / 活跃方向 / 文件索引
├── discipline.md        # 写作纪律（模板实例）
├── track_<方向>.md      # 方向摘要：进度+关键决定+待办+指针（8KB/120行内）
├── track_<方向>/        # 方向详细档案（分枝：上限豁免，默认禁读）
├── temp_recent.md       # temp 一级：最近 3 个日期文件夹详情（滚动）
├── temp_archive.md      # temp 二级：全历史流水（只追加，超限豁免，默认禁读）
├── _branch.md           # 分支身份卡（git 未跟踪的本地文件）
└── harness_<工具>.md    # 工具专属行为（准入门槛见纪律；只读本工具）
```

- 上限只约束默认加载的主干文件；`track_<方向>/` 内详细报告不限长，但只有用户选定该方向后才读入。
- 方向级详细报告一律写入 `track_<方向>/`（不进 `docs/`、不进 `_temp/`），文档带精确到秒的时间戳。
- 两级加载：agent 先只读概况 → 汇报并**询问用户本次推进方向** → 才读该方向 track。禁止未定向就全量加载。

## 安装位置约定（接手项目时先做这一步）

本 skill 的**唯一来源**保存在项目内的 `skills/memory-workflow/`，随仓库版本控制。

1. **先查注册**：在本 agent 工具中查看是否已注册 `memory-workflow`。
2. **已注册**：核对版本——确认已安装版本 ≥ 项目内 `SKILL.md` frontmatter 的 `version`；落后则**重新安装覆盖**再继续。
3. **未注册**（首次接手 / 项目初始化）：读取项目内 `skills/memory-workflow/` 全部内容，安装到**本工具的全局（user-level）skills 目录**：

   | 工具 | 全局 skills 目录 |
   |------|------------------|
   | WorkBuddy / CodeBuddy | `~/.workbuddy/skills/memory-workflow/` |
   | Codex | `~/.codex/skills/memory-workflow/` |
   | Cursor | `~/.cursor/skills-cursor/memory-workflow/` |
   | zcode | `~/.zcode/skills/memory-workflow/` |
   | Claude Code | `~/.claude/skills/memory-workflow/` |

   **不在表内的工具：查该工具官方文档确认其 user-level skills 目录，不得猜测。**

4. **禁止装进项目内部**：不要在项目根目录创建 `.workbuddy/`、`.claude/` 等工具专属隐藏目录；全局装一次，所有项目共用。
5. 无论是否安装本 skill，项目内 `AGENTS.md` 与 `memory/` 必须能独立支撑工作——本 skill 只提供流程细节。

## 核心纪律

全部写作纪律（十四条）唯一来源为 `references/discipline.md`（模板种子）。

- 若目标项目已有 `memory/discipline.md`，以项目实例为准；项目实例**可删不可加**，新条款走回流机制（纪律第 14 条）。
- 项目尚无该文件时，按 `references/init.md` 用模板种子初始化。

## 项目工程规范

- **AGENTS.md**：初始化时按 `references/agents-template.md` 生成。它是共享主干文件：只在 main 分支修改，改后立即推送，并用 `git merge main` 同步到各 worktree；功能分支不得改。
- **Git 与工作树**：日常提交推送、worktree 位置规范、分支记忆纪律与合并协议，见 `references/git.md`。
- **过程文件（_temp）**：规范见 `references/temp.md`；每个任务文件夹**产生即登记**进 temp 双层索引。

## 工作流执行规范

### 执行前检查（对全部工作流生效，各流程文件不再重复）

1. 核对已安装 skill 版本（见「安装位置约定」第 2 条）。
2. 确认记忆系统可用，无法确认则停止并告知用户。
3. 记忆目录固定为**当前已打开项目根目录下的 `memory/`**；不存在则创建。任何工作流不得将记忆写入工具的默认全局记忆路径。

### 引用优先级

| 内容 | 文件 | 说明 |
|------|------|------|
| 写作纪律（十四条） | `references/discipline.md` | 唯一来源 |
| 共用问题清单（16 项） | `references/issues.md` | 体检与维护共用 |
| 初始化 | `references/init.md` | |
| 会话收尾 | `references/wrapup.md` | |
| 会话恢复 | `references/resume.md` | |
| 定期维护 | `references/maintenance.md` | 动手模式 |
| 记忆健康检查 | `references/health.md` | 只读模式 |
| 上下文压缩 | `references/compact.md` | |
| AGENTS.md 模板 | `references/agents-template.md` | |
| Git / worktree / 合并协议 | `references/git.md` | |
| 过程文件规范 | `references/temp.md` | |

### 执行后汇报（统一要求，流程文件只列增量汇报点）

- 当前记忆目录路径与所在分支；创建/修改了哪些文件及一句话摘要；MEMORY.md 当前行数和大小；发现的重复、冲突或潜在风险；本次改动涉及的 Git 提交与推送情况（如有文件改动）。
