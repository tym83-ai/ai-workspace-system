# AI Workspace System

This project defines the local multi-machine workspace used by Codex, Claude Code, and GitHub.

For new machines, start with [BOOTSTRAP.md](BOOTSTRAP.md).

The goal is simple:

- one canonical local project root: `~/projects`
- one interactive launcher for Codex and Claude Code
- one conservative sync command for all Git repositories
- scheduled sync at 13:00, 19:00, and 23:50
- shared global and project-level instructions
- reusable prompts for bootstrapping a new machine
- reusable Codex/Claude assets synced through a private GitHub organization

## Commands

- `ai-launch codex` - choose a project, then start Codex there.
- `ai-launch claude` - choose a project, then start Claude Code there.
- `workspace-sync` - pull/push clean repos only.
- `workspace-sync --commit` - interactively commit dirty repos before syncing.
- `workspace-sync --scheduled` - safe scheduled mode; never commits automatically and never pushes to external `origin` remotes.
- `workspace-new-project <name>` - create a new project under `~/projects`.
- `workspace-ensure-backup --push` - add private `backup` remotes in `tym83-ai` for upstream repos.

## Project Picker

The launcher always offers these first:

1. `New project`
2. `No project`

Then it lists discovered projects from configured roots.

`No project` starts the agent from `$HOME`, useful for one-off questions or work that does not need repository context.

## Sync Policy

Automated sync is intentionally conservative:

- fetches remotes
- pulls only clean worktrees with upstream branches
- pushes only committed local changes
- pushes external/upstream repositories only to their private `backup` remotes
- skips dirty repos unless `--commit` is used interactively
- never force-pushes
- never auto-commits in scheduled mode

This avoids accidental uploads of secrets, broken work, generated files, and half-finished edits.

## Configuration

Local config lives at:

```text
~/.config/ai-workspace/config
```

Template:

```text
PROJECT_ROOTS="$HOME/projects:$HOME/claude"
SYNC_ROOTS="$HOME/projects"
GITHUB_ORG=""
CODEX_BIN="$HOME/.local/bin/codex"
CLAUDE_BIN="$HOME/.local/bin/claude"
```

Use `PROJECT_ROOTS` for what appears in the picker.
Use `SYNC_ROOTS` for what scheduled sync touches.

## Important Boundary

Claude Code reads `CLAUDE.md`, not `AGENTS.md`. For shared project instructions, keep `AGENTS.md` canonical and make `CLAUDE.md` import it with:

```md
@AGENTS.md

## Claude Code

- Add Claude-specific notes here.
```

Codex reads `AGENTS.md` automatically.

## Docs

- [Architecture](docs/architecture.md)
- [GitHub Organization Plan](docs/github-organization-plan.md)
- [GitHub Remotes Playbook](docs/github-remotes-playbook.md)
- [New Machine Setup](docs/new-machine-setup.md)
- [Recommendations](docs/recommendations.md)
