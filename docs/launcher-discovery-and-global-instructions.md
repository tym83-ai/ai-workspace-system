# Launcher, Project Discovery, And Global Instructions

This document defines the first-run workflow that makes the workspace system useful on real machines.

## Launcher Workflow

Use the launcher instead of starting agents directly:

```bash
ai-launch codex
ai-launch claude
```

The launcher always shows:

1. `New project`
2. `No project`
3. discovered projects from `PROJECT_ROOTS`

`New project` calls `workspace-new-project <name>`.
That command creates the project under `PROJECTS_HOME`, writes baseline `README.md`, `AGENTS.md`, `CLAUDE.md`, `workspace.yaml`, `.gitignore`, and `.env.example`, then runs:

```bash
git init -b main
git add README.md AGENTS.md CLAUDE.md workspace.yaml .gitignore .env.example
git commit -m "Initial project scaffold"
```

If `GITHUB_ORG` is set and `gh` is authenticated, it can also create and push a private GitHub repository.

`No project` starts the selected agent from `$HOME`. Use it for one-off questions and work that should not attach to a project.

## Discover Existing Local Projects

Use [Discover And Normalize Local Projects](../prompts/discover-and-normalize-local-projects.md) when a machine already has scattered projects.

The prompt is part of the workspace system. It asks the agent to:

- inventory likely project directories
- classify each candidate
- create the configured project root if missing
- propose a move plan before moving files
- add baseline docs and instructions
- initialize Git only for real projects
- preserve existing remotes and branches
- avoid secrets, caches, sessions, logs, auth state, and runtime state

## Global Agent Files Policy

Global files are machine-level preferences, not project source.

Typical files:

- `~/.codex/AGENTS.md`
- `~/.claude/CLAUDE.md`
- `~/.claude/rules/*.md`

Agents must not blindly overwrite them.

Policy:

- If a global file is missing, the agent may propose creating it.
- If a global file exists, the agent should read it, preserve existing content, and propose a small additive patch.
- If automated editing is explicitly approved, append or update a clearly marked section instead of replacing the file.
- Never commit global runtime state, sessions, auth files, logs, sqlite state, or shell history.
- Keep reusable public instructions in project docs; keep personal machine preferences in local config or global agent files.
