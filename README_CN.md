# Research Skills

用于研究工作流的 Codex Skills。

## 安装

```bash
npx skills add EricZhu-42/research-skills
```

## Skills

- `skills/worktree-swarm-plan`：用于先和用户讨论如何把一个大型研究或工程任务拆成可并行推进的部分，再创建或同步多个 Git worktree，并为每个 worktree 写入独立的执行计划。
- `skills/audit-gated-goal`：用于目标还不够明确、只能描述一个大方向时，让 Codex Goal Mode 在一段时间内进行探索式改进。它通过外部 Codex sub-agent 审计来判断是否可以停止，避免模型只做浅层尝试、本地自审或很小的数值波动后就提前结束。

## 使用场景

当一个任务可以拆到多个 Git worktree 中并行探索时，使用 `worktree-swarm-plan`。

当任务不是固定 checklist，而是研究探索、方法打磨、benchmark 寻找、setting 搜索等方向性任务时，使用 `audit-gated-goal`。它不适合明确的一次性任务，例如只跑某个指定实验、修复某个已知 bug，或者生成某个固定文件。
