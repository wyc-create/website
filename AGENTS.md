# AGENTS.md — 项目协作指南

> 本文件面向任何接手本项目的 Agent 工具：开始工作前先读本文件，了解本项目如何管理记忆、应遵守哪些规则。

## 项目概况

website 项目，远程仓库 https://github.com/wyc-create/website.git。当前为初始化阶段，项目内容待定，尚未开始实际开发。

## 接手必读（按此顺序）

1. 分支身份：`git branch --show-current` → 核对 `memory/_branch.md`（缺卡建卡，冲突以 git 为准）
2. 读 `memory/MEMORY.md` 三节（概况 / 活跃方向 / 文件索引），**只读主干，不读 track 详情与 temp_archive**
3. 向用户汇报概况与各方向一行摘要，**询问本次在哪个方向推进**；确认后才读该方向 `track_<方向>.md` 及其详情子夹
4. 只读本工具的 `memory/harness_<本工具名>.md`，不读其它工具的；没有记录就直说「没有记录」，不凭印象猜测

## 记忆工作流：skill 注册检查与安装

唯一来源在项目内 `skills/memory-workflow/`：先查本工具是否已注册；已注册核对版本（落后则重装覆盖）；未注册读取项目内全部内容装到本工具全局 skills 目录（目录表见 SKILL.md「安装位置约定」，不在表内查官方文档，不得猜测）。禁止装进项目内建 `.workbuddy/`、`.claude/` 等隐藏目录。**即使不装 skill，本文件与 `memory/` 也必须能独立支撑工作。**

## 记忆管理

- 记忆树结构、十四条纪律唯一来源见 `references/discipline.md`（skill 模板）；项目实例为 `memory/discipline.md`（可删不可加，新条款走回流）
- 要点速记：主干受 8KB/120 行上限、方向详情子夹与 temp_archive 豁免且默认禁读；一主题一文件；弃旧从新直接覆盖；机密不写；工具专属内容按准入门槛拆 `harness_<工具>.md` 且只读本工具；每条规则只写一个方向、写完重读压缩；分支身份只写在本地卡片 `_branch.md`（不入 git），记忆正文不写当前分支
- `_temp/` 每个任务文件夹产生即登记进 `temp_recent.md` 与 `temp_archive.md`

## 会话结束（收尾）

有价值才写：进度、结论、决定及原因、失败方案及原因、关键文件修改、未决风险、下一步；先按方向分拣再落文件，细节落 `track_<方向>/`（带秒级时间戳）。写完更新 `MEMORY.md` 与 temp 登记 → 提交推送。详细见 memory-workflow skill。

## Git 规则

- 及时提交推送；密钥等敏感信息永不提交
- worktree 一律建在 `<项目根名>-worktrees/`（项目根的兄弟目录），禁止嵌套在项目根内
- 共享主干文件只在 main 改，改后 merge 同步各 worktree；日常记忆与改动只在本分支，跨分支唯一通道是合并+主干对账
- 判断推送/同步状态以 `git ls-remote origin` 为准
