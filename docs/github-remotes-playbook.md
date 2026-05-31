# GitHub Remotes Playbook

## New Owned Project

Use the workspace helper:

```bash
GITHUB_ORG=tym83-ai workspace-new-project my-project
```

This creates:

- `~/projects/my-project`
- local git repo
- baseline `README.md`, `AGENTS.md`, `CLAUDE.md`, `.gitignore`, `.env.example`
- private GitHub repo under the org, if `gh` is authenticated

## Existing Local Project Without Remote

From inside the project:

```bash
gh repo create tym83-ai/<repo-name> --private --source=. --remote=origin --push
```

Before running this:

```bash
git status
~/projects/_codex/scripts/audit-secrets .
```

## Existing Owned Project With Personal Remote

If the project belongs in the org, prefer GitHub repository transfer.

Alternative:

```bash
git remote rename origin personal
git remote add origin git@github.com:<org>/<repo-name>.git
git push -u origin <branch>
```

Do this only after confirming the old remote does not need to remain canonical.

## Upstream Or Mirror Repository

For projects like website mirrors, vendor repos, customer repos, or upstream-owned repos:

```bash
git remote -v
git remote add backup git@github.com:tym83-ai/<repo-name>-mirror.git
```

Policy:

- keep `origin` as the canonical upstream
- use `backup` for private backup/mirror only
- do not push to `origin` from scheduled automation unless that is the project workflow
- do not mirror repos that contain customer data, secrets, generated dumps, or unclear ownership

## Sync Roots

`~/.config/ai-workspace/config` separates discovery from sync:

```text
PROJECT_ROOTS="$HOME/projects:$HOME/claude"
SYNC_ROOTS="$HOME/projects"
```

Use `PROJECT_ROOTS` to show projects in the picker.
Add a root to `SYNC_ROOTS` only when you are comfortable with scheduled fetch/pull/push for every repo under it.

Current default:

```text
PROJECT_ROOTS="$HOME/projects:$HOME/claude:$HOME/Загрузки/cozyportal-demo"
SYNC_ROOTS="$HOME/projects:$HOME/claude:$HOME/Загрузки/cozyportal-demo"
```

External `origin` remotes are treated as read-mostly upstreams. `workspace-sync` may fetch/pull from them, but it pushes only to `backup`.

## Create Backup Remotes

For all repositories under `SYNC_ROOTS`:

```bash
workspace-ensure-backup --push
```

For one repository:

```bash
workspace-ensure-backup --push ~/projects/aenix.io
```

This creates a private repo in `tym83-ai` named:

```text
mirror-<source-owner>-<source-repo>
```

and adds it as:

```text
backup https://github.com/tym83-ai/mirror-<source-owner>-<source-repo>.git
```

After that, scheduled sync pushes committed local state to `backup`, not to the external `origin`.
