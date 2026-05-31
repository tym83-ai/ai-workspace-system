# GitHub Organization Plan

Use the dedicated private GitHub organization `tym83-ai` for workspace infrastructure and local projects.

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

For repos like `cozystack/website` or `aenix.io`, do not collapse ownership.

Use explicit remotes:

```bash
git remote -v
git remote add backup git@github.com:<org>/<repo>-mirror.git
```

Keep `origin` pointed at the canonical upstream unless the project is truly being transferred.

## Data Policy

Before mirroring, classify the repo:

- safe source repo
- private customer repo
- generated content repo
- contains secrets or private data
- too large for Git without LFS

Only mirror repos that are safe for the private organization.
