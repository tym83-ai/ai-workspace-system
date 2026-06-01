# Архитектура

[English version](../architecture.md)

## Локальная структура

```text
~/projects/
  ai-workspace-system/   # этот проект
  _agent-assets/         # опциональные reusable assets Codex/Claude
  _artifacts/            # вспомогательные материалы, не source repos
  <project>/             # реальные проекты, mirrors и workbenches
```

Launcher показывает проекты из `PROJECT_ROOTS`.
Scheduled sync трогает только `SYNC_ROOTS`.
Оба значения лежат в локальном конфиге, который пишет `workspace-configure`.
GitHub-to-local hydration клонирует недостающие repositories из `GITHUB_ORG` в `PROJECTS_HOME`.
Helper-команды устанавливаются symlink'ами в `LOCAL_BIN_DIR`; `workspace-shell-setup` добавляет эту директорию в shell startup files через idempotent managed block.

## Типы проектов

Target repositories и delivery workbenches разделяются:

```text
target repo          проверенный код/site/docs для реального репозитория
delivery workbench   prompts, research, briefs, raw inputs, drafts, pipeline state
```

Используйте `workspace.yaml`, чтобы связать workbench с target repos. Target repo не должен содержать приватные prompts, session logs, raw generation pipelines или runtime state агента.

## GitHub Layout

Рекомендуемые private repositories в организации или аккаунте:

- `ai-workspace-system`
- `ai-workspace-private` или `codex-assets`
- `dotfiles`
- один repository на реальный проект
- опционально `project-template`

Если `GITHUB_SYNC_REPOS=1`, `workspace-sync` использует настроенную GitHub organization/account как еще один discovery source:

- получает список repositories через `gh repo list`;
- клонирует недостающие repositories в `PROJECTS_HOME`;
- пропускает уже существующие paths;
- не перезаписывает существующие remotes;
- затем запускает обычную локальную sync policy по `SYNC_ROOTS`.

## Mirror Repositories

Часть локальных проектов не принадлежит вашей private organization: public/private upstream sites, vendor repos или customer repos.

Рекомендуемый remote pattern:

```text
origin   canonical upstream repository
backup   private organization mirror
```

Sync policy:

- pull из `origin`;
- automatic push committed working branches в `backup`;
- push working branches в `origin` только по явной команде;
- никогда не заменять `origin` молча.

## Загрузка инструкций

Codex:

- global: `~/.codex/AGENTS.md`
- project: `AGENTS.md`
- optional: nested `AGENTS.override.md`

Claude Code:

- global: `~/.claude/CLAUDE.md`
- project: `CLAUDE.md` или `.claude/CLAUDE.md`
- project rules: `.claude/rules/*.md`
- recommended bridge: `CLAUDE.md` imports `@AGENTS.md`

## Scheduled Sync

Используйте user-level systemd timer:

- 13:00
- 19:00
- 23:50

Scheduled job запускает `workspace-sync --scheduled`.
