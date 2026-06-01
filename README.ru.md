# AI Workspace System

[English version](README.md)

`AI Workspace System` - переносимое локальное рабочее пространство для Codex, Claude Code и GitHub.

Проект специально сделан универсальным: пути конкретной машины, GitHub-организация, пути до бинарников и команда secret scanning хранятся в локальном конфиге, а не в репозитории.

Для новой машины начните с [BOOTSTRAP.ru.md](BOOTSTRAP.ru.md).

## Цели

- один настраиваемый корень проектов, обычно `~/projects`;
- интерактивный launcher для Codex и Claude Code;
- консервативная синхронизация всех управляемых Git-репозиториев;
- разделение target repos и delivery workbenches;
- опциональный scheduled sync в 13:00, 19:00 и 23:50;
- общие глобальные и проектные инструкции;
- reusable bootstrap prompts для новых машин;
- опциональная синхронизация agent assets через настроенную GitHub-организацию или аккаунт;
- first-run prompt для обнаружения разбросанных локальных проектов и нормализации workspace.

## Команды

- `workspace-configure` - интерактивно пишет локальный конфиг в `~/.config/ai-workspace/config`.
- `workspace-shell-setup` - добавляет `LOCAL_BIN_DIR` в shell startup files через managed PATH block.
- `ai-launch codex` - выбрать проект и запустить Codex.
- `ai-launch claude` - выбрать проект и запустить Claude Code.
- `workspace-sync` - клонировать недостающие GitHub repos, затем pull/push только чистых репозиториев.
- `workspace-sync --commit` - интерактивно закоммитить dirty repos перед sync.
- `workspace-sync --scheduled` - безопасный scheduled mode: без auto-commit и без push во внешний `origin`.
- `workspace-sync --clone-only` - подтянуть на эту машину недостающие repos из `GITHUB_ORG` и выйти.
- `workspace-new-project <name>` - создать новый проект в настроенном project root.
- `workspace-ensure-backup --push` - добавить приватный `backup` remote для upstream-репозиториев.

## Project Picker

Launcher всегда сначала показывает:

1. `New project`
2. `No project`

Затем он показывает найденные проекты из `PROJECT_ROOTS`.

Каталоги с `.git`, `workspace.yaml`, `AGENTS.md`, `CLAUDE.md`, `README.md` или типичными package-файлами считаются проектами.

`New project` создает настоящий Git-репозиторий с базовой документацией и initial commit.

`No project` запускает агента из `$HOME` для разовых задач без проектного контекста.

## Существующие локальные проекты

Если на машине уже есть разбросанные проекты, используйте:

```text
prompts/ru/discover-and-normalize-local-projects.md
```

Этот prompt инвентаризирует локальные каталоги, предлагает план переноса в настроенный project root, добавляет базовую документацию и инструкции, а Git инициализирует только для настоящих проектов.

## Глобальные агентские файлы

Система считает глобальные агентские файлы машинными предпочтениями:

- `~/.codex/AGENTS.md`
- `~/.claude/CLAUDE.md`
- `~/.claude/rules/*.md`

Агенты не должны затирать эти файлы. Если они уже существуют, агент должен сохранить содержимое и предложить или применить только additive changes после подтверждения.

## Sync Policy

Автоматический sync намеренно консервативный:

- клонирует недостающие repositories из `GITHUB_ORG` в `PROJECTS_HOME`;
- fetch remotes;
- pull только чистых worktrees с upstream branches;
- push только уже закоммиченных локальных изменений;
- push внешних/upstream repositories только в приватный `backup` remote;
- dirty repos пропускаются, если не используется `--commit`;
- force-push не используется;
- scheduled mode никогда не делает auto-commit.

Это защищает от случайной публикации секретов, сломанной работы, generated files и промежуточных правок.

## Конфигурация

Локальный конфиг:

```text
~/.config/ai-workspace/config
```

Создать или обновить:

```bash
workspace-configure
```

Шаблон:

```text
PROJECTS_HOME="$HOME/projects"
LOCAL_BIN_DIR="$HOME/.local/bin"
PROJECT_ROOTS="$HOME/projects"
SYNC_ROOTS="$HOME/projects"
GITHUB_ORG=""
GITHUB_SYNC_REPOS="1"
GITHUB_CLONE_PROTOCOL="https"
GITHUB_REPO_LIMIT="200"
BACKUP_REMOTE="backup"
CODEX_BIN="$HOME/.local/bin/codex"
CLAUDE_BIN="$HOME/.local/bin/claude"
SECRET_SCAN_CMD=""
```

`PROJECT_ROOTS` управляет списком в picker.
`SYNC_ROOTS` управляет scheduled sync.
`LOCAL_BIN_DIR` задает директорию для symlink-команд, которые устанавливает `install-local`.
Если `ai-launch` или `workspace-*` не находятся в новой shell, запустите `workspace-shell-setup`.
`GITHUB_ORG` задавайте только там, где helpers должны создавать или использовать GitHub repositories.
Если `GITHUB_SYNC_REPOS=1`, `workspace-sync` сначала получает список repositories в `GITHUB_ORG` через `gh repo list` и клонирует недостающие в `PROJECTS_HOME`.
`GITHUB_CLONE_PROTOCOL=ssh` используйте только на машинах с настроенным SSH-доступом к GitHub; по умолчанию безопаснее оставить `https`.

`SECRET_SCAN_CMD` опционален. Если он задан, `workspace-sync --commit` запускает его перед commit и добавляет путь репозитория последним аргументом.

## Важная граница

Claude Code читает `CLAUDE.md`, а не `AGENTS.md`. Для общих проектных инструкций держите `AGENTS.md` каноническим, а `CLAUDE.md` делайте мостом:

```md
@AGENTS.md

## Claude Code

- Add Claude-specific notes here.
```

Codex автоматически читает `AGENTS.md`.

## Project Boundaries

Для подготовки изменений в другой репозиторий используйте два типа проектов:

- target repo: проверенный код, website content, public docs, PR branches;
- delivery workbench: prompts, research, briefs, raw inputs, generated drafts, pipeline state, agent assets.

Связывайте их через `workspace.yaml`. Агенты должны спрашивать перед переносом workbench output в target repo или push во внешний upstream.

## Документация

- [Архитектура](docs/ru/architecture.md)
- [План GitHub-организации](docs/ru/github-organization-plan.md)
- [GitHub remotes playbook](docs/ru/github-remotes-playbook.md)
- [Launcher, обнаружение проектов и глобальные инструкции](docs/ru/launcher-discovery-and-global-instructions.md)
- [Настройка новой машины](docs/ru/new-machine-setup.md)
- [Границы проектов](docs/ru/project-boundaries.md)
- [Рекомендации](docs/ru/recommendations.md)

## Prompts

- [Найти и нормализовать локальные проекты](prompts/ru/discover-and-normalize-local-projects.md)
- [Bootstrap новой машины для Codex](prompts/ru/bootstrap-new-machine-codex.md)
- [Bootstrap новой машины для Claude Code](prompts/ru/bootstrap-new-machine-claude.md)

## Лицензия

Apache License 2.0 - см. [LICENSE](LICENSE) и [NOTICE](NOTICE). Репозиторий, включая документацию, prompts, templates и config, распространяется под Apache-2.0.
