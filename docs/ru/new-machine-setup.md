# Настройка новой машины

[English version](../new-machine-setup.md)

## Ручной bootstrap

1. Установите `git`, `gh`, Codex и Claude Code.
2. Авторизуйте GitHub:

   ```bash
   gh auth login
   ```

3. Склонируйте workspace system:

   ```bash
   mkdir -p ~/projects
   git clone <ai-workspace-system-repo-url> ~/projects/ai-workspace-system
   ```

4. Установите scripts:

   ```bash
   ~/projects/ai-workspace-system/bin/install-local
   ```

5. Настройте машину:

   ```bash
   workspace-configure
   sed -n '1,120p' ~/.config/ai-workspace/config
   ```

6. Включайте scheduled sync только после проверки config и dry-run:

   ```bash
   workspace-sync --dry-run
   systemctl --user daemon-reload
   systemctl --user enable --now ai-workspace-sync.timer
   ```

## Первый запуск агента

Используйте:

```bash
ai-launch codex
ai-launch claude
```

Опциональные aliases:

```bash
alias codex='ai-launch codex'
alias claude='ai-launch claude'
```

Не добавляйте aliases, пока не убедитесь, что launcher работает правильно.

Launcher показывает `New project`, `No project`, затем discovered projects.
`New project` создает Git repository с baseline docs и initial commit.
`No project` стартует из `$HOME` для разовых задач.

## Нормализация существующих локальных проектов

Если на машине уже есть разбросанные проекты, дайте агенту:

```text
~/projects/ai-workspace-system/prompts/ru/discover-and-normalize-local-projects.md
```

Этот prompt инвентаризирует локальные каталоги, предлагает move plan в configured project root, добавляет baseline docs/instructions и инициализирует Git только для real projects.

## Глобальные агентские файлы

Глобальные файлы Codex и Claude должны сохраняться.
Если их нет, агенты могут предложить создать их.
Если они уже существуют, агенты должны прочитать их и применять только additive changes после подтверждения.

Не перемещайте и не коммитьте global runtime state, sessions, auth files, logs, sqlite state, shell history или локальные machine secrets.
