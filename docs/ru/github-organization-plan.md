# План GitHub-организации

[English version](../github-organization-plan.md)

Используйте отдельную GitHub organization или user account для workspace infrastructure, owned projects и опциональных private mirrors.

Это значение является локальной конфигурацией. Задавайте его через `workspace-configure`; не хардкодьте его в репозитории.

## Классы репозиториев

## Workspace Infrastructure

- `ai-workspace-system` - scripts, prompts, docs, systemd units.
- `ai-workspace-private` - sanitized Codex/Claude assets, prompts, skills, reusable instructions.
- `dotfiles` - shell/editor/git config, если нужно.
- `project-template` - минимальный repo scaffold.

## Реальные проекты

Один repo на проект:

- application code
- websites
- research repos
- local tools
- automation pipelines

## Mirror/Upstream Projects

Для upstream-owned repos, vendor repos, website mirrors или customer repos не смешивайте ownership.

Используйте явные remotes:

```bash
git remote -v
git remote add backup git@github.com:<github-org-or-user>/<repo>-mirror.git
```

Держите `origin` направленным в canonical upstream, если проект реально не переносится.

## Data Policy

Перед mirroring классифицируйте repo:

- safe source repo
- private customer repo
- generated content repo
- contains secrets or private data
- too large for Git without LFS

Mirror делайте только для repos, безопасных для настроенной private organization или account.
