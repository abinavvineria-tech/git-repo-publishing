---
name: git-repo-publishing
description: "Use when publishing a local folder as a new GitHub repo."
version: 1.0.0
author: Everlily Nexus
license: MIT
platforms: [linux]
metadata:
  hermes:
    tags: [Git, GitHub, Repository, Publishing]
    related_skills: [hermes-agent-engineer, termux-linux-engineer]
---

# Git Repo Publishing

Publishes a local folder/workspace as a new public GitHub repository using `gh`, with correct handling of embedded git sub-repos and backup directories.

## Activation

Reference by name: `git-repo-publishing`.

## When to use

- User asks to "setup github" or "publish this folder" to GitHub.
- A local folder needs to become a new GitHub repo (not already tracked remotely).

## Procedure

1. Check current git state in the target folder: `git status`, `git remote -v`, `git log --oneline -5`.
   - If already a clean repo with matching remote and all commits pushed → already published; report and stop.
2. Check `gh auth status` to confirm GitHub CLI is logged in with `repo` scope.
3. Initialize the repo if needed: `git init && git branch -M main`.
4. Create a `.gitignore` that excludes embedded sub-repos (each is its own published repo) and non-publishable dirs (backups, logs, caches).
   - Do not `git add` embedded git repositories — they appear as gitlinks and won't carry their contents into clones.
5. Stage intended files: `git add .` (after `.gitignore` is in place).
6. Commit: `git commit -m "chore: initialize <workspace> repo\n\n- brief description"`.
7. Create and push: `gh repo create <repo-name> --description "..." --public --source=. --remote=origin --push`.
8. Verify: `git remote -v` shows origin; `git status` is clean; repo URL is returned by `gh`.

## Pitfalls

- Embedded sub-repo: `git add` of a folder that already has `.git` stages it as a gitlink (empty pointer). Clones of the outer repo won't contain the sub-repo's contents. Exclude sub-repos in `.gitignore` instead.
- Working tree not clean or commits not pushed: `gh repo create --push` will fail. Ensure `git status` is clean and all local commits are on the branch being pushed.
- `gh` not authenticated: run `gh auth login` first and confirm `repo` token scope.

## Example

```
User: "setup github too publish our new folder"
Everlily: Initialize cru-projects/ as a git repo, add .gitignore excluding CookieRunners-Everlasting-Kingdom/ and backups/, commit, then `gh repo create cru-projects --public --source=. --remote=origin --push`.
```