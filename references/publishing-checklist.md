---
name: git-repo-publishing
description: "Checklist for publishing a local folder as a new GitHub repo."
version: 1.0.0
author: Everlily Nexus
license: MIT
---

# Git Repo Publishing Checklist

## Pre-flight
- [ ] `git status` clean (or intentional staged changes only)
- [ ] No unpushed commits ahead of remote
- [ ] `gh auth status` shows logged in with `repo` scope
- [ ] Target repo name available on GitHub

## Init (if not already a repo)
- [ ] `git init && git branch -M main`
- [ ] `.gitignore` excludes embedded sub-repos, backups, logs, caches

## Commit & Push
- [ ] `git add .` (sub-repos excluded by .gitignore)
- [ ] `git commit -m "..."`
- [ ] `gh repo create <name> --description "..." --public --source=. --remote=origin --push`

## Verify
- [ ] `git remote -v` shows `origin https://github.com/<user>/<repo>.git`
- [ ] `git status` clean
- [ ] Repo URL returned by `gh`

## Do NOT
- [ ] Do not `git add` folders that already have their own `.git` (creates empty gitlinks)
- [ ] Do not run `gh repo create --push` with unpushed local commits
- [ ] Do not publish backups/, logs/, or cache directories
