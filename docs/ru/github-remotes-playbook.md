# GitHub Remotes Playbook

[English version](../github-remotes-playbook.md)

## Новый owned project

Сначала настройте машину:

```bash
workspace-configure
```

Затем используйте helper:

```bash
workspace-new-project my-project
```

Он создает:

- `<projects-home>/my-project`
- локальный git repo
- базовые `README.md`, `AGENTS.md`, `CLAUDE.md`, `.gitignore`, `.env.example`
- private GitHub repo в `GITHUB_ORG`, если `GITHUB_ORG` задан и `gh` авторизован

## Существующий локальный проект без remote

Изнутри проекта:

```bash
gh repo create <github-org-or-user>/<repo-name> --private --source=. --remote=origin --push
```

Перед запуском:

```bash
git status
workspace-sync --dry-run
```

Если настроен `SECRET_SCAN_CMD`, используйте `workspace-sync --commit` для interactive commit flow с pre-commit scan.

## Существующий owned project с personal remote

Если проект должен жить в организации, предпочтителен transfer репозитория на GitHub.

Альтернатива:

```bash
git remote rename origin personal
git remote add origin git@github.com:<github-org-or-user>/<repo-name>.git
git push -u origin <branch>
```

Делайте это только после подтверждения, что старый remote не должен оставаться canonical.

## Upstream или mirror repository

Для website mirrors, vendor repos, customer repos или upstream-owned repos:

```bash
git remote -v
git remote add backup git@github.com:<github-org-or-user>/<repo-name>-mirror.git
```

Policy:

- keep `origin` as canonical upstream;
- use `backup` for private backup/mirror only;
- do not push to `origin` from scheduled automation unless that is the project workflow;
- do not mirror repos with customer data, secrets, generated dumps, or unclear ownership.

## Sync Roots

`~/.config/ai-workspace/config` разделяет discovery и sync:

```text
PROJECT_ROOTS="$HOME/projects"
SYNC_ROOTS="$HOME/projects"
```

`PROJECT_ROOTS` управляет project picker.
Добавляйте root в `SYNC_ROOTS` только если scheduled fetch/pull/push безопасен для всех repos внутри.

External `origin` remotes считаются read-mostly upstreams. `workspace-sync` может fetch/pull из них, но push делает только в `backup`.

## Создание backup remotes

Для всех repos в `SYNC_ROOTS`:

```bash
workspace-ensure-backup --push
```

Для одного repo:

```bash
workspace-ensure-backup --push ~/projects/example-site
```

Это создает private repo в настроенной GitHub org/account:

```text
mirror-<source-owner>-<source-repo>
```

и добавляет remote:

```text
backup https://github.com/<github-org-or-user>/mirror-<source-owner>-<source-repo>.git
```

После этого scheduled sync пушит committed local state в `backup`, а не во внешний `origin`.
