# Work Log

This file is the durable project memory for future Codex conversations. Keep it
short, factual, and current. Update it whenever a meaningful task starts,
finishes, changes direction, or leaves follow-up work.

## Status Legend

- `Done` - implemented and verified, or intentionally completed with notes.
- `In Progress` - currently being worked on in this branch/session.
- `Pending` - known next work that has not started.
- `Blocked` - cannot continue without a missing dependency, credential, answer,
  or external action.
- `Skipped` - intentionally not done; include the reason.

## Current State

- Active branch: `codex/add-stitch-skills`
- Open PR: https://github.com/iamdinhthuan/codex-skill/pull/1
- Base branch: `main`
- Unrelated worktree changes: root `SKILL.md` is deleted, and `.gitignore` has
  a local `docs/WORKLOG.md` ignore entry. They are not part of this refactor
  unless the user explicitly includes them.

## Tasks

| Status | Task | Notes | Verification |
| --- | --- | --- | --- |
| Done | Add QA repair loop after BE/FE handoff | Tightened repo/global orchestration so QA verifies Backend/Frontend/Stitch implementation handoffs, routes failures back through the main agent to the owning implementation agent, and repeats focused repair checks up to 3 cycles before Review or Blocked status. | Updated `AGENTS.md`, `global/AGENTS.md`, `docs/CLIENT_SETUP.md`, and `docs/GLOBAL_BOOTSTRAP.md`; synced runtime with `bin/sync-codex-runtime ~/.codex`; `diff -q global/AGENTS.md ~/.codex/AGENTS.md`; `git diff --check`; `rg` confirmed QA Repair Loop in source and runtime prompts. |
| Done | Add reusable runtime sync helper | Added `bin/sync-codex-runtime` to install shared global AGENTS, specialist agent TOML files, skill bundles, templates, and helper scripts into `CODEX_HOME` or `~/.codex`; updated setup/bootstrap docs to use the helper. | Verified against a temporary `CODEX_HOME` with expected file counts (23 Codex team skills, 8 Stitch skills), then synced to `~/.codex` and confirmed source/runtime parity with `diff -q`; `git diff --check` passed. |
| Done | Review and sync global agent runtime | Re-reviewed the full agent stack across `global/AGENTS.md`, `global/agents/*.toml`, `~/.codex/AGENTS.md`, `~/.codex/agents/*.toml`, and `~/.codex/config.toml`; then re-synced the source-controlled global files and `team-builder` skill into `~/.codex`. | `diff -q` matched all synced source/runtime file pairs; runtime role config in `~/.codex/config.toml` still points to the expected specialist TOML files. |
| Done | Tighten AGENTS/subagent routing guidance | Clarified that this repo is the source-controlled workflow reference, documented `react:components` as the canonical skill ID with `react-components/` folder mapping, added single-writer file ownership rules, and synced the installed global/runtime prompts. | Reviewed updated `AGENTS.md`, `global/AGENTS.md`, `skills/codex-team-skills/team-builder/SKILL.md`, and synced runtime files under `~/.codex`. |
| Done | Add Google Stitch skills to the repo | Vendored upstream `google-labs-code/stitch-skills` into `skills/stitch-skills/` with README and LICENSE attribution. | Compared copied files with upstream checkout; only local root README/LICENSE additions differ. |
| Done | Integrate Stitch into Frontend Agent workflow | Updated `AGENTS.md` with triggers for `stitch-design`, `taste-design`, `enhance-prompt`, `design-md`, `react:components`, `stitch-loop`, and `shadcn-ui`. | `rg` checks confirmed Stitch references in `AGENTS.md`. |
| Done | Document Stitch source attribution | Updated `README.md` with upstream repo, pinned commit `6c0cbdb909b7d256c8b9b3854c8c8f87aab2c140`, and license path. | Reviewed README diff. |
| Done | Push Stitch integration to GitHub | Created branch `codex/add-stitch-skills`, committed `a45a4bc`, pushed to origin, and opened draft PR #1. | `git push -u origin codex/add-stitch-skills`; `gh pr create --draft`. |
| Done | Install team and Stitch skills globally for all local Codex projects | Copied curated skills into `~/.codex/skills/codex-team-skills` and Stitch skills into `~/.codex/skills/stitch-skills`; appended global team/Stitch instructions to `~/.codex/AGENTS.md`. | Verified 23 curated skills and 8 Stitch skills exist globally. |
| Done | Add durable work log for future conversations | Added this file so future sessions can quickly inspect completed, active, pending, blocked, and skipped work. | Created in `2c873e1`, marked complete in `7b32541`, and pushed to PR #1. |
| Done | Add global bootstrap for new projects | Added global templates for `docs/WORKLOG.md` and project `AGENTS.md`, plus `~/.codex/bin/codex-bootstrap-project`; updated global `~/.codex/AGENTS.md` to create project work logs when meaningful work starts. | Tested bootstrap against `/tmp/codex-bootstrap-test`; it created `AGENTS.md` and `docs/WORKLOG.md`. |
| Done | Rename and review Codex skill bundle | Renamed the curated `everything-claude-code` bundle to `codex-team-skills`, adapted Claude-specific runtime assumptions for Codex, synced the global skill install, and added `docs/SKILL_REVIEW.md`. | Commit `f03ec89`; `git diff --check`; verified 23 repo skills and 23 global skills; `rg` found no stale Claude runtime references outside the review report/source attribution. |
| Done | Document client setup and usage | Added client-machine setup docs, source-controlled global instructions, project templates, and bootstrap script so another machine can install and verify the same Codex workflow. | Verified install commands in `/tmp/codex-client-install-test`; counts: 23 Codex team skills, 8 Stitch skills; `git diff --check`. |
| Done | Push branch into main | Verified this machine's global setup, pushed `codex/add-stitch-skills`, then fast-forwarded remote `main` from the branch. | `git push origin codex/add-stitch-skills:main` updated `main` from `482d251` to `72dbbbc`. |
| Done | Add specialist subagent routing config | Added specialist subagent routing rules to repo/global instructions; added source-controlled `global/agents/*.toml`; synced `backend`, `frontend`, `qa`, `review`, and `stitch-frontend` named agents into `~/.codex/config.toml` and `~/.codex/agents/`; updated client/bootstrap docs. | `tomllib` parsed `~/.codex/config.toml` and agent TOML files; `rg` confirmed config references; `git diff --check` passed. |
| Done | Complete full-role Codex subagent setup | Clarified automatic spawning for independent role slices; removed stale root `SKILL.md` reference from README; added `pm` and `architect` named specialist TOML files; updated sync helper, client setup docs, global bootstrap docs, team-builder wording, `~/.codex/AGENTS.md`, `~/.codex/agents/`, and `~/.codex/config.toml`. | Verified by reading source/runtime routing guidance, confirming `pm` and `architect` exist under both `global/agents/` and `~/.codex/agents/`, confirming `~/.codex/config.toml` has all seven role entries, and searching for stale README/root `SKILL.md` and old role-list wording. A combined shell verification command was canceled before completion. |
| Done | Enforce staged multi-agent implementation flow | Updated source and global instructions so non-trivial implementation uses read-only review/discovery agents first, writing implementation agents second with exclusive file ownership, and the main agent as orchestrator/merger/verifier except for shared, conflict-prone, or explicitly unassigned files. | Reviewed updated `AGENTS.md`, `global/AGENTS.md`, `docs/GLOBAL_BOOTSTRAP.md`, and `docs/CLIENT_SETUP.md`; synced runtime via `bin/sync-codex-runtime ~/.codex`. |
| Done | Compact prompts and add external coordination prototype | Replaced the long repo/global prompts with shorter workflow-first versions, kept the staged review->implementation->QA flow, documented an external coordination prototype in bootstrap/setup docs, added its local MCP config entry, and re-synced runtime. | `bin/sync-codex-runtime ~/.codex`; `tomllib` parsed `~/.codex/config.toml` and `global/agents/*.toml`; `diff -q` confirmed source/runtime parity; bootstrap smoke test created `/tmp/codex-bootstrap-test`; `git diff --check` passed. |
| Done | Smoke test parallel roles and external coordination | Spawned PM, Architect, QA, and Review agents in parallel. They exchanged handoffs through the prototype coordination path, and the main agent patched prompt/docs gaps found by Review: compact prompts preserved the 3-cycle QA repair loop and canonical identity guidance. | Verified subagent spawn for four roles; file reservation cycle granted and released `docs/WORKLOG.md`; follow-up verification pending after runtime sync. |
| Done | Document install path and publish to main | Added README quick-install instructions for cloning the repo, syncing runtime into `~/.codex`, configuring named role agents, verifying counts/config, and using the workflow in another project. | `git diff --check`; `bin/sync-codex-runtime ~/.codex`; `tomllib` parsed `~/.codex/config.toml` and `global/agents/*.toml`; source/runtime `diff -q`; bootstrap smoke test; pushed branch and main. |
| Done | Simplify workflow and remove external mail coordination | Removed the external mail coordination path from source/runtime guidance and local config, kept multi-agent coordination local to Codex subagents and the parent thread, documented research patterns from CrewAI, LangGraph, AutoGen, and `hoangnb24/skills`, pruned the default skill stack from 23/8 to 15/4, and synced the installed runtime. | `rg` found no removed coordination refs in source/runtime; `rg` found no removed skill refs in active routing/docs/runtime; source/runtime skill counts are 15 Codex and 4 Stitch; removed skill directories are absent in source/runtime; `tomllib` parsed config and agents; source/runtime `diff -q`; `git diff --check`; markdown fences balanced. |
| Done | Align subagent config with OpenAI docs | Added required `name` and `description` fields to each specialist agent TOML, set read-only sandbox for PM/Architect/QA/Review, reduced `agents.max_depth` from 2 to 1 to avoid recursive fan-out, and carried over the lightweight Khuym worker-result labels `[DONE]`, `[BLOCKED]`, `[HANDOFF]`, and `[NOOP]`. Synced runtime to `~/.codex`. | `bin/sync-codex-runtime ~/.codex`; `tomllib` parsed `~/.codex/config.toml` and specialist TOML files; schema check confirmed names/descriptions and read-only sandbox; source/runtime `diff -q`; `rg` confirmed `max_depth = 1`; `git diff --check`. |

