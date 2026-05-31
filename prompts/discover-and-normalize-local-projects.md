# Prompt: Discover And Normalize Local Projects

[Русская версия](ru/discover-and-normalize-local-projects.md)

Use this prompt with Codex or Claude Code when a machine already has scattered local projects and you want to bring them into the AI Workspace System.

```text
You are working on my local machine.

Goal:
Find likely local projects, classify them, move safe projects into ~/projects, add baseline AI/project documentation, and initialize Git repositories where appropriate.

Read local config first if it exists:
~/.config/ai-workspace/config

Use PROJECTS_HOME as the target directory. If it is not configured, use ~/projects.

Rules:
- Do not upload secrets.
- Do not delete files.
- Do not move anything before showing me the move plan, unless I have explicitly approved this prompt as executable.
- Do not touch runtime state, auth, sessions, logs, caches, shell history, package caches, browser profiles, sqlite state, or agent conversation history.
- Do not move ~/.claude, ~/.codex, ~/.ssh, ~/.gnupg, ~/.config, ~/.local/state, or hidden service directories into Git.
- Do not force-push.
- Do not push to external/upstream origin remotes unless I explicitly ask.
- For external/upstream repos, preserve origin and use backup only after confirming the strategy.
- Keep target repos separate from delivery workbenches.
- Do not overwrite global agent files. If ~/.codex/AGENTS.md, ~/.claude/CLAUDE.md, or ~/.claude/rules already exist, preserve them and propose additive changes only.

Tasks:
1. Inventory likely projects under my home directory and configured PROJECT_ROOTS.
2. Classify each candidate as:
   - owned project
   - target repo
   - delivery workbench
   - research workbench
   - integration workbench
   - upstream/mirror repo
   - website/content repo
   - agent assets
   - artifact/archive
   - ignore
   - needs review
3. Save an inventory report under ~/projects/_reports.
4. Create ~/projects if missing.
5. For safe project directories outside ~/projects, propose a move plan into ~/projects.
6. After approval, move approved projects and leave symlinks at old paths when useful.
7. For project directories without Git, initialize Git only when the directory is a real project, not an artifact/cache/archive.
8. Before the first commit, add or update:
   - README.md
   - AGENTS.md
   - CLAUDE.md importing @AGENTS.md
   - .gitignore
   - .env.example if env vars are used
   - workspace.yaml when the project is a workbench or has target repos
9. Keep private prompts, raw research, generated drafts, pipeline runtime, and local agent state in workbench repos, not target repos.
10. For existing Git repos, preserve remotes and branches. Do not rewrite history.
11. For dirty repos, report status and ask before committing.
12. Run the narrowest useful verification command for changed projects.
13. For global agent files, create missing files only after approval. For existing files, report recommended additions instead of replacing the file.

Final answer:
- projects moved
- projects initialized as Git repos
- docs/instructions added
- projects skipped
- projects needing manual review
- sync strategy for owned repos and upstream/mirror repos
- exact commands for optional next steps
```
