# Skill Review

Review date: 2026-05-13

Scope:
- Installed Superpowers plugin under `~/.codex/plugins/cache/openai-curated/superpowers/`
- Removed repo-managed skill bundles under `skills/`
- Removed runtime skill bundles under `~/.codex/skills/`

## Summary

This repo no longer vendors or installs its own skill set. The workflow now uses
100% of the installed Superpowers plugin skills for planning, TDD, debugging,
subagent execution, review, and completion.

Repo decision:
- Keep global prompts, specialist agent TOML files, templates, and runtime
  helper scripts in this repo.
- Do not copy skill bundles from this repo into `~/.codex/skills/`.
- Remove old `codex-team-skills`, `stitch-skills`, and `frontend-skills`
  runtime directories during sync.
- Verify that the Superpowers plugin exposes 14 `SKILL.md` files before
  claiming the runtime is ready.

## Superpowers Skills

| Skill | Use |
| --- | --- |
| `using-superpowers` | Entry point for skill selection discipline. |
| `brainstorming` | Requirement and design clarification before implementation. |
| `writing-plans` | Detailed implementation plans after an approved design. |
| `using-git-worktrees` | Isolated workspaces for feature work. |
| `subagent-driven-development` | Plan execution with fresh subagents and review stages. |
| `executing-plans` | Sequential plan execution with checkpoints. |
| `dispatching-parallel-agents` | Parallel investigation for independent tasks. |
| `test-driven-development` | RED-GREEN-REFACTOR implementation discipline. |
| `systematic-debugging` | Root-cause debugging before fixes. |
| `requesting-code-review` | Structured review before merge or handoff. |
| `receiving-code-review` | Technical handling of review feedback. |
| `verification-before-completion` | Evidence gate before completion claims. |
| `finishing-a-development-branch` | Final branch integration workflow. |
| `writing-skills` | Creating or editing skills when explicitly needed. |

## Removed Repo-Managed Skills

| Removed bundle | Reason |
| --- | --- |
| `skills/codex-team-skills/` | Replaced by Superpowers plugin workflows. |
| `skills/frontend-skills/` | Replaced by Superpowers plugin workflows and project-local UI patterns. |
| `skills/stitch-skills/` | Already removed; kept absent. |

## Verification Rule

After changing skill routing or runtime sync, run:

```bash
bin/sync-codex-runtime ~/.codex
bin/check-agent-workflow ~/.codex
```

The check must confirm:
- old repo-managed skill directories are absent in runtime
- Superpowers plugin exists
- exactly 14 Superpowers `SKILL.md` files are present
- expected specialist agent TOML files still parse and sync
