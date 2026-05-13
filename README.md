# Research Skills

[中文说明](README_CN.md)

Codex skills for research-oriented workflows.

## Installation

```bash
npx skills add EricZhu-42/research-skills
```

## Skills

- `skills/worktree-create`: creates or reuses Git worktrees for manual or agent-driven follow-up work. It handles branch/path selection, safe setup, and basic verification without decomposing the research task.
- `skills/research-split-plan`: splits a research or engineering direction into independent workstreams and writes execution plans such as `WORKTREE_PLAN.md`. It does not create Git worktrees by itself.
- `skills/audit-gated-goal`: helps create Goal Mode prompts for exploratory improvement tasks where the objective is directional or hard to quantify. It uses external Codex sub-agent audits to keep the agent working until the planned deadline or until substantive progress is verified.

## When To Use

Use `worktree-create` when you only need a new or reused Git worktree, for example to manually continue work in a separate checkout.

Use `research-split-plan` when you want to decompose a broad topic into parallel subtopics, define ownership and success criteria, or prepare plan files before creating worktrees.

Use `audit-gated-goal` when the task is not a fixed checklist, such as research exploration, method refinement, benchmark discovery, or setting search. It is not intended for clear one-shot tasks like running one specified experiment, fixing one known bug, or generating one fixed file.

For the full workflow, use:

```text
research-split-plan -> worktree-create -> audit-gated-goal
```
