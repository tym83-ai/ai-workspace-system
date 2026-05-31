# Рекомендации

[English version](../recommendations.md)

## Держите auto sync консервативным

Scheduled sync не должен auto-commit. Используйте его для fetch, pull clean repos и push уже закоммиченной работы.

Interactive commit mode запускается вручную:

```bash
workspace-sync --commit
```

## Позже добавьте project manifest

Manifest делает поведение явным:

```yaml
projects:
  - name: example-site
    path: ~/projects/example-site
    type: website
    sync: backup
  - name: vendor-docs
    path: ~/projects/vendor-docs
    type: upstream-mirror
    sync: manual
```

Это лучше, чем постоянно угадывать policy по путям.

## Отделяйте source sync от backup

GitHub sync - не полноценный backup. Используйте `restic` или `borg` для:

- large artifacts
- generated outputs
- documents
- files not committed yet
- private local data

## Спрашивать перед изменением upstream strategy

Для repositories, где `origin` не принадлежит configured `GITHUB_ORG`, агенты должны спрашивать strategy перед изменением remotes или push в upstream.

Default:

- keep `origin` as canonical upstream;
- add `backup` under configured GitHub org/account;
- scheduled sync pushes to `backup`;
- upstream PRs/commits happen only after explicit instruction;
- exclude deep nested clones such as pipeline workspaces unless explicitly promoted to a managed project.

## Secret scanning перед organization push

Настройте optional scanner:

```text
SECRET_SCAN_CMD="gitleaks detect --source"
```

Затем используйте:

```bash
workspace-sync --commit
```

Sync script добавляет путь репозитория к `SECRET_SCAN_CMD`.

## Держите инструкции компактными

Используйте:

- global files для personal working style;
- project files для build/test/project rules;
- rules/skills для длинных topic-specific guidance.

Большие always-loaded instruction files ухудшают качество контекста.
