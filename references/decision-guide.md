# Commit boundary decision guide

Use this guide when changes are mixed, dependent, or difficult to split. It is a judgment aid, not a quota.

## Candidate questions

1. Can the purpose be described in one short sentence without hiding a second independent change?
2. Would reverting this candidate later remove one intended behavior or maintenance step?
3. Does it include everything needed to understand and verify that purpose, including relevant tests and docs?
4. Would the project remain usable after this commit, before the next one?
5. Are unrelated edits and local artifacts excluded?

If the answer to 1 or 2 is no, propose a split. If 3 or 4 is no, combine dependent changes or finish the work before committing. If 5 is no, have the human review the selection before staging.

## Patterns that affect grouping

| Situation | Likely recommendation | Reason |
| --- | --- | --- |
| Bug fix plus regression test | One commit | The test proves the fix. |
| New feature plus required config and documentation | Often one commit | They form one working behavior. |
| Behavior-preserving rename followed by a feature | Two commits if each is independently valid | The preparatory change can be reviewed and reverted separately. |
| Formatting across many files plus a focused fix | Separate or defer formatting | Formatting hides the behavior diff. |
| Two unrelated fixes in one file | Split by hunks if safe | A file is not a commit boundary. |
| Separate issue tickets that require one working change | Usually one commit | Ticket count alone is not a boundary. |
| Large mechanical migration | One or more commits by coherent migration step | Line count is a review signal, not a maximum. |
| Generated files or lockfile updates | Include only when the project deliberately tracks them and they support the same change | Their size alone should not drive a split. |

Dependencies matter more than category labels. Do not prescribe “tests last” if that would leave a feature commit unverified. Avoid claiming every intermediate commit must be deployable in every repository; use the project's contribution and release practice, and explain any incomplete checkpoint.

## Mixed staged and unstaged work

`git diff --cached` shows the current candidate for the next commit. `git diff` shows unstaged edits. A path can appear in both. Say which side contains the recommended change. If staged content already mixes concerns, flag that the human must review and adjust staging before committing. Do not run the adjustment.

For a mixed file, map recommendations to observed function names, line neighborhoods, or hunk headers. Mention VS Code's stage selected ranges only if selection can preserve valid code. If a hunk combines intertwined edits, recommend a manual code separation and a fresh diff review. Never encourage hunk staging that would create a syntax error or leave a test pointing at behavior not yet present.

## Timing and checks

Commit after a coherent step is complete and reviewable, not at a fixed interval. Pause when the change is only an experiment, the relevant test fails, or the diff contains something unexpected. A clean `git diff --check` only rules out some whitespace problems. Prefer the narrowest relevant test first, then broader checks if project policy or risk requires them. If a check was skipped for a reason, state that reason without implying it passed.

## Message advice

Follow local conventions first. If the repository uses Conventional Commits, propose `type(scope): imperative summary`, with scope omitted when unhelpful. `feat`, `fix`, `refactor`, `docs`, `test`, and `chore` describe intent, not file extensions. The message should reflect the whole proposed commit, not a single incidental file. Use a body when the reason or trade-off is not evident. Do not invent an issue number or a breaking-change marker.
