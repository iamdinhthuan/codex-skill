# Skill Review

Review date: 2026-05-02

Scope:
- `skills/codex-team-skills/`
- `skills/stitch-skills/`
- global install paths under `~/.codex/skills/`

## Summary

The skill stack has been pruned for a lean Codex team workflow. The goal is to
keep skills that reliably improve common product, backend, frontend, QA,
security, documentation, and verification work, while removing skills that add
heavy ceremony, duplicate core instructions, or target rare workflows.

Research inputs:
- `crewAIInc/crewAI`: role/task/process separation, sequential or manager-led
  execution, and explicit task context.
- `langchain-ai/langgraph`: stateful graph execution, durable state, human
  oversight, and debuggable transitions.
- `microsoft/autogen`: predefined agent/team patterns with clear run and
  termination behavior.
- `hoangnb24/skills`: local-first Codex coordination through subagents, parent
  thread handoffs, validation gates, and local artifacts without an external
  mail service. The useful lightweight carry-over is explicit worker result
  labels such as `[DONE]`, `[BLOCKED]`, `[HANDOFF]`, and `[NOOP]`.

Repo decision:
- Keep orchestration local to Codex subagents, parent-thread handoffs,
  explicit file ownership, and `docs/WORKLOG.md`.
- Do not require an external coordination server for normal multi-agent work.
- Keep 15 Codex team skills and 4 Stitch skills.

## Kept Codex Team Skills

| Skill | Status | Review Notes |
| --- | --- | --- |
| `accessibility` | Kept | Practical frontend quality gate for WCAG 2.2 AA work. |
| `api-design` | Kept | Common backend/API design guidance with low ceremony. |
| `backend-patterns` | Kept | Useful for Node, Express, and Next.js backend work. |
| `browser-qa` | Kept | Needed for real UI/browser verification after frontend changes. |
| `coding-standards` | Kept | Lightweight review lens for readability and maintainability. |
| `database-migrations` | Kept | Important for schema changes and rollback safety. |
| `documentation-lookup` | Kept | Prevents stale framework/API assumptions. |
| `e2e-testing` | Kept | Focused Playwright strategy for user-facing flows. |
| `frontend-design` | Kept | Preserves visual quality when UI work matters. |
| `frontend-patterns` | Kept | Common React/Next.js implementation guidance. |
| `product-lens` | Kept | Lightweight product scoping without a full PRD lane. |
| `search-first` | Kept | Encourages local and upstream research before building. |
| `security-review` | Kept | Needed for auth, input, secrets, payments, and sensitive data. |
| `tdd-workflow` | Kept | Useful when tests can drive or protect behavior. |
| `verification-loop` | Kept | Final evidence gate for commands, diff, and residual risk. |

## Removed Codex Team Skills

| Skill | Reason |
| --- | --- |
| `ai-regression-testing` | Too niche for the default bundle; overlaps with QA plus `verification-loop`. |
| `blueprint` | Heavy multi-session planning lane; not needed for the compact workflow. |
| `council` | Adds role ceremony for rare tradeoff calls; main-thread judgment is enough. |
| `gateguard` | Duplicates the core "read before editing" rule and slows routine work. |
| `hexagonal-architecture` | Specialized architecture pattern; use `backend-patterns` and `api-design` instead. |
| `product-capability` | PRD-to-SRS lane is too heavy for the default repo scope. |
| `safety-guard` | Duplicates Codex approval/sandbox/worklog rules in this repo. |
| `team-builder` | Redundant now that core instructions define when to spawn specialists. |

## Kept Stitch Skills

| Skill | Status | Review Notes |
| --- | --- | --- |
| `design-md` | Kept | Useful for synthesizing `.stitch/DESIGN.md` from actual design context. |
| `enhance-prompt` | Kept | Lightweight prompt improvement for Stitch work. |
| `react:components` | Kept | Canonical Stitch-to-React conversion path. |
| `stitch-design` | Kept | Main Stitch design entry point; falls back to local prompts/design docs when MCP is unavailable. |

## Removed Stitch Skills

| Skill | Reason |
| --- | --- |
| `remotion` | Video walkthrough output is niche and not part of normal software delivery. |
| `shadcn-ui` | Framework-specific and only useful in projects that already choose shadcn/ui. |
| `stitch-loop` | Multi-page autonomous site loop is broader than the compact default workflow. |
| `taste-design` | Highly opinionated design system layer; keep frontend design judgment local to the task. |

## Ongoing Rules

- Add a skill only when it removes repeated real work or prevents a repeated
  failure mode.
- Prefer one broad, reliable skill over several overlapping niche skills.
- Keep role routing in `AGENTS.md`, `global/AGENTS.md`, and
  `global/agents/*.toml` aligned with the installed skill set.
- After any skill add/remove, run `bin/sync-codex-runtime ~/.codex` and verify
  repo/runtime skill counts.
