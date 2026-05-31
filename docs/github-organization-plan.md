# GitHub Organization Plan

[Русская версия](ru/github-organization-plan.md)

Use a dedicated GitHub organization or user account for workspace infrastructure, owned projects, and optional private mirrors.

This value is local configuration. Set it with `workspace-configure`; do not hardcode it in the repository.

## Repository Classes

## Workspace Infrastructure

- `ai-workspace-system` - scripts, prompts, docs, systemd units.
- `ai-workspace-private` - sanitized Codex/Claude assets, prompts, skills, reusable instructions.
- `dotfiles` - shell/editor/git config, if desired.
- `project-template` - minimal repo scaffold.

## Real Projects

One repo per project:

- application code
- websites
- research repos
- local tools
- automation pipelines

## Mirror/Upstream Projects

For upstream-owned repos, vendor repos, website mirrors, or customer repos, do not collapse ownership.

Use explicit remotes:

```bash
git remote -v
git remote add backup git@github.com:<github-org-or-user>/<repo>-mirror.git
```

Keep `origin` pointed at the canonical upstream unless the project is truly being transferred.

## Data Policy

Before mirroring, classify the repo:

- safe source repo
- private customer repo
- generated content repo
- contains secrets or private data
- too large for Git without LFS

Only mirror repos that are safe for the configured private organization or account.
