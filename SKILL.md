---
name: github-onboarding
description: "This skill should be used when a user wants to register a GitHub account, create a new repository, or upload/push local project code to GitHub. It guides the user step-by-step through browser automation and Git CLI commands, handling registration, authentication setup, repo creation, and pushing code. Trigger phrases: 帮我注册GitHub, create a GitHub repo, upload my code to GitHub, push project to GitHub, 把我的代码上传到GitHub, 创建GitHub项目, 注册github账号"
agent_created: true
---

# GitHub Onboarding Skill

## Overview

This skill guides a user through the complete GitHub onboarding journey:
1. **Register** — Create a new GitHub account via browser automation
2. **Create** — Set up a new repository (public or private)
3. **Upload** — Initialize a local project with Git and push it to GitHub

Work through each phase in order. Skip phases the user has already completed.

---

## Phase Detection

Before starting, ask the user which phases they need:

> "需要我从哪一步开始帮你？
> 1️⃣ 注册 GitHub 账号
> 2️⃣ 创建新仓库 (repo)
> 3️⃣ 上传本地代码到 GitHub
> 还是全部流程都需要？"

Adjust the workflow to start at the correct phase.

---

## Phase 1 — Register a GitHub Account

### 1.1 Open Registration Page

Use the browser automation skill (`agent-browser`) to navigate to:
```
https://github.com/signup
```

### 1.2 Collect User Information

Before filling the form, ask the user for:
- **Email address** — must be a valid, accessible email (for verification)
- **Username** — suggest checking availability at `https://github.com/USERNAME` first
- **Password** — must be ≥15 characters or ≥8 characters with a number and lowercase letter

If the user is unsure about a username, offer 2–3 suggestions based on their name or project.

### 1.3 Fill the Signup Form

Use browser automation to:
1. Enter email → click "Continue"
2. Enter password → click "Continue"
3. Enter username → click "Continue"
4. For email preferences: choose "n" (no marketing)
5. Complete the CAPTCHA puzzle (describe it to the user and ask them to solve it manually if needed)
6. Click "Create account"

### 1.4 Email Verification

After signup, GitHub sends a verification code to the email:
1. Ask the user to open their email and find the code from GitHub
2. Enter the 8-digit code in the browser
3. Confirm the account is now active by checking `https://github.com/USERNAME`

### 1.5 Checkpoint

After registration, confirm:
> "✅ GitHub 账号注册成功！用户名：@USERNAME
> 接下来要创建一个项目仓库吗？"

---

## Phase 2 — Create a Repository

### 2.1 Check Git CLI Availability

Run: `git --version`
- If not installed: guide the user to download from `https://git-scm.com/downloads` or use `winget install Git.Git` (Windows) / `brew install git` (macOS)

### 2.2 Authenticate with GitHub

**Option A — GitHub CLI (recommended)**

Check if `gh` is available: `gh --version`

If available:
```bash
gh auth login
# Choose: GitHub.com → HTTPS → Login with a web browser
```

If not installed, offer to install:
- Windows: `winget install GitHub.cli`
- macOS: `brew install gh`
- Linux: See https://github.com/cli/cli/blob/trunk/docs/install_linux.md

**Option B — Personal Access Token (PAT)**

Guide the user to:
1. Go to `https://github.com/settings/tokens/new`
2. Set note: "WorkBuddy Upload"
3. Set expiration: 90 days
4. Check scopes: `repo`, `workflow`
5. Click "Generate token" and copy it

Store it securely — this will be used as the password for HTTPS pushes.

**Option C — SSH Key**

```bash
# Check for existing keys
ls ~/.ssh/id_ed25519.pub

# Generate a new key if needed
ssh-keygen -t ed25519 -C "your_email@example.com"

# Show the public key
cat ~/.ssh/id_ed25519.pub
```

Add the public key to GitHub:
1. `https://github.com/settings/ssh/new`
2. Title: "My Computer"
3. Paste the key → "Add SSH key"

