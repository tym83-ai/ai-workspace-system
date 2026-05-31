# Bootstrap AI Workspace

[English version](BOOTSTRAP.md)

Используйте этот файл при настройке новой машины с Codex или Claude Code.

Репозиторий универсальный. Перед bootstrap дайте агенту:

- URL этого репозитория;
- опциональную GitHub-организацию или user account для owned repos и private mirrors;
- желаемый локальный project root, обычно `~/projects`.

## Стартовый prompt

```text
Настрой мой AI workspace на этой машине.

Repository URL for the workspace system:
<ai-workspace-system-repo-url>

GitHub organization or user for owned repos and private mirrors:
<github-org-or-user, or leave empty if disabled>

Local project root:
~/projects

Сначала клонируй workspace system в:
~/projects/ai-workspace-system

Затем прочитай:
~/projects/ai-workspace-system/docs/ru/new-machine-setup.md
~/projects/ai-workspace-system/prompts/ru/discover-and-normalize-local-projects.md
~/projects/ai-workspace-system/prompts/ru/bootstrap-new-machine-codex.md
~/projects/ai-workspace-system/prompts/ru/bootstrap-new-machine-claude.md

Следуй инструкциям.

Правила:
- Не загружай секреты.
- Не удаляй локальные файлы.
- Не делай force-push.
- Не коммить dirty repositories автоматически.
- Не включай scheduled sync, пока не покажешь, что будет включено.
- Не добавляй runtime state Codex и Claude в Git.
- Используй AGENTS.md как канонический файл общих проектных инструкций.
- Используй CLAUDE.md как мост Claude Code, который импортирует @AGENTS.md.
- Спрашивай перед изменением remotes для upstream или mirror repositories.

В конце покажи:
- что было склонировано;
- что было установлено;
- какие repositories подключены к GitHub;
- что осталось только локально;
- что требует ручной проверки.
```

## Минимальная ручная установка

```bash
mkdir -p ~/projects
git clone <ai-workspace-system-repo-url> ~/projects/ai-workspace-system
~/projects/ai-workspace-system/bin/install-local
workspace-configure
```

Проверьте конфиг:

```bash
sed -n '1,120p' ~/.config/ai-workspace/config
```
