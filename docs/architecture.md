# Architecture

[Русская версия](ru/architecture.md)

## Local Layout

```text
~/projects/
  ai-workspace-system/   # this project
  _agent-assets/         # optional reusable Codex/Claude assets
  _artifacts/            # supporting materials, not source repos
  <project>/             # real projects, repo mirrors, and delivery workbenches
```

The launcher lists projects from `PROJECT_ROOTS`.
Scheduled sync includes only `SYNC_ROOTS`.
Both values are local machine config written by `workspace-configure`.

## Project Types

Target repositories and delivery workbenches are separate:

```text
target repo          reviewed code/site/docs intended for a real repository
delivery workbench   prompts, research, briefs, raw inputs, generated drafts, pipeline state
```

Use `workspace.yaml` to connect a workbench to its targets. A target repo should not contain private prompts, local session logs, raw generation pipelines, or agent runtime state.

## GitHub Layout

Recommended private organization/account repositories:

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