Test: `ssh -T git@github.com`

### 2.3 Create the Repository

**Via GitHub CLI (fastest):**
```bash
gh repo create REPO_NAME --public --description "DESCRIPTION"
# Or for private:
gh repo create REPO_NAME --private --description "DESCRIPTION"
```

**Via GitHub Web UI:**
1. Navigate to `https://github.com/new`
2. Fill in:
   - Repository name
   - Description (optional)
   - Public / Private selection
   - Do NOT initialize with README if the user already has local code
3. Click "Create repository"

Ask the user:
- Repository name (suggest using the local project folder name)
- Public or private?
- Brief description (optional)

### 2.4 Checkpoint

Confirm the repo URL with the user:
> "✅ 仓库已创建：https://github.com/USERNAME/REPO_NAME
> 接下来帮你把本地代码上传上去！"

---

## Phase 3 — Upload Local Code

### 3.1 Locate the Project

Ask the user for the local project directory path if not already known.

Navigate to the directory and list its contents:
```bash
ls -la /path/to/project
```

### 3.2 Check for Existing Git Repository

```bash
git -C /path/to/project status
```

- If already a Git repo: check current remote with `git remote -v`
- If not a Git repo: initialize one

### 3.3 Initialize Git (if needed)

```bash
cd /path/to/project
git init
```

### 3.4 Create .gitignore

Run the helper script to generate an appropriate `.gitignore`:
```bash
python scripts/generate_gitignore.py /path/to/project
```

Or manually create one based on project type. Common patterns:
- **Node.js**: `node_modules/`, `.env`, `dist/`, `.DS_Store`
- **Python**: `__pycache__/`, `*.pyc`, `.venv/`, `.env`, `dist/`
- **Java/Maven**: `target/`, `*.class`, `.idea/`
- **General**: `.DS_Store`, `Thumbs.db`, `*.log`

Show the user what will be ignored and confirm.

### 3.5 Stage and Commit

```bash
cd /path/to/project
git add .
git status   # Show the user what will be committed
git commit -m "Initial commit"
```

If the user wants a custom commit message, ask for it.

### 3.6 Set Remote and Push

**HTTPS:**
```bash
git remote add origin https://github.com/USERNAME/REPO_NAME.git
git branch -M main
git push -u origin main
```

**SSH:**
```bash
git remote add origin git@github.com:USERNAME/REPO_NAME.git
git branch -M main
git push -u origin main
```

If a remote already exists but points to a different URL:
```bash
git remote set-url origin NEW_URL
```

### 3.7 Verify Upload

```bash
# Verify locally
git log --oneline -5

# Open in browser (if gh CLI available)
gh repo view --web
```

Or instruct the user to open `https://github.com/USERNAME/REPO_NAME` in a browser.

### 3.8 Success Summary

Provide a final summary:
> "🎉 代码已成功上传到 GitHub！
>
> 📁 仓库地址：https://github.com/USERNAME/REPO_NAME
> 🌿 默认分支：main
> 📦 已提交 X 个文件
>
> 下次更新代码只需运行：
> ```
> git add .
> git commit -m '你的更新说明'
> git push
> ```"

---

## Error Handling

### Authentication Failures
- `remote: Support for password authentication was removed` → Switch to PAT or SSH
- `Permission denied (publickey)` → SSH key not added to GitHub; re-run Phase 2.2 Option C
- `Repository not found` → Check remote URL with `git remote -v`; ensure repo exists

### Push Rejected
- `rejected (non-fast-forward)` → Remote has commits not in local. Run: `git pull --rebase origin main` then push again
- `error: failed to push some refs` → Same as above

### Large Files
- `error: File exceeds 100.00 MB` → Add the file to `.gitignore` or use Git LFS: `git lfs track "*.zip"`

### Username/Email Not Set
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

---

## Resources

- `scripts/generate_gitignore.py` — Detects project type and generates an appropriate `.gitignore`
- `references/github_tips.md` — Common Git commands reference and GitHub workflow tips
