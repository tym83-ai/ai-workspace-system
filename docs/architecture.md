# Architecture

## Local Layout

```text
~/projects/
  ai-workspace-system/   # this project
  _codex/                # reusable Codex assets
  _artifacts/            # supporting materials, not source repos
  <project>/             # real projects and repo mirrors

~/claude/                # optional legacy Claude project root
```

The launcher can list projects from both `~/projects` and `~/claude`.
Scheduled sync includes `~/projects`, `~/claude`, and selected legacy paths such as `~/Загрузки/cozyportal-demo`.

## GitHub Layout

Recommended private organization repositories:

- `ai-workspace-system`
- `ai-workspace-private` or `codex-assets`
- `dotfiles`
- one repository per real project
- optional `project-template`

## Mirror Repositories

Some local projects are not owned by the private organization, for example public/private upstream sites or customer repositories.

Recommended remote pattern:

```text
origin   canonical upstream repository
backup   private organization mirror
```

Sync policy:

- pull from `origin`
- push committed working branches to `backup` automatically
- push working branches to `origin` only when explicitly requested for that project
- never replace `origin` silently

## Instruction Loading

Codex:

- global: `~/.codex/AGENTS.md`
- project: `AGENTS.md`
- optional: nested `AGENTS.override.md`

Claude Code:

- global: `~/.claude/CLAUDE.md`
- project: `CLAUDE.md` or `.claude/CLAUDE.md`
- project rules: `.claude/rules/*.md`
- recommended bridge: `CLAUDE.md` imports `@AGENTS.md`

## Scheduled Sync

Use a user-level systemd timer:

- 13:00
- 19:00
- 23:50

The scheduled job runs `workspace-sync --scheduled`.
