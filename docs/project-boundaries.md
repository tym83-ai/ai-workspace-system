# Project Boundaries

[Русская версия](ru/project-boundaries.md)

Some projects prepare work for another repository. Keep those roles separate.

## Target Repo

A target repo owns material that can be reviewed, built, deployed, or submitted upstream:

- product code
- website source
- public docs
- reviewed content
- PR branches

It should not contain private prompts, session logs, raw AI drafts, pipeline runtime, or local agent state.

## Delivery Workbench

A delivery workbench owns the process:

- prompts and instructions
- research and briefs
- source notes and raw inputs
- generated drafts before review
- local pipeline scripts/state
- Claude/Codex agents, skills, and workflow assets

The workbench can reference one or more target repos through `workspace.yaml`.

## Default Policy

- Keep `origin` on the target repo pointed at the real upstream.
- Add `backup` under the configured GitHub org/account for automatic private backup when safe.
- Scheduled sync may push target repo state to `backup`.
- Upstream `origin` pushes and PRs require explicit user approval.
- When unsure where a file belongs, keep it in the workbench first.

## Manifest

Use this shape:

```yaml
name: example-delivery-workbench
type: delivery-workbench
owner: <github-org-or-user>

targets:
  - name: example-site
    path: ~/projects/example-site
    origin: https://github.com/example/site.git
    backup: https://github.com/<github-org-or-user>/mirror-example-site.git
    publish_policy: explicit
    role: target website repository

boundaries:
  workbench_owns:
    - prompts
    - research
    - drafts
  target_owns:
    - reviewed website content
    - public assets
```
