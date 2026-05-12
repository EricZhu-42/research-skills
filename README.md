# Research Skills

[中文说明](README_CN.md)

Codex skills for research-oriented workflows.

## Skills

- `skills/worktree-swarm-plan`: helps discuss how to split a large research or engineering task into parallel workstreams, create or synchronize Git worktrees, and write per-worktree execution plans for independent Codex sessions.
- `skills/audit-gated-goal`: helps create Goal Mode prompts for exploratory improvement tasks where the objective is directional or hard to quantify. It uses external Codex sub-agent audits to keep the agent working until the planned deadline or until substantive progress is verified.

## When To Use

Use `worktree-swarm-plan` when a task can be explored in parallel across multiple Git worktrees.

Use `audit-gated-goal` when the task is not a fixed checklist, such as research exploration, method refinement, benchmark discovery, or setting search. It is not intended for clear one-shot tasks like running one specified experiment, fixing one known bug, or generating one fixed file.

## Installation

```bash
npx skills add EricZhu-42/research-skills
```
