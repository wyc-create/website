# 初始化工作流

## 目标
首次使用或重置项目的记忆系统：先整改目录结构，再建立标准化记忆树，完成 Git 接入与 AGENTS.md 创建。

## 步骤

1. **结构体检（先于一切）**：`git worktree list` 检查布局。发现 worktree 嵌套在项目根内部（如 `<项目根>/.worktrees/xxx`），主动整改：`mkdir ../<项目根名>-worktrees/` 后 `git worktree move <旧路径> ../<项目根名>-worktrees/<分支名>`，再用 `git worktree list` 复核。整改命令与位置规范见 `references/git.md`。体检结果向用户报告后再继续。

2. **确认记忆目录**：检查项目根 `memory/`，查看现有 `MEMORY.md` 与子文件，不要直接覆盖。目录不存在则创建；不使用任何工具的默认全局记忆路径。发现旧记忆在全局默认路径时，提示用户是否迁移。

3. **检查 Git 状态**：按 `references/git.md`——不是仓库则 `git init`；无远程则请用户提供地址（必须等待，不得编造）。

4. **建立记忆树**：
   - `memory/discipline.md`：按 `references/discipline.md` 十四条写入（项目实例可删不可加）。
   - `MEMORY.md`：三节结构——项目概况 / 活跃方向 / 文件索引。与用户确认当前有哪些活跃方向，每个方向建 `track_<方向>.md` 摘要 + `track_<方向>/` 详情子夹。
   - `temp_recent.md`、`temp_archive.md`：建空骨架（顶部各写一行用途与读取限制）。
   - `_branch.md`：为本分支建身份卡（格式见 `references/git.md`），并在 `.gitignore` 追加 `memory/_branch.md`（.gitignore 不存在则创建）。

5. **创建 AGENTS.md**：按 `references/agents-template.md` 生成；若已存在，对照模板补充更新，不盲目覆盖。

6. **健康检查**：跑一次 `references/health.md`，确认初始化后零问题。

7. **首次提交推送**：按 `references/git.md` 提交本次全部文件（memory/、AGENTS.md、.gitignore）。

8. **汇报**：结构体检与整改结果；创建/修改的文件；Git 状态（初始化与否、远程地址、提交推送结果）；MEMORY.md 行数与大小；发现的重复、冲突或潜在风险。
