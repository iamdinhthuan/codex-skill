# Skill Review

Review date: 2026-05-02

Scope:
- `skills/codex-team-skills/`
- `skills/frontend-skills/`
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
- Keep 13 Codex team skills and 12 Taste frontend skills.
- Adopt only lightweight Khuym-style coordination invariants: validate the plan
  against actual repo files before writing, require structured handoff fields,
  and keep the parent thread tending spawned work until there is a real blocker
  or final result.

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
| `frontend-design` | Replaced by Taste Skill frontend design guidance. |
| `frontend-patterns` | Replaced by Taste Skill frontend implementation guidance plus project-local patterns. |

## Kept Taste Frontend Skills

| Skill | Status | Review Notes |
| --- | --- | --- |
| `design-taste-frontend` | Kept | Default premium frontend design and implementation taste guidance. |
| `gpt-taste` | Kept | Stricter Codex/GPT-oriented anti-generic layout and motion guidance. |
| `image-to-code` | Kept | Image-first website workflow for visually important frontend work. |
| `redesign-existing-projects` | Kept | Existing UI audit and redesign path. |
| `high-end-visual-design` | Kept | Softer premium visual direction when requested. |
| `minimalist-ui` | Kept | Restrained editorial/product UI direction. |
| `industrial-brutalist-ui` | Kept | Specific brutalist visual direction when requested. |
| `full-output-enforcement` | Kept | Guards against placeholder or partial UI output. |
| `stitch-design-taste` | Kept | Taste-compatible Stitch design-system guidance. |
| `imagegen-frontend-web` | Kept | Web reference image generation prompts. |
| `imagegen-frontend-mobile` | Kept | Mobile reference image generation prompts. |
| `brandkit` | Kept | Brand identity board generation prompts. |

## Removed Stitch Skills

| Skill | Reason |
| --- | --- |
| `design-md` | Replaced by `stitch-design-taste` and project-local `.stitch/DESIGN.md` guidance. |
| `enhance-prompt` | Replaced by Taste Skill prompt/design-system guidance. |
| `react:components` | Replaced by Taste Skill image-to-code and project-local frontend patterns. |
| `stitch-design` | Replaced by `stitch-design-taste`. |

## Ongoing Rules

- Add a skill only when it removes repeated real work or prevents a repeated
  failure mode.
- Prefer one broad, reliable skill over several overlapping niche skills.
- Keep role routing in `AGENTS.md`, `global/AGENTS.md`, and
  `global/agents/*.toml` aligned with the installed skill set.
- After any skill add/remove, run `bin/sync-codex-runtime ~/.codex` and verify
  repo/runtime skill counts.