## Decisions

- Use global `~/.codex/AGENTS.md` for behavior that should apply to every
  project. New projects should inherit this automatically.
- Use project-level `AGENTS.md` only for project-specific rules, commands,
  architecture constraints, or domain context.
- Bootstrap `docs/WORKLOG.md` automatically for meaningful work in new projects
  so task state survives across conversations.
- Keep vendored skill sources under `skills/` with source attribution and pinned
  commits. Do not silently replace them with floating upstream content.
- Do not assume Stitch MCP is available. If MCP tools are missing, continue with
  local prompt/design-system/React conversion work that can be done from files.
- Keep shared prompts compact. Put only the durable workflow, routing, worklog,
  and coordination rules in `AGENTS.md` and `global/AGENTS.md`; move setup and
  operational detail to docs.
- Use real specialist subagents automatically when the user asks for
  multi-agent, parallel, delegated, or role-split execution, when project
  instructions explicitly require specialist subagents, or when independent
  PM/Architect/BE/FE/QA/Review/Stitch slices can run without blocking each
  other. For non-trivial implementation, review/discovery agents run read-only
  first, implementation agents write second with exclusive file ownership, and
  the main agent orchestrates, merges, resolves conflicts, verifies, and updates
  the work log. Otherwise keep small tasks in the main thread with logical team
  roles.
- Use Codex subagents plus parent-thread handoffs for role coordination. Keep
  durable task state in `docs/WORKLOG.md`.
- Keep unrelated worktree changes out of commits. The current `SKILL.md`
  deletion remains outside the Stitch branch commit scope.

## How To Update This Log

When starting a task:

1. Add a row with `In Progress`.
2. Include the branch or affected files when useful.
3. State the intended verification before implementation if the task is risky.

When finishing a task:

1. Change status to `Done`, `Skipped`, or `Blocked`.
2. Add the commit, PR, command output, or manual check that proves the outcome.
3. Note any remaining risk or unrelated worktree change.

Keep entries concise. This file is context for future agents, not a full chat
transcript.
