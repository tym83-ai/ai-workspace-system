# AI Workspace System

This project defines a portable local workspace for Codex, Claude Code, and GitHub.

It is intentionally generic: machine paths, GitHub organization names, binary paths, and secret-scanning commands belong in local config, not in this repository.

For new machines, start with [BOOTSTRAP.md](BOOTSTRAP.md).

## Goals

- one configurable local project root, usually `~/projects`
- one interactive launcher for Codex and Claude Code
- one conservative sync command for all managed Git repositories
- explicit separation between target repositories and delivery workbenches
- optional scheduled sync at 13:00, 19:00, and 23:50
- shared global and project-level instructions
- reusable bootstrap prompts for new machines
- optional syncing of reusable agent assets through a configured GitHub organization or user account

## Commands

- `workspace-configure` - interactively write local machine config to `~/.config/ai-workspace/config`.
- `ai-launch codex` - choose a project, then start Codex there.
- `ai-launch claude` - choose a project, then start Claude Code there.
- `workspace-sync` - pull/push clean repos only.
- `workspace-sync --commit` - interactively commit dirty repos before syncing.
- `workspace-sync --scheduled` - safe scheduled mode; never commits automatically and never pushes to external `origin` remotes.
- `workspace-new-project <name>` - create a new project under the configured project root.
- `workspace-ensure-backup --push` - add private `backup` remotes in the configured GitHub org/user for upstream repos.

## Project Picker

The launcher always offers these first:

1. `New project`
2. `No project`

Then it lists discovered projects from configured roots.

Directories with `.git`, `workspace.yaml`, `AGENTS.md`, `CLAUDE.md`, `README.md`, or common package files are treated as projects.

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

Create or update it with:

```bash
workspace-configure
```

Template:

```text
PROJECTS_HOME="$HOME/projects"
PROJECT_ROOTS="$HOME/projects"
SYNC_ROOTS="$HOME/projects"
GITHUB_ORG=""
BACKUP_REMOTE="backup"
CODEX_BIN="$HOME/.local/bin/codex"
CLAUDE_BIN="$HOME/.local/bin/claude"
SECRET_SCAN_CMD=""
```

Use `PROJECT_ROOTS` for what appears in the picker.
Use `SYNC_ROOTS` for what scheduled sync touches.
Set `GITHUB_ORG` to a GitHub organization or user account only on machines where helpers should create or use GitHub repositories.

`SECRET_SCAN_CMD` is optional. If set, `workspace-sync --commit` runs it before committing and appends the repository path as the final argument.

## Important Boundary

Claude Code reads `CLAUDE.md`, not `AGENTS.md`. For shared project instructions, keep `AGENTS.md` canonical and make `CLAUDE.md` import it with:

```md
@AGENTS.md

## Claude Code

- Add Claude-specific notes here.
```

Codex reads `AGENTS.md` automatically.

## Project Boundaries

Use two project types when preparing changes for another repo:

- target repo: owns reviewed code, website content, public docs, and PR branches
- delivery workbench: owns prompts, research, briefs, raw inputs, generated drafts, pipeline state, and agent assets

Connect them with `workspace.yaml`. Agents must ask before copying workbench output into a target repo or pushing to any external upstream.

## Docs

- [Architecture](docs/architecture.md)
- [GitHub Organization Plan](docs/github-organization-plan.md)
- [GitHub Remotes Playbook](docs/github-remotes-playbook.md)
- [New Machine Setup](docs/new-machine-setup.md)
- [Project Boundaries](docs/project-boundaries.md)
- [Recommendations](docs/recommendations.md)

## Prompts

- [Discover And Normalize Local Projects](prompts/discover-and-normalize-local-projects.md) - find scattered local projects, move approved ones into the configured project root, add baseline docs, and initialize Git where appropriate.
- [Bootstrap New Machine For Codex](prompts/bootstrap-new-machine-codex.md)
- [Bootstrap New Machine For Claude Code](prompts/bootstrap-new-machine-claude.md)
