# Client Setup

Use this guide to make another machine read this repo and install the same
Codex team workflow, skills, Stitch design support, and project work-log
bootstrap.

## 1. Clone The Repo

```bash
git clone https://github.com/iamdinhthuan/codex-skill.git ~/codex_skill
cd ~/codex_skill
```

If the Stitch integration PR has not been merged yet, use the active branch:

```bash
git fetch origin codex/add-stitch-skills
git switch codex/add-stitch-skills
```

## 2. Install Into Codex Home

The install copies shared instructions, skill bundles, templates, and the
bootstrap helper into `~/.codex`.

```bash
mkdir -p ~/.codex/skills ~/.codex/templates ~/.codex/bin

cp global/AGENTS.md ~/.codex/AGENTS.md

rm -rf ~/.codex/skills/codex-team-skills ~/.codex/skills/stitch-skills
cp -R skills/codex-team-skills ~/.codex/skills/codex-team-skills
cp -R skills/stitch-skills ~/.codex/skills/stitch-skills

cp templates/PROJECT_WORKLOG.md ~/.codex/templates/PROJECT_WORKLOG.md
cp templates/PROJECT_AGENTS.md ~/.codex/templates/PROJECT_AGENTS.md
cp bin/codex-bootstrap-project ~/.codex/bin/codex-bootstrap-project
chmod +x ~/.codex/bin/codex-bootstrap-project
```

## 3. Configure MCP Servers

Codex can use the skills without MCP, but Stitch design generation requires a
configured Stitch MCP server.

Add or verify these entries in `~/.codex/config.toml`:

```toml
[features]
apps = true
multi_agent = true

[mcp_servers.playwright]
command = "npx"
args = ["-y", "@playwright/mcp@latest"]

[mcp_servers.stitch]
url = "https://stitch.googleapis.com/mcp"
env_http_headers = { "X-Goog-Api-Key" = "STITCH_API_KEY" }
```

Set the API key in the shell environment used to launch Codex:

```bash
export STITCH_API_KEY="your-key"
```

Do not commit secrets into this repo or into project files.

## 4. Verify Install

```bash
test -f ~/.codex/AGENTS.md
find ~/.codex/skills/codex-team-skills -maxdepth 2 -name SKILL.md | wc -l
find ~/.codex/skills/stitch-skills -maxdepth 2 -name SKILL.md | wc -l
~/.codex/bin/codex-bootstrap-project /tmp/codex-bootstrap-test
test -f /tmp/codex-bootstrap-test/docs/WORKLOG.md
test -f /tmp/codex-bootstrap-test/AGENTS.md
```

Expected counts:

- `codex-team-skills`: 23
- `stitch-skills`: 8

## 5. Daily Usage

For a new project, open Codex in that project normally. Codex should read
`~/.codex/AGENTS.md` automatically.

When meaningful work starts, Codex should:

1. Check for `docs/WORKLOG.md`.
2. Create it from `~/.codex/templates/PROJECT_WORKLOG.md` if missing.
3. Update the current task to `In Progress`.
4. Keep the work log current when task status changes.

Manual bootstrap is available:

```bash
cd /path/to/project
~/.codex/bin/codex-bootstrap-project
```

Use project-level `AGENTS.md` only for project-specific rules: commands,
architecture constraints, style rules, domain context, and deployment notes.

## 6. Updating A Client Machine

```bash
cd ~/codex_skill
git pull --ff-only

cp global/AGENTS.md ~/.codex/AGENTS.md
rm -rf ~/.codex/skills/codex-team-skills ~/.codex/skills/stitch-skills
cp -R skills/codex-team-skills ~/.codex/skills/codex-team-skills
cp -R skills/stitch-skills ~/.codex/skills/stitch-skills
cp templates/PROJECT_WORKLOG.md ~/.codex/templates/PROJECT_WORKLOG.md
cp templates/PROJECT_AGENTS.md ~/.codex/templates/PROJECT_AGENTS.md
cp bin/codex-bootstrap-project ~/.codex/bin/codex-bootstrap-project
chmod +x ~/.codex/bin/codex-bootstrap-project
```

After updating, rerun the verification commands above.

## 7. Troubleshooting

- If Codex does not follow the team workflow, verify `~/.codex/AGENTS.md`
  exists and restart the Codex session.
- If a skill is missing, verify the expected `SKILL.md` counts.
- If Stitch generation fails, verify `STITCH_API_KEY` is set and the Stitch MCP
  server is configured.
- If a project should not create local docs, tell Codex the task is read-only.
