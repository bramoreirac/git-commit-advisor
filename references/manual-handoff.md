# Human handoff in VS Code

Use this reference when the developer wants concrete next steps. These are instructions for the human, never actions for the agent.

1. In VS Code, open **Source Control** and review each changed file. Open the diff and check the exact lines that belong to the suggested commit. Watch for a file that has both staged and unstaged edits.
2. Stage only the intended files or selected change ranges/hunks using VS Code's controls. If VS Code cannot cleanly select a block, inspect the diff and finish separating the edits manually before staging. A terminal user can choose `git add -p`, but the agent must not run it.
3. Review the staged result with VS Code's staged diff or run `git diff --cached --stat` and `git diff --cached` yourself. Confirm that the commit contains its required tests and no unrelated files, secrets, local config, or accidental generated output.
4. Run the project's relevant checks, or review checks the agent has actually run. Write the commit message yourself and commit through VS Code or your terminal. Repeat for the next proposed group. Push only when you decide the local commits are ready to share.

If the project is new and has no commits, `git diff` may not show untracked files. Use Source Control or open those files to inspect them. If a file is marked ignored, it will not appear in the ordinary untracked list; review `.gitignore` and project practice before deciding whether it belongs in version control.

Ask the agent again after staging if you want a read-only second review. It can inspect the staged diff and comment on the candidate, but it still cannot stage, commit, or push.
