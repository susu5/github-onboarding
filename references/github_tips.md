# GitHub Quick Reference

## Essential Git Commands

### First-time Setup
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main   # Set default branch to main
```

### Daily Workflow
```bash
git status                        # Check what changed
git add .                         # Stage all changes
git add path/to/file              # Stage specific file
git commit -m "Your message"      # Commit with message
git push                          # Push to remote (after first push with -u)
git pull                          # Pull latest changes
```

### Working with Branches
```bash
git branch feature-name           # Create a branch
git switch feature-name           # Switch to branch (Git 2.23+)
git checkout -b feature-name      # Create and switch (older Git)
git merge feature-name            # Merge branch into current
git branch -d feature-name        # Delete merged branch
```

### Remote Repositories
```bash
git remote -v                     # List remotes
git remote add origin URL         # Add remote
git remote set-url origin URL     # Change remote URL
git clone URL                     # Clone a repository
```

### Undoing Changes
```bash
git restore file.txt              # Discard unstaged changes
git restore --staged file.txt     # Unstage a file
git revert HEAD                   # Revert last commit (safe)
git reset --soft HEAD~1           # Undo last commit, keep changes staged
```

---

## GitHub CLI (`gh`) Cheatsheet

```bash
gh auth login                     # Authenticate
gh repo create NAME --public      # Create public repo
gh repo create NAME --private     # Create private repo
gh repo clone USERNAME/REPO       # Clone a repo
gh repo view --web                # Open repo in browser
gh pr create                      # Create a pull request
gh issue list                     # List issues
```

---

## Authentication Methods Comparison

| Method | Pros | Cons |
|--------|------|------|
| GitHub CLI (`gh auth login`) | Easiest, auto-configured | Requires gh installation |
| Personal Access Token (PAT) | Works anywhere with HTTPS | Token must be stored/managed |
| SSH Key | Most secure, no token needed | Requires key generation and upload |

**Recommendation**: Use GitHub CLI for beginners; SSH for advanced users.

---

## Common Errors & Fixes

### `Please tell me who you are` (on first commit)
```bash
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```

### `src refspec main does not match any`
The local branch might be named `master`. Fix:
```bash
git branch -M main
git push -u origin main
```

### `fatal: refusing to merge unrelated histories`
Remote repo was initialized with files (README, etc.) and local is separate:
```bash
git pull origin main --allow-unrelated-histories
```

### `error: failed to push some refs`
Remote has newer commits. Rebase and push:
```bash
git pull --rebase origin main
git push
```

### `remote: Support for password authentication was removed`
Switch to token or SSH. For HTTPS with token:
- On macOS: `git credential-osxkeychain erase` then push again (it will prompt)
- Windows: Open Credential Manager → remove github.com entry → push again

---

## .gitignore Tips

- `#` starts a comment
- `*` matches any characters within a path segment
- `**` matches across path segments (e.g., `**/*.log` matches any .log file anywhere)
- `!` negates a pattern (e.g., `!important.log` un-ignores that file)
- Trailing `/` means "directory only" (e.g., `node_modules/`)
- Already-tracked files are NOT affected by .gitignore; to remove them:
  ```bash
  git rm --cached path/to/file
  ```

---

## Recommended GitHub Repository Settings

When creating a repository:
- **README.md** — Add description and usage instructions (skip if uploading existing code with README)
- **License** — Choose MIT (open) or keep empty (private/proprietary)
- **Branch protection** — For team projects, protect `main` branch from direct pushes

---

## Useful GitHub Links

| Purpose | URL |
|---------|-----|
| Sign up | https://github.com/signup |
| Create repo | https://github.com/new |
| SSH keys | https://github.com/settings/ssh/new |
| Personal Access Tokens | https://github.com/settings/tokens/new |
| GitHub CLI download | https://cli.github.com/ |
| Git download | https://git-scm.com/downloads |
