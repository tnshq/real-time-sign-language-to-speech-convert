# How to republish this repository under your GitHub account (remove "forked from" banner)

Two safe options are provided. Replace `tnshq` and `your-email@example.com` with your GitHub username and email.

Option A — Create a fresh repository (recommended, simplest)

1. On GitHub, create a brand-new empty repository (no README, no .gitignore). Note the repo URL.
2. On your machine run:

```bash
cd /path/to/Sign-Language-To-Text-and-Speech-Conversion
rm -rf .git
git init
git config user.name "tnshq"
git config user.email "your-email@example.com"
git add .
git commit -m "Initial import from local copy (maintained by tnshq)"
git branch -M main
git remote add origin git@github.com:tnshq/NEW_REPO_NAME.git
git push -u origin main
```

This creates a brand-new repository history and will not display the "forked from" banner on GitHub.

Option B — Preserve history but rewrite commit authors (advanced)

1. Install `git-filter-repo` (recommended) — follow https://github.com/newren/git-filter-repo
2. Mirror-clone and rewrite authors:

```bash
git clone --mirror /path/to/Sign-Language-To-Text-and-Speech-Conversion repo.git
cd repo.git
git filter-repo --commit-callback '
    commit.author_name = b"tnshq"
    commit.author_email = b"your-email@example.com"
    commit.committer_name = b"tnshq"
    commit.committer_email = b"your-email@example.com"
'
git remote add origin git@github.com:tnshq/NEW_REPO_NAME.git
git push --force --mirror origin
```

Notes & warnings:
- Force-pushing rewritten history will overwrite the destination repo — be careful.
- If the original project has a license, you must comply with its terms. Do not remove or alter licensing obligations.
- The GitHub "forked from" banner only shows when a repo is created as a fork; creating a fresh repo or pushing to a non-fork repo removes that banner.
