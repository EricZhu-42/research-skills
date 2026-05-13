# Research Skills

用于研究工作流的 Codex Skills。

## 安装

```bash
npx skills add EricZhu-42/research-skills
```

## Skills

- `skills/worktree-create`：用于创建或复用 Git worktree，适合用户只想快速准备一个独立 checkout，之后手动或让 agent 在其中继续工作。它只处理分支、路径、安全创建和基础验证，不负责拆分研究任务。
- `skills/research-split-plan`：用于把一个研究或工程方向拆成可并行推进的子任务，并写入 `WORKTREE_PLAN.md` 等执行计划。它本身不创建 Git worktree。
- `skills/audit-gated-goal`：用于目标还不够明确、只能描述一个大方向时，让 Codex Goal Mode 在一段时间内进行探索式改进。它通过外部 Codex sub-agent 审计来判断是否可以停止，避免模型只做浅层尝试、本地自审或很小的数值波动后就提前结束。

## 使用场景

只需要创建或复用一个 Git worktree 时，使用 `worktree-create`。例如用户想先准备一个独立目录，之后自己在上面做事。

需要把宽泛研究方向拆成多个可并行子话题、明确责任边界、成功标准和计划文件时，使用 `research-split-plan`。

当任务不是固定 checklist，而是研究探索、方法打磨、benchmark 寻找、setting 搜索等方向性任务时，使用 `audit-gated-goal`。它不适合明确的一次性任务，例如只跑某个指定实验、修复某个已知 bug，或者生成某个固定文件。

完整工作流可以按这个顺序组合：

```text
research-split-plan -> worktree-create -> audit-gated-goal
```
