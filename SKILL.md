---
name: startup
description: "Connect the current project folder to a GitHub repository. Use when the user asks to set up GitHub for a project, connect the current folder to GitHub, or check its existing GitHub remote."
---

# Startup

Connect only the current project folder to GitHub. Do not perform unrelated project setup.

## Workflow

1. Check whether the current folder is already a Git repository with `git rev-parse --is-inside-work-tree`.
2. If it is a Git repository, inspect all remotes with `git remote -v`.
3. Determine whether any existing remote points to GitHub.
   - If connected, report the repository name and URL and stop without changing anything.
   - Do not assume the remote is named `origin`.
4. If the folder is not connected to GitHub, ask the user to choose one option:
   - Create a new GitHub repository.
   - Connect an existing GitHub repository.
5. Collect only the details required by the selected option:
   - New repository: repository name, visibility, and owner or organization when ambiguous.
   - Existing repository: its GitHub URL or `owner/repository` identifier.
6. Initialize Git locally with `git init` only when the current folder is not already a Git repository.
7. Connect the selected repository without replacing any existing remote:
   - Prefer the remote name `origin` when it is unused.
   - If `origin` already exists, explain the conflict and ask before changing it, or offer a different remote name.
   - When creating a repository with GitHub CLI, do not use `--push`.
8. Verify the connection with `git remote get-url <remote-name>` and report the remote name, repository name, and URL.

## Rules

- Never overwrite, rename, or remove an existing Git remote without explicit permission.
- Never create a commit, stage files, or push files without explicit permission.
- Never modify project source code.
- Never perform unrelated setup.
- Treat creating a GitHub repository as an external write: show the intended repository name, owner, and visibility before creating it.
- If GitHub authentication is unavailable, report that login is required and stop instead of attempting to bypass it.
