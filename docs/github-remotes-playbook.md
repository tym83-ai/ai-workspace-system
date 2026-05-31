# GitHub Remotes Playbook

## New Owned Project

Configure this machine first:

```bash
workspace-configure
```

Then use the workspace helper:

```bash
workspace-new-project my-project
```

This creates:

- `<projects-home>/my-project`
- local git repo
- baseline `README.md`, `AGENTS.md`, `CLAUDE.md`, `.gitignore`, `.env.example`
- private GitHub repo under `GITHUB_ORG`, if `GITHUB_ORG` is set and `gh` is authenticated

## Existing Local Project Without Remote

From inside the project:

```bash
gh repo create <github-org-or-user>/<repo-name> --private --source=. --remote=origin --push
```

Before running this:

```bash
git status
workspace-sync --dry-run
```

If `SECRET_SCAN_CMD` is configured, use `workspace-sync --commit` for interactive commit flow with a pre-commit scan.

## Existing Owned Project With Personal Remote

If the project belongs in the org, prefer GitHub repository transfer.

Alternative:

```bash
git remote rename origin personal
git remote add origin git@github.com:<github-org-or-user>/<repo-name>.git
git push -u origin <branch>
```

Do this only after confirming the old remote does not need to remain canonical.

## Upstream Or Mirror Repository

For projects like website mirrors, vendor repos, customer repos, or upstream-owned repos:

```bash
git remote -v
git remote add backup git@github.com:<github-org-or-user>/<repo-name>-mirror.git
```

Policy:

- keep `origin` as the canonical upstream
- use `backup` for private backup/mirror only
- do not push to `origin` from scheduled automation unless that is the project workflow
- do not mirror repos that contain customer data, secrets, generated dumps, or unclear ownership

## Sync Roots

`~/.config/ai-workspace/config` separates discovery from sync:

```text
PROJECT_ROOTS="$HOME/projects"
SYNC_ROOTS="$HOME/projects"
```

Use `PROJECT_ROOTS` to show projects in the picker.
Add a root to `SYNC_ROOTS` only when you are comfortable with scheduled fetch/pull/push for every repo under it.

External `origin` remotes are treated as read-mostly upstreams. `workspace-sync` may fetch/pull from them, but it pushes only to `backup`.

## Create Backup Remotes

For all repositories under `SYNC_ROOTS`:

```bash
workspace-ensure-backup --push
```

For one repository:

```bash
workspace-ensure-backup --push ~/projects/example-site
```

This creates a private repo in the configured GitHub org/account named:

```text
mirror-<source-owner>-<source-repo>
```

and adds it as:

```text
backup https://github.com/<github-org-or-user>/mirror-<source-owner>-<source-repo>.git
```

After that, scheduled sync pushes committed local state to `backup`, not to the external `origin`.
