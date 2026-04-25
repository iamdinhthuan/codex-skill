# Skill Review

Review date: 2026-04-24

Scope:
- `skills/codex-team-skills/`
- `skills/stitch-skills/`
- global install paths under `~/.codex/skills/`

## Summary

The curated `everything-claude-code` bundle has been renamed to
`codex-team-skills` and reviewed for Codex fit. The main required changes were
runtime assumptions: Claude-specific paths, `claude agents`, PreToolUse hooks,
Context7-only docs lookup, and Claude-branded examples.

The Stitch bundle stays named `stitch-skills` because it is a product/source
name, not a Claude-specific name.

## Codex Team Skills

| Skill | Status | Review Notes |
| --- | --- | --- |
| `accessibility` | OK | Generic WCAG 2.2 guidance; no Codex-specific changes needed beyond neutral origin metadata. |
| `ai-regression-testing` | Updated | Changed trigger language from named AI agents to generic AI coding agent. The `.claude/commands` example is illustrative only and does not affect runtime. |
| `api-design` | OK | Generic REST API guidance; no Codex-specific changes needed. |
| `backend-patterns` | OK | Generic backend patterns; no Codex-specific changes needed. |
| `blueprint` | Updated | Replaced Claude/ECC install assumptions with repo-local `skills/codex-team-skills/...` guidance. |
| `browser-qa` | Updated | Replaced `claude-in-chrome` preference with Playwright MCP / Browserbase / direct browser automation guidance for Codex. |
| `coding-standards` | Updated | Replaced "narrower ECC skill" wording with "narrower Codex team skill". |
| `council` | Updated | Changed "in-context Claude voice" and ECC example language to Codex team wording. |
| `database-migrations` | OK | Generic migration guidance; no Codex-specific changes needed. |
| `documentation-lookup` | Updated | Removed Context7-only assumption; now uses available docs MCPs, official docs, local docs, or official web sources. |
| `e2e-testing` | OK | Generic Playwright guidance; no Codex-specific changes needed. |
| `frontend-design` | OK | Generic frontend design guidance; compatible with Codex frontend rules. |
| `frontend-patterns` | OK | Generic React/Next.js guidance; no Codex-specific changes needed. |
| `gateguard` | Updated | Replaced Claude PreToolUse hook assumption with a Codex pre-action checklist workflow. |
| `hexagonal-architecture` | OK | Generic architecture guidance; no Codex-specific changes needed. |
| `product-capability` | Updated | Replaced ECC-native wording with Codex team lane wording. |
| `product-lens` | OK | Generic product diagnosis guidance; no Codex-specific changes needed. |
| `safety-guard` | Updated | Replaced PreToolUse hook/runtime log assumptions with Codex approval/sandbox/worklog guidance. |
| `search-first` | Updated | Replaced `~/.claude` paths, Claude skill wording, Context7-only SDK docs, and generic Task call with Codex paths and `spawn_agent` guidance. |
| `security-review` | OK | Generic security review guidance; no Codex-specific changes needed. |
| `tdd-workflow` | OK | Generic testing workflow; no Codex-specific changes needed. |
| `team-builder` | Updated | Replaced `claude agents` discovery with runtime roles from `~/.codex/config.toml`, optional personas from `./agents` and `~/.codex/agents`, built-in Codex team roles, and explicit single-writer parallel dispatch rules. |
| `verification-loop` | Updated | Rebranded from Claude Code sessions to Codex sessions. |

## Stitch Skills

| Skill | Status | Review Notes |
| --- | --- | --- |
| `design-md` | OK | Purpose-built for Stitch design-system docs. Requires Stitch project context or local screenshots. |
| `enhance-prompt` | OK | Useful before Stitch generation/editing. No local runtime assumptions. |
| `react:components` | OK with constraint | Fits Stitch-to-React conversion. Requires Stitch MCP metadata or local `.stitch/designs` files; validation depends on its local package scripts. |
| `remotion` | Optional | Useful only when the user asks for walkthrough/video output. Keep out of normal frontend work. |
| `shadcn-ui` | Conditional | Use only if the target React project already uses or requests shadcn/ui. |
| `stitch-design` | OK | Main Stitch design entry point. Requires Stitch MCP for generation/edit flows; otherwise use prompt/design-doc portions only. |
| `stitch-loop` | Conditional | Use only for multi-page site generation from one prompt; broader scope than normal component/page edits. |
| `taste-design` | OK | Useful design-system guardrail for avoiding generic AI UI; should be balanced with existing project design systems. |

## Project-Level Review

Findings:
- The repo now has a clear global/local split: global behavior lives in
  `~/.codex/AGENTS.md`, while this repo stores source-controlled prompts,
  vendored skills, bootstrap docs, and work logs.
- `docs/WORKLOG.md` gives future conversations durable task state. Keep it
  concise; do not turn it into a transcript.
- The renamed `skills/codex-team-skills/` path is clearer for Codex users while
  preserving upstream attribution.
- `skills/stitch-skills/` is correctly kept separate because it is sourced from
  Google Stitch, not the Codex team skill bundle.

Risks:
- Vendored skills can drift from upstream. Update only from reviewed pinned
  commits and document the new source commit.
- Global files under `~/.codex` are not source-controlled by this repo. When
  changing global behavior, mirror the durable explanation in `docs/`.
- `SKILL.md` is deleted in the current worktree but unrelated to this scope; do
  not stage it unless the user explicitly asks.

Recommendation:
- Keep `codex-team-skills` as the bundle name.
- Keep `stitch-skills` as the Stitch source bundle name.
- Treat `docs/SKILL_REVIEW.md` as the checklist to update whenever adding,
  removing, renaming, or adapting skills.
