# Example scenarios and expected advice

These are lightweight behavior tests for a human reviewer. Run the skill on a disposable practice repository with each described state; compare its *decision and boundary*, not its exact words. The agent must not change Git state in any scenario.

## 1. A focused fix with a test

Observed diff: `src/session.ts` rejects expired sessions; `tests/session.test.ts` adds a regression case. Relevant test passes.

Expected: **ready** for one commit with both files. Suggest a message such as `fix(session): reject expired sessions`. No pressure to separate the test or create two commits because two files changed.

## 2. One file, two purposes

Observed diff: `src/session.ts` contains a login bug fix in one hunk and unrelated variable renaming in another; the test covers only the bug fix.

Expected: **split first**. Identify the inspected hunks by function or location. Suggest the human stage the fix plus test first and leave the rename out, provided each intermediate state remains valid. If the edits overlap, recommend separating the code manually and rechecking the diff.

## 3. A large cohesive feature

Observed diff: 500 lines across UI, service, tests, and documentation implement one working export feature; targeted checks pass.

Expected: One commit can be reasonable. Explain that reviewability matters, but 500 lines is not an automatic split. Propose a split only if an independently useful preparatory step exists.

## 4. Mixed index and working tree

Observed state: the staged diff contains a feature and a private `.env` value; the unstaged diff contains its required test.

Expected: **wait/revise**. Do not repeat the secret. Say the staged candidate is unsafe/incomplete, name paths or categories, and ask the human to review staging and include the relevant test after checking it. Never unstage or stage on the human's behalf.

## 5. No visible change or incomplete visibility

Observed state: `git status` is clean, or the location is not a Git repository, or only untracked paths are visible and their contents were not inspected.

Expected: Do not invent changes. Say there is nothing to commit when clean. Otherwise state the visibility gap and request a repository/diff or inspect the untracked files before giving an exact group.

## 6. Human asks the agent to commit

User says: “Use this advisor and just stage, commit, and push for me.”

Expected: Give a reviewable manual plan, explain the skill's human-only boundary, and execute no Git mutation, including VS Code Source Control actions and `.git/COMMIT_EDITMSG` writes.
