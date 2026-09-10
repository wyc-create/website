# Git 与工作树管理

## 初始化接入（init 时执行）

1. 项目根 `git status`：不是仓库则 `git init`。
2. `git remote -v`：已配置向用户确认沿用；未配置**必须等用户手动提供地址**再 `git remote add origin <地址>`，不得编造。
3. 首次提交推送全部初始化文件；推送失败如实报告，不得强制推送。

## 日常提交规范

1. 每次修改项目文件后及时提交并推送，不积压。
2. 同一批工作的改动合并一次提交；不相关的分开。
3. 提交信息简要、有描述性（如「更新记忆：同步数据索引结论」）。
4. 密码/密钥/Token/Cookie/私钥永不提交（与纪律第 9 条一致）。
5. 推送失败排查顺序：凭据 → 网络/代理 → 权限（403 多为 token 缺写权限）。查明后报告用户，不用强制推送或破坏性操作绕过。

## 工作树（worktree）布局与创建

布局约定（容器模式——`.git` 只存在于主工作目录，worktree 永不进仓库根）：

```
<容器>/<项目名>_main/     ← 主工作目录（main 分支，.git/ 在此）
<容器>/worktrees/<分支名>/ ← 全部工作树，一个分支一个子夹
```

- 规则所称「项目根」= 含 `.git/` 与 `AGENTS.md` 的主工作目录 `<项目名>_main/`；agent 打开容器层时先进主工作目录再操作记忆。
- `git worktree add` **没有默认位置**，路径完全由命令参数决定——创建命令固定模板：
  `git -C <项目根> worktree add ../worktrees/<分支名> -b <分支名>`
- **禁止**把 worktree 嵌套进主工作目录（仓库根）内（`.worktrees/` 之类）：git 无法彻底忽略仓库内的 worktree，`git status`、check-ignore 行为会异常。
- **共享主干文件（AGENTS.md、纪律、MEMORY.md 概况、他人 track）只在 main 修改**，改完立即推送，再用 `git -C <worktree路径> merge main` 同步到各 worktree（merge 而非手动复制，避免覆盖该分支自有改动）。
- 违规整改：单个 worktree 位置不对 → `git worktree move <旧路径> <容器>/worktrees/<分支名>`；主工作目录本身要迁入容器 → 先把各 worktree 移到目标位，再整体移动主目录，然后在主仓库内执行 `git worktree repair <worktree路径...>` 修复双向指针，`git worktree list` 复核。
- **移动后必查悬空链接**：树内指向旧仓库路径的 junction/符号链接全部失效——`Get-ChildItem -Recurse -Force` 过滤 ReparsePoint 扫描，逐一 `mklink /J` 重建到新路径（大模型权重链接是重灾区）。
- 分支停用后 `git worktree remove <路径>`；不得留下空转 worktree。

## 分支记忆纪律

1. **身份卡 `memory/_branch.md`**：列入 `.gitignore`，故意不入库——合并物理上不会污染它。格式：分支名 / 派生自 / 对应该方向或任务 / 创建时间（秒级）/ 状态（进行中|已合并|废弃）。
2. 会话恢复时先 `git branch --show-current` 核对卡片：缺卡建卡，冲突以 git 为准并修卡。新 worktree 必然无卡（不随检出传播），属正常。
3. 日常记忆只在本分支更新；跨分支流动的唯一通道是合并，禁止跨 worktree 手动复制记忆「同步」。
4. 记忆正文不写「当前在哪个分支」——分支状态由工作区 git 实时反映，写进记忆必然过期（与纪律 7 一致）。

## 合并协议（仅用户明确要求合并时执行）

1. `git merge <分支>` 合入文件改动。冲突和解规则：track 摘要/主干文件按「两侧新事实都保留、删重复」；`temp_recent.md` 超 3 个日期 → 最老的沉入 `temp_archive.md`；`temp_archive.md`（append-only）两侧行都保留，按日期重排。
2. **合并后主干对账（必做）**：更新 MEMORY.md 概况与当前阶段、核对索引、跑一次健康检查（issues 第 2/3/4/12 项重点）。
3. **卡片处理**：被合并分支的卡片状态改「已合并」后删除该 worktree；main 卡片不动——身份卡是本地文件，merge 不影响其它工作区的卡片。
