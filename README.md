# git-commit-advisor

A Codex-compatible Agent Skill for developers learning when to commit while keeping all Git writes in their own hands. The agent inspects local changes, recommends coherent commit groups, points out when to wait, and suggests messages and checks. The human uses VS Code or a terminal to stage, commit, and push.

## Use in Codex with VS Code

Copy the entire `git-commit-advisor` folder into the skill location used by your Codex setup, such as the user-level `.codex/skills/` directory or a project skill directory supported by that setup. Keep `references/` beside `SKILL.md`. Start a fresh Codex chat if the skill list was already loaded. Ask, for example:

> Use git-commit-advisor to review my current changes. Tell me whether a commit is ready and exactly what I should stage myself.

The skill contains no scripts, hooks, or automatic Git actions. It can run read-only diagnostics and relevant project checks. Its recommendations are advisory; inspect the diff yourself before every commit.

## What “atomic” means here

One commit should tell one coherent story and leave a useful project state. No maximum line count or daily commit target determines that. A feature, its tests, and required configuration may belong together; two unrelated edits in one file may need separate commits. See [example scenarios](examples/scenarios.md).

## Package contents

- `SKILL.md` — discovery metadata, safety boundary, inspection, and response workflow.
- `references/decision-guide.md` — grouping and timing judgment for mixed changes.
- `references/manual-handoff.md` — manual VS Code review and staging steps.
- `examples/scenarios.md` — lightweight behavior checks for realistic cases.
- `references/research-notes.md` — source review and design choices.

## Source and attribution

The skill text is original. The repositories surveyed are credited in [research notes](references/research-notes.md). Their approaches informed the decision criteria, but this skill intentionally does not adopt their agent-operated staging, commit, push, hook, or Gitflow enforcement workflows.
